---
layout: post
title: "使用 Chrome 在快速鍵切換至網址列後，再用快速鍵切換回來"
---

## 啥？

習慣於鍵盤操作 Chrome，比如 [Vimium][vimium] 或是 [cvim][cvim] 等擴充套件，在需要跳轉網址列進行搜尋時，都會很自然的使用 Chrome 的內建快速鍵：`CMD + L`（在 Windows 是 F6）來操作。

那回來呢？我想要在切回網頁本身，再次用熟悉的 VIM key binding 來衝浪。

很遺憾的，Chrome 本身並沒有提供此功能，而擴充套件的作用域也不包含網址列，在經過 Google 之後終於找到一個 Hacky 的解法。

## 建立自訂搜尋引擎

在設定找「搜尋引擎」：

![search-engine](http://i.imgur.com/qmkSYvX.png)

在最底下新增一個搜尋引擎，三個欄位分別填入

```bash
Refocus # 自訂搜尋引擎名稱
i # 要綁定的關鍵字，越短越好
javascript:
```

如下圖：

![add-search-engine](http://i.imgur.com/J5d9Kec.png)

接下來在切換到網址列之後，主要輸入綁定的關鍵字，再按下 enter，就能切換回網頁囉：

![address-bar](http://i.imgur.com/z36JQpo.png)


via [Google Chrome (Mac) set keyboard focus from address bar back to page - Super User](http://superuser.com/questions/324266/google-chrome-mac-set-keyboard-focus-from-address-bar-back-to-page/324267#324267)

[vimium]: https://github.com/philc/vimium/
[cvim]: https://github.com/1995eaton/chromium-vim
