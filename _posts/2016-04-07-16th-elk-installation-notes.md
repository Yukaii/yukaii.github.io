---
layout: post
title: 第十六篇 - ELK 安裝筆記
date: '2016-04-07 13:43'
comments: true
categories:
  - Elasticsearch
  - Logstash
  - Kibana
  - 安裝魂
  - 就只是篇筆記
published: true
---

這份其實是學校作業（逃）。雖然說是作業，但也還算有分享的價值，就在此整理出來。因為主力機 MBA 的儲存空間快炸了，所以我在 AWS 上開了一台 `t2.medium` 的機器來跑 Spark 以及 ELK Stack。在課堂中， ELK 是以下載 binary zip 包的方式設定，因為小弟習慣用 APT 套件管理程式，所以裝 ELK 相關設定方式也和 binary zip 不太一樣。

安裝過程主要參考[數位海的這篇][1]，還有上課簡報。數位海這篇超詳細的，可以交互對照一下。

[1]: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04

## 安裝 Java 8

```bash
sudo add-apt-repository -y ppa:webupd8team/java
sudo apt-get update
sudo apt-get -y install oracle-java8-installer
```

## 安裝 ElasticSearch

```bash
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
sudo apt-get update
sudo apt-get -y install elasticsearch
```

到這邊裝完，然後編輯 ElasticSearch 設定檔：

```bash
sudo vi /etc/elasticsearch/elasticsearch.yml
```

把 network.host 改成 `0.0.0.0`，讓外網也可以存取。

