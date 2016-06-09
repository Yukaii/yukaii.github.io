---
layout: post
title: '第三篇 - Synergy造成Windows Explorer Crash'
date: 2014-05-04 14:30
comments: true
categories: [Windows7, 安安, 裝個Windows也這麼煩, Synergy, bugs, explorer, crash]
---
![explorer_crash](http://www.geekersmagazine.com/wp-content/uploads/2011/01/windows-explorer-has-stopped-working.gif)

# 問題

在重裝Windows後，裝了一些常用工具並且重新啟動，發現Windows Explorer掛了。
點選任何一個icon，包括開始選單的應用程式捷徑，都會造成Windows Explorer崩潰並重啟。

# 解法

打開「檢視所有問題報告」，並Google之，結果是Synergy的Shell Extension，貌似就這最新版(1.4.17)有這問題。比較簡單的方法就解除安裝，並安裝前一個版本(1.4.16)，再不然：

![shellexfix.png](http://user-image.logdown.io/user/1128/blog/1112/post/197087/wX1Rjv0xTTKPJihmNjK3_shellexfix.png)

下載[ShellExView](www.nirsoft.net/utils/shexview.html‎)，開啟之後停用Synergy的Shell Extension。

以上。
