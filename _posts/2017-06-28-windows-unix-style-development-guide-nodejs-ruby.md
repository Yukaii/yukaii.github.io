---
layout: post
title: Windows Unix-Like 環境設定踩坑紀錄
---

## TL;DR

Hyper-V + Ubuntu Server + Samba + WSL ssh + cmder

## 緣起

自從印度人上臺之後，感覺微軟真的變了，從邪惡帝國主義（？）變成開源大法好，可惜基於歷史因素，社群圈講到微軟多半是酸居多，唉。至少從我這個 JavaScript 開發者兼 VSCode 愛好者來看，微軟真的進步超多了，害我都想拋棄 macOS、買台 Surface 跳槽 Windows 10 試試了。

本文就是在購買 Surface 前的開發前置準備。如果買了 Surface 之後，整個開發 workflow 都要砍掉重練那也是滿累的 QQ。剛好最近回老家，就拿桌機的 Windows 10 來開刀吧！(至於我怎麼把 1G 的可用硬碟空間拉到 60G 那又是另一個故事了)

## WSL - Windows Subsystem for Linux (Bash on Windows)

在 Creators Update 之後，WSL 的能用度更高了，還是在來試驗了一下做 node.js 的開發揪竟會如何。

* 存在 ulimit file descriptor 2048 限制 [BashOnWindows#1688](https://github.com/Microsoft/BashOnWindows/issues/1688)、[BashOnWindows#1576](https://github.com/Microsoft/BashOnWindows/issues/1576) 等。比如用 `copy-webpack-plugin` 就會噴錯，不過也有 `graceful-fs` 能解
* 檔案系統還無法完美互通。官方文件是這樣說的：**DO NOT,** __**under ANY circumstances**__ **, create and/or modify Linux files using Windows apps, tools, scripts, consoles, etc.**。我遇到因為在 WSL 的 git 與 windows 的 git 交互使用，造成檔案遺失無法存取的狀況
* 效能比想像中差，裝套件花的時間比想像中久(感覺上啦，沒真的測時間，Fall Creators Update 應該還會再提升)
* 不太適合新手

雖然開發程式跑得起來，但叫我都要用文字介面的 git？我才不要 XD 於是電梯向下，繼續找下一套替代方案。

雖然 WSL 有一些雷，可是 ssh、vim、tmux mosh 一系列工具已經很熟了，而且在 WSL 裡面沒有大問題(好歹我沒遇到啦)，作為「原生 ssh 客戶端」還是非常夠用的 w。

## Vagrant

Vagrant 可以說是虛擬機開發懶人方案，用 ruby 寫設定檔，幫你把資料夾、網路共享、port forwarding 一次搞定。

Vagrant 預設是用 virtualbox，也有 vmware 或 hyper-v 可以選用。就要另外找壓好的 vagrant box。因為 Hyper-V 似乎比 VirtualBox 的效能還要好，所以特別找了[基於 hyper-v 的 image](https://app.vagrantup.com/jjworren/boxes/xenial64) 來用，無奈就是跑不起來。看了一下命令列的輸出，發現他的目錄共享也是透過 samba 來弄。那就來自己搞吧！

## Hyper-V

Windows 內建虛擬化方案，可以隨開關機自動啟動停止，完全無縫整合進 Windows 裡面！不過還不知道對續航力有啥影響，請期待 Surface 開箱文(誤)

我直接裝上了 ubuntu server 16.04 版，反正桌面環境就用 Windows 10 的，UWP 大好。先建立一個虛擬交換器，之後日常照著裝完就好了。

### 設定 Samba

在設定的時候一直在想，到底要 **Windows 開共享資料夾讓 Ubuntu 掛載** 還是 **Ubuntu 開共享讓 Windows 掛載**呢？結論：**Ubuntu 共享讓 Windows 連線**，以 Node.JS 開發環境來說，`node_modules` 安裝的時候就要處理一堆 symlink 了，而 NTFS 不支援啊，所以前者一定爆的...（會這麼說是因為我在這卡很久 orz）

設定過程參考了以下資料：

* http://www.arthurtoday.com/2015/04/ubuntu-server-share-folder-samba.html
* http://flykof.pixnet.net/blog/post/23028119
* https://wsgzao.github.io/post/samba/

還有一點是要把建立的 samba user 加到目前使用者的 Group 底下，要不然每次都要 `chown` 很麻煩：

```bash
sudo usermod -G GROUP_NAME smbuser
```

![drive](http://i.imgur.com/SJYhYwJ.png)

自此專案資料夾便能無痛掛載到 Windows 內，由 Windows 內的 IDE 或編輯器存取啦！

## cmder

說到 Windows 平台下好用的 Terminal 大家都會推 cmder，就是 ConEMU 加上 Git for Windows 一整套幫你裝好了，直接上截圖。字體是我愛用的 [mononoki](https://github.com/madmalik/mononoki)。

![cmder](http://i.imgur.com/VHM4USs.png)

可惜的是還沒有滑鼠支援，大家可以到 [ConEmu#1114](https://github.com/Maximus5/ConEmu/issues/1114) 追蹤。

## Git

* filemode 要設為 false，不然權限會跑掉

## 結論

說實話有點高估 WSL 的能耐了 XD 如果在輕量級開發(多輕量？我也不知道)之下，體驗說不定真的很好。

剩下來的設定檔全面移轉、AHK Scripting、各式輔助軟體就等 Surface 開箱之後再說吧 XDD

![VSCode](http://i.imgur.com/G09U4RQ.png)

本篇文章就是在 Windows 下的 VSCode 編輯 Markdown、配上 Hyper-V 下的 Ubuntu 使用 Jekyll 生成的。讚讚 :heart:

（完）
