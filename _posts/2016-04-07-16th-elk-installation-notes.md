---
layout: post
title: '第十六篇 - ELK 安裝筆記'
date: 2016-04-07 13:43
comments: true
categories: [Elasticsearch, Logstash, Kibana, 安裝魂, 就只是篇筆記]
---
雖然說是筆記啦，實際上就是把[數位海的文章][2]照著打一打就做完了（暈）這其實是一門課的作業，附上 [Github][1] 上的簡單說明：

<blockquote>
此份 Repo 是 118 104 第二學年度，CS5124701 巨量資料與分析的課堂作業一。作業一的要求就是寫一隻應用到 Twitter 或是 Github 的爬蟲，並把爬下的資料格式化成 json。在 3/29 的這週，教了 ELK Stack 設定方法，所以 Crawler 程式並沒有多做特別設定，直接套用課堂簡報上的 Logstash Twitter 樣板。

<br/><br/>

以下記錄一下 ELK 安裝筆記，與課堂上略有不同的部分。說明一下，因為主力機 MBA 的儲存空間快炸了，所以我在 AWS 上開了一台 t2.medium 的機器當做跑 Spark 以及 ELK Stack 的平臺。在上週的課程簡報中 ELK 幾乎都是以下載 binary zip 包的方式設定，因為習慣用 apt 之流等套件管理程式，裝 ELK 相關設定檔也跟 binary zip 不太一樣。
</blockquote>

就是我龜毛所以要特別租一台機器操作。說到這，最近 Google Cloud Platform(GCP) 一註冊就送三百鎂的額度可以用，很讚，早知道就直接裝在上面了，幹嘛買啥 EC2 XDDD。詳情就請移駕到小弟 [Github][1]。


[1]: https://github.com/Yukaii/CS5124701
[2]: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04