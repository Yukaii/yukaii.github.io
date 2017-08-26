---
layout: post
title: 使用 puma 和 nginx 部屬 Redmine（加上從 MySQL 搬到 PostgreSQL）
published: true
---

前陣子看了幾篇 Linode 安利文，每月五鎂的機器好像就 Linode CP 值最高惹，管理界面雖然看起來陽春，使用起來倒挺有效率的。這幾天就把擺在 Vultr 上的自用服務 (Redmine，幾個 crontab 腳本) 搬到了 Linode 上。

除了單純的搬家外，部屬的 Redmine 也做了一些調整：

- 3.2 到 3.3
- 資料庫由 MySQL 轉換到 PostgreSQL
- Application server 由 passenger 換成 puma
- 版控由官方 svn 換成 git mirror，方便操作 XD

總之本篇除了是我的搬家筆記，還可以視為**自架 Redmine 指南**（大概啦）。

## 設定新伺服器

直接在 Linode 東京開了台 Ubuntu 的 Instance，然後 ssh 打密碼連進去。

### 解決 apt-get update 卡住問題

請參考 [apt-get update stuck: Connecting to security.ubuntu.com](https://askubuntu.com/a/787491)

### 裝 Dependency

首先當然就把系統的相依套件裝一裝，包含 tmux、postgres 資料庫等等。滿建議學習一下 tmux 的。可以開兩個 tab，一個 root 權限，一個一般權限，方便許多，不用一直切 su。

```bash
apt-get update && apt-get upgrade && apt-get install -y git build-essential tmux postgresql libpq-dev # database
apt-get install -y libssl-dev libreadline-dev zlib1g-dev # ruby dependency
apt-get install -y imagemagick libmagickcore-dev libmagickwand-dev # redmine dependency
```

### 建立 deploy 用的使用者

我不希望直接用 root 帳號跑起 Redmine 服務。總之就是安全性疑慮啦 XD。直接打 `adduser` 走完過程：

```bash
adduser deploy
```

### 安裝 Redmine

#### 下載原始碼

切到 `deploy` 帳號，把 redmine clone 下來，並把設定檔弄好：

```bash
su deploy
cd ~ # 切到家目錄
git clone https://github.com/redmine/redmine

cd redmine
git checkout -b 3.3-stable origin/3.3-stable

cp config/database.yml.example config/database.yml
cp config/configuration.yml.example config/configuration.yml
```

#### 建立 Ruby 環境

使用 [rbenv](https://github.com/rbenv/rbenv) 裝 ruby，直接照著步驟跑：

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile # linode 上是 .bashrc
~/.rbenv/bin/rbenv init # 貼上輸出到 .bash_profile 或 .bashrc
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build # 裝 ruby build
```

裝 ruby：

```bash
rbenv install 2.3.4
rbenv global 2.3.4
echo 'gem: --no-ri --no-rdoc' >> ~/.gemrc
gem install bundler
```

#### 設定資料庫

用好棒棒的 [`pgcli`](https://github.com/dbcli/pgcli) 與本機 postgres 建立連線。上面 APT 套件裝完後應該就會自己跑起來了。

```bash
apt-get install pgcli
sudo -i -u postgres
pgcli
```

進去以後，建立使用者和資料庫：

```sql
CREATE USER redmine WITH PASSWORD 'PASSWORD';
CREATE DATABASE redmine;
GRANT ALL PRIVILEGES ON DATABASE redmine to redmine;
```

##### 測試連線(Optional)

以防密碼打錯這種奇妙事件發生，我們可以測試一下連線。打開 `/etc/postgresql/9.5/main/pg_hba.conf`，找到下面加入這行：

```txt
local   all             redmine                                 md5
```

這行開啟本地端的帳密授權登入。然後用 `psql` 連線本機測試：

```bash
psql -U redmine -d redmine -W
```

成功連進去就 Ok 啦。

#### 設定 Redmine 資料庫

1. 編輯 `config/database.yml` 填入資料庫帳密，把 `mysql` 相關的都註解掉(development 等等)
1. 然後安裝 redmine 的 gem dependency

```bash
bundle install --without development test
```

##### Migrate from old redmine (Optinoal)

複製以下檔案：

- `config/initializers/secret_token.rb`
- `config/database.yml`
- `config/configuration.yml`

從 MySQL 搬家到 Postgres 的部分，因為都是同一個 App，Redmine 版本也只是小升沒有大型 migration，我直接利用[`yaml_db`](https://github.com/yamldb/yaml_db) 套件，把資料庫 dump 出來成 yaml，然後在 load 就行了 😍 可以參考 [`yaml_db`](https://github.com/yamldb/yaml_db) 套件 GitHub 說明。

先跑 `RAILS_ENV=production bundle exec rake db:setup` 建立資料庫，然後在 `bundle exec rake db:data:load` 把檔案載入進來。

## 設定 Puma

在 Gemfile 加上 `gem 'puma'` 一行，然後跑 `bundle install --without development test` 安裝。再來編輯 `config/puma.rb` 貼上以下內容：

```ruby
#!/usr/bin/env puma

application_path = '/home/deploy/redmine'
directory application_path
environment 'production'
# daemonize true
pidfile "#{application_path}/tmp/pids/redmine.pid"
state_path "#{application_path}/tmp/pids/redmine.state"
stdout_redirect "#{application_path}/log/redmine.stdout.log", "#{application_path}/log/redmine.stderr.log"
bind "unix://#{application_path}/tmp/sockets/redmine.sock"
```

記得建立 `pids` 和 `sockets` 資料夾：

```bash
cd ~/redmine
mkdir tmp/sockets tmp/pids
```

可以用以下指令試跑 Server：

```bash
RAILS_ENV=production bundle exec puma -C config/puma.rb
```

## 設定 systemctl 自啟動

用 root 權限編輯 `/etc/systemd/system/redmine.service`：

```Ini
[Unit]
Description=Rails-Puma Webserver

[Service]
Type=simple
User=deploy
WorkingDirectory=/home/deploy/redmine
ExecStart=/bin/bash -lc 'bundle exec puma -C config/puma.rb'
TimeoutSec=15
Restart=always

[Install]
WantedBy=multi-user.target
```

跑起來：

```bash
systemd daemon-reload # 重新載入設定檔
systemd enable redmine.service # 設為開機啟動
systemd start redmine.service # 啟動
systemd status redmine.service # 看狀態
```

## 把 nginx 跟 puma socket server 接起來

編輯 `/etc/nginx/sites-available/redmine`：

```nginx
upstream app {
        server unix:/home/deploy/redmine/tmp/sockets/redmine.sock fail_timeout=0;
}

server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name localhost;
        root /home/deploy/redmine/public;
        try_files $uri/index.html $uri @app;

        location @app {
                proxy_pass http://app;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_redirect off;
        }

        error_page 500 502 503 504 /500.html;
        client_max_body_size 4G;
        keepalive_timeout 10;
}
```

記得 soft link 回 `/etc/nginx/sites-enabled/`底下：

```bash
cd /etc/nginx/sites-enabled
ln -s ../sites-available/redmine .
```

重啟 `nginx`：

```bash
service nginx restart
service nginx status
```

打開瀏覽器連 IP 應該就能看到辣！

## 目前的 Plugin 列表

- [redmine_emojibutton](www.redmine.org/plugins/redmine_emojibutton): Emoji 萬歲！
- [redmine\_markdown\_task\_list](https://github.com/eichisanden/redmine_markdown_task_list): Checklist
- [redmine_mentions](http://www.redmine.org/plugins/redmine-mentions): Tag 人寄 email

## References

* https://gist.github.com/velenux/6883dc221a7d2eae7dcb
* https://gist.github.com/jbradach/6ee5842e5e2543d59adb for config/puma.rb
* https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-puma-and-nginx-on-ubuntu-14-04
* https://github.com/puma/puma/blob/master/docs/systemd.md
* https://github.com/puma/puma/blob/master/docs/nginx.md
* https://github.com/yamldb/yaml_db