![](https://i.imgur.com/dlKuGDm.png)

> 註：之後還會搭上 nginx 的 reverse proxy，並裝上前端 kibana，這裡為了安全性也可以不要改。

重啟 elasticsearch 的服務：

```bash
sudo service elasticsearch restart
```

讓它開機時自動啟動（Optional）

```bash
sudo update-rc.d elasticsearch defaults 95 10
```

ElasticSearch binary 檔的位置在 `/usr/share/elasticserach/bin`。

安裝 head plugin：

```bash
sudo /usr/share/elasticserach/bin/plugin install mobz/elasticsearch-head
```

把 iptable 打開

```bash
sudo iptables -A INPUT -m tcp -p tcp --dport 9200 -j ACCEPT
sudo iptables -A INPUT -m udp -p udp --dport 9200 -j ACCEPT
```

記得在 EC2 打開 9200 port，這時候開啟瀏覽器，進到

```bash
http://YOUR_IP_ADDRESS:9200/_plugin/head
```

就有一個簡單的圖形界面啦。

## 安裝 Kibana

```bash
echo "deb http://packages.elastic.co/kibana/4.4/debian stable main" | sudo tee -a /etc/apt/sources.list.d/kibana-4.4.x.list
sudo apt-get update
sudo apt-get -y install kibana
```

到這裡裝完，然後編輯 kibana 的設定檔：

```bash
sudo vi /opt/kibana/config/kibana.yml
```

把 server.host 從 `0.0.0.0` 改成 `localhost`，因為稍後會裝 nginx 來做我們的反向代理：

```bash
server.host: "localhost"
```

加入開機啟動，然後啟動服務：

```bash
sudo update-rc.d kibana defaults 96 9
sudo service kibana start
```

安裝 nginx 啥的

```bash
sudo apt-get install nginx apache2-utils
```

建立一個 kibanaadmin 的認證使用者（名稱可換）

```bash
sudo htpasswd -c /etc/nginx/htpasswd.users kibanaadmin
```

編輯 nginx 設定檔

```bash
sudo vi /etc/nginx/sites-available/default
```

換成下面這個

```nginx
server {
    listen 80;

    server_name example.com;

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

`example.com` 的地方，看你要換成 Instance 的 IP 或是 domain name 都可以。EC2 可以換成 public dns name。

重啟 nginx

```bash
sudo service nginx restart
```

## 安裝 Logstash

```bash
echo 'deb http://packages.elastic.co/logstash/2.2/debian stable main' | sudo tee /etc/apt/sources.list.d/logstash-2.2.x.list
sudo apt-get update
sudo apt-get install logstash
```

中間數位海插了一段設定 SSL 的，因為我沒有要用就跳過這部分。

Logstash 安裝完的 binary 會在 `/opt/logstash/bin` 。

## 設定 Logstash

```bash
sudo vim /etc/logstash/conf.d/02-twitter.conf
```

填入以下內容

```logstash
input {
    twitter {
        consumer_key => ""
        consumer_secret => ""
        oauth_token => ""
        oauth_token_secret => ""
        keywords => ["beauty"]
        languages => ["en"]
        full_tweet => true
    }

}
output {
    elasticsearch {
        index => "twitter"
    }
}
```

oauth 的 key 就填自己申請的，keywords 填自己要的，然後存檔離開。

接下來跑

```bash
sudo service logstash configtest
```

看一下設定檔有沒有寫錯， 然後重啟 logstash 服務，並加入開機自啟動(Optional)

```bash
sudo service logstash restart
sudo update-rc.d logstash defaults 96 9
```

原則上這時候登入 kibana 或是在 head 儀表板就可以看到記錄數一直增多了 :p

## 匯出資料

我用了 [elasticsearch-dump](https://github.com/taskrabbit/elasticsearch-dump) 套件，記得先裝好 node v1.0 以上版本。

```bash
npm install elasticdump -g # 安裝
elasticdump --input=http://localhost:9200/twitter --output=twitter.json # 一鍵匯出
```

## 上傳

之前 Github 推了 LFS(Large File Storage)，json 雖然純文字檔很好壓縮，但無論怎麽壓還是有點難推進到 100 MB ，也就是 Github 支援的最大容量，以內。勢必就要來用下 git-lfs 了。

![](https://i.imgur.com/ypwyyN4.png)

## 匯出的資料格式

搜尋的關鍵字是 beauty，你知道 twitter 上最多這種圖帳了，多訂閱幾個總是使人心情愉悅 #)

```json
{
  "_index": "twitter", // 灌入到 elasticsaerch 裡的資料表
  "_type": "logs",
  "_id": "AVPCY9McFvHLlvNymVV9", // elasticearch 產生的 id
  "_score": 1,
  "_source": {
    "created_at": "Tue Mar 29 12:40:34 +0000 2016", // 建立於
    "id": 714794377491587100, // twitter 的 tweets id
    "id_str": "714794377491587072",
    "text": "Regina King On The 'Fascinating' Beauty Of Black Women's Hair https://t.co/0838uXkQ1e @HannahOliver152",
    "source": "<a href=\"http://dlvr.it\" rel=\"nofollow\">dlvr.it</a>",
    "truncated": false,
    "in_reply_to_status_id": null,
    "in_reply_to_status_id_str": null,
    "in_reply_to_user_id": null,
    "in_reply_to_user_id_str": null,
    "in_reply_to_screen_name": null,
    "user": { // twitter user 的資料
      "id": 2558258431, // user id
      "id_str": "2558258431",
      "name": "Hannah Oliver", //名字
      "screen_name": "HannahOliver152", //帳號名
      "location": null,
      "url": "http://www.wordpress.com",
      "description": "Our life always expresses the result of our dominant thoughts.",
      "protected": false,
      "verified": false,
      "followers_count": 31,
      "friends_count": 11,
      "listed_count": 6,
      "favourites_count": 0,
      "statuses_count": 17481,
      "created_at": "Tue Jun 10 05:18:02 +0000 2014",
      "utc_offset": null,
      "time_zone": null,
      "geo_enabled": false,
      "lang": "en",
      "contributors_enabled": false,
      "is_translator": false,
      "profile_background_color": "C0DEED",
      "profile_background_image_url": "http://abs.twimg.com/images/themes/theme1/bg.png",
      "profile_background_image_url_https": "https://abs.twimg.com/images/themes/theme1/bg.png",
      "profile_background_tile": false,
      "profile_link_color": "0084B4",
      "profile_sidebar_border_color": "C0DEED",
      "profile_sidebar_fill_color": "DDEEF6",
      "profile_text_color": "333333",
      "profile_use_background_image": true,
      "profile_image_url": "http://pbs.twimg.com/profile_images/476232749041201152/yWKGqAkC_normal.jpeg",
      "profile_image_url_https": "https://pbs.twimg.com/profile_images/476232749041201152/yWKGqAkC_normal.jpeg",
      "profile_banner_url": "https://pbs.twimg.com/profile_banners/2558258431/1402377722",
      "default_profile": true,
      "default_profile_image": false,
      "following": null,
      "follow_request_sent": null,
      "notifications": null
    },
    "geo": null,
    "coordinates": null,
    "place": null,
    "contributors": null,
    "is_quote_status": false,
    "retweet_count": 0,
    "favorite_count": 0,
    "entities": {
      "hashtags": [],
      "urls": [
        {
          "url": "https://t.co/0838uXkQ1e",
          "expanded_url": "http://dlvr.it/Kw7QcG",
          "display_url": "dlvr.it/Kw7QcG",
          "indices": [
            62,
            85
          ]
        }
      ],
      "user_mentions": [
        {
          "screen_name": "HannahOliver152",
          "name": "Hannah Oliver",
          "id": 2558258431,
          "id_str": "2558258431",
          "indices": [
            86,
            102
          ]
        }
      ],
      "symbols": []
    },
    "favorited": false,
    "retweeted": false,
    "possibly_sensitive": false,
    "filter_level": "low",
    "lang": "en",
    "timestamp_ms": "1459255234488",
    "@version": "1",
    "@timestamp": "2016-03-29T12:40:34.000Z"
  },
  "fields": {
    "@timestamp": [
      1459255234000
    ]
  }
}
```

（完）

[1]: https://github.com/Yukaii/CS5124701
[2]: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04
