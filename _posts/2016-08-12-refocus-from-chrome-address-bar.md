---
layout: post
title: 使用 Chrome 在快速鍵切換至網址列後，再用快速鍵切換回來
comments: true
published: true
---

## 啥？

在 Chrome 要把焦點切到網址列（omnibox）進行搜尋時，有組合鍵：<kbd>⌘</kbd>/<kbd>Ctrl</kbd> + <kbd>L</kbd> 可以用。但 Chrome 卻沒有快速鍵能夠把焦點設回網頁，非得要用滑鼠或觸控板點一下，這對 Vim 插件（[Vimium][vimium]、[cvim][cvim]）使用者來說實在太不舒適了，要知道雙手不用離開鍵盤才是**真正的舒適圈**啊，而擴充套件的作用範圍也不包含網址列，經過一番 Google 後我終於找到一個 Hacky 的解法。

(**更新**：改自 Chromium 的 Opera 按 <kbd>ESC</kbd> 鍵就有同樣效果了，不需要再改啦！在此推薦 :p)

## 建立自訂搜尋引擎

在設定頁點開「搜尋引擎」設定：

![search-engine](https://i.imgur.com/qmkSYvX.png)

在最底下新增一個搜尋引擎，三個欄位分別填入

* `Refocus` （自訂搜尋引擎名稱）
* `i` （要綁定的關鍵字，越短越好）
* `javascript:`

如下圖：

![add-search-engine](https://i.imgur.com/J5d9Kec.png)

現在在焦點切到網址列後，只要輸入綁定的關鍵字（比如我綁的是 `i`）、再按下 enter，就能把焦點切回頁面囉：

![address-bar](https://i.imgur.com/tgxysRe.png)

## 參考資料

* [Google Chrome (Mac) set keyboard focus from address bar back to page - Super User](http://superuser.com/questions/324266/google-chrome-mac-set-keyboard-focus-from-address-bar-back-to-page/324267#324267)

[vimium]: https://github.com/philc/vimium/
[cvim]: https://github.com/1995eaton/chromium-vim
