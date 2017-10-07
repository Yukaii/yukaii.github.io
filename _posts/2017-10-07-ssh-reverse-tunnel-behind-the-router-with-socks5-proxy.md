---
layout: post
title: 用 ssh tunnel 連線到 router 背後的機器，並建立 SOCKS5 proxy 存取內網資源
published: true
---

## 這役男實在是...

記一下最近遇到的問題。

目前小弟正在公家單位服役。凡公家機關，內部架個外包的管理系統也是很合理的，不過因為是內部系統，所以只能透過區網來存取。對於我這個用自己機器的人來說就不太方便啦，連線的 Wifi Router 又不能和內網相通。

剛好單位有台閒置的虛擬機，也可以存取內網，Google 了幾個方案之後發現 ssh tunnel 的影響最小：不用裝些有的沒的，也不用更改網路環境，加上我已經有了台 Linode 上的遠端開發機常駐，就來試試吧 =w=

以下是環境的簡易說明圖。「那臺機器」就是閒置的內網虛擬機，「開發機器」是常駐 Linode 的遠端開發機，「Client 機」是任一台可以連線到開發機的裝置，可能是 Macbook Air、iPad，或是路邊撿來裝上 putty 的 Windows（逃）

```txt
                          +
                          |
                          |
                   ssh with reverse port
+---------------+  forwarding            +---------------+
|               +------------------------>               |
|   那臺機器     <------------------------+    開發機器    <-----+ mosh + tmux
|               |   ssh to localhost via |               |     |   掛上去
|   (ubuntu)    |   forwarded port       |   (Linode)    |     |
+---------------+         |              +---------------+     |
                          |                                    +-------------+
                          |                                    |             |
                          |                                    |             |
                          |                                    |  Client 機  |
                          |                                    |             |
                          |                                    |             |
                          +                                    +-------------+

                一堆 router 和防火牆等等

```

## ssh remote port forwarding

先在 Ubuntu 上跑 ssh 指令連到遠端開發機，再加上 Remote port forwarding 參數：

```bash
# run on machine behind router
ssh -NfR 11111:localhost:22 REMOTE_SERVER_IP
```

可以看到 ssh 有三個參數，分別為：

* `-N`: 單純連線，不送指令
* `-f`: 放到背景執行
* `-R`: Remote port forwarding，遠端埠重導

上面指令的作用是，建立 ssh tunnel，讓遠端機器的 `11111` port 連回本機的 port 22 (localhost:22)。

所以接下來在遠端機跑：

```bash
# run on remote machine
ssh localhost -p 11111
```

就可以 ssh 回 router 背後的機器啦。

## SOCKS5 Proxy

接下來我們修改一點指令，加上 dynamic port forwarding 參數 `-D`：

```bash
# run on remote machine
ssh -ND 0.0.0.0:8080 localhost -p 11111
```

另一個參數 `-N` 讓指令純建立 tunnel，`0.0.0.0` 讓所有 IP 都能連到這臺機器，`8080` 是綁定的 port。`-p` 指令連接埠，也就是方才 forward 過來的 `11111`。

這樣 SOCKS5 Proxy 就算跑起來了，最後來設定 Client 端。我另外開了一個 Chrome 的 Profile，裝上 [Proxy SwitchyOmega][1] 插件，方便隔離：

![image](https://i.imgur.com/kOjwrdw.png)

Remote IP 就是遠端開發機的 IP。Bypass List 要檢查一下，`192.168.x.x` 也要 forward，記得從 List 刪掉。

然後就可以在外網存取 `192.168.X.X` 的區網資源啦！太舒適啦！

## 參考資料

* [Accessing home services from anywhere, without port forwarding!][2] - 用 tinc 做 VPN，感覺更佳舒適
* [ssh through a router without port forwarding][3] - SuperUser 日常爬文

[1]: https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif
[2]: https://jordancrawford.kiwi/home-server-without-portforward/
[3]: https://superuser.com/questions/595989/ssh-through-a-router-without-port-forwarding
