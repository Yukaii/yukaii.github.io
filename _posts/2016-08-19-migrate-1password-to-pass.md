---
layout: post
title: "從 1Password 搬家到 pass"
---

## Why?

pass 照[官網][1]的說法，是一套 「標準 unix 密碼管理」（the standard unix password manager），開源簡單好用。雖然 1Password 我是用 Dropbox 同步，應該還算安全，不過多個開源選擇也不錯。

pass 也有一些社群版套件，引用一下官網：

> The community has even produced a [cross-platform GUI client](http://qtpass.org/), an [Android app](https://github.com/zeapo/Android-Password-Store), an [iOS app](https://github.com/davidjb/pass-ios#readme), a [Firefox plugin](https://github.com/jvenant/passff#readme), a [Windows client](https://github.com/mbos/Pass4Win), a pretty [Python QML app](https://github.com/TheLastProject/Pext), a nice [Go GUI app](https://github.com/cortex/gopass),  an [interactive console UI](https://github.com/Kwpolska/upass), Alfred integration [(1)](https://github.com/CGenie/alfred-pass) [(2)](https://github.com/MatthewWest/pass-alfred) [(3)](https://github.com/johanthoren/simple-pass-alfred), a [dmenu script](https://git.zx2c4.com/password-store/tree/contrib/dmenu), [OS X integration](https://git.zx2c4.com/password-store/tree/contrib/pass.applescript), [git credential integration](https://github.com/languitar/pass-git-helper), and even an [emacs package](https://git.zx2c4.com/password-store/tree/contrib/emacs).


不過 iOS App 版要 jailbreak 之後才能使用呀 QQ，不過身為一個還算熟悉 CLI 的使用者，這些當然都不是問題啊，哼☹。

## 安裝 pass

OS X 安裝十分容易：

```bash
brew install pass
```

就好了。他也提供一些 shell completion 可以使用，`fish` 需要在 `~/.config/fish/config.fish` 裡加上：

```bash
source /usr/local/share/fish/vendor_completions.d/pass.fish
```

## 設定 pass

完成之後我們要看一下有沒有 gpg 金鑰，如果沒有的話要新增個

```bash
gpg --gen-key # 產生 gpg key
```

記得輸入的 key phrase 呀，之後會拿來解鎖的。用 `gpg --list-keys` 查看剛剛產生的 key，類似下面這樣：

```bash
/Users/username/.gnupg/pubring.gpg
-------------------------------
pub   2048R/A534B400 2016-08-18
uid                  My Name <your_email@gmail.com>
sub   2048R/E1001945 2016-08-18
```

pubic id 就是 `A534B400`，拿這組 id 來初始化 pass

```bash
pass init A534B400
```

完成！之後就可以用 pass 指令管理密碼了，可以下一些測試的指令試試，比如 insert 一組 foo 的密碼：

```bash
pass insert foo
```

然後顯示 foo 的密碼

```bash
pass show foo
```

## 由 1Password 匯入

官網上就寫上匯入 1password 的 [ruby script][2] 😍 ，直接使用就行了。在 1Password 匯出 txt 時，記得勾選 `Include Column Labels`，如下圖：

![1password-export-option](http://i.imgur.com/YsoUQcv.png)

把匯出的檔案和匯入腳本準備好，跑一下：

```
ruby 1password2pass.rb /path/to/1password_exported.txt
```

## 基礎使用

pass 的管理可以說是相當自由，通通放在 `~/.password-store` 目錄，底下可以建立任意的目錄分類，或是乾脆不分 XD

一些基本指令：

```bash
pass show PROFILE # 印出 PROFILE 的密碼
pass -c PROFILE # 複製 PROFILE 的密碼到剪貼簿
pass ls # 列出所有密碼設定檔
```

我做了以下 alias，方便快速密碼搜尋：

```bash
alias passgrep="pass ls | grep -i"
```

就可以用 `passgrep` 來搜尋現有設定檔了。

[1]: https://www.passwordstore.org/
[2]: https://git.zx2c4.com/password-store/tree/contrib/importers/1password2pass.rb
