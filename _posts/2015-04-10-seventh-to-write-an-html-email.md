---
layout: post
title: '第七篇 - 寫一封 html 的 email'
date: 2015-04-10 15:46
comments: true
categories:
---

## Prerequisite

一些簡單的 html/css 知識

## 流程

Email 裏 css 有些東西沒法用，所以需要測試在每一個 client 的效果。

完整的支援表[請見此](https://www.campaignmonitor.com/css/)。

我的需求簡單的滿版就能搞定了，參考了[問一](http://blog.winwu.today/)的建議，儘量用 border / padding 來推，並沒有花很久的時間。

先在 `<head>` 寫完 `<style>` ，呈現大致的成果：

<!--more-->

<!-- ![螢幕快照 2015-04-10 下午11.59.39.png](http://user-image.logdown.io/user/1128/blog/1112/post/259954/6bXmF4hCTcyDK252gezU_%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202015-04-10%20%E4%B8%8B%E5%8D%8811.59.39.png) -->

## 測試寄信

使用 [Putsmail](https://putsmail.com)

![螢幕快照 2015-04-11 上午12.02.00.png](http://user-image.logdown.io/user/1128/blog/1112/post/259954/AyQVfGvSwa49GwCN77If_%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202015-04-11%20%E4%B8%8A%E5%8D%8812.02.00.png)

填上測試的 email 和 html，按送出就可以了，記得把 inliner 打勾。

![螢幕快照 2015-04-11 上午12.03.05.png](http://user-image.logdown.io/user/1128/blog/1112/post/259954/dI1CyNk7QCKtMebw6k6k_%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202015-04-11%20%E4%B8%8A%E5%8D%8812.03.05.png)


他也有提供 inliner 服務：

![螢幕快照 2015-04-11 上午12.03.27.png](http://user-image.logdown.io/user/1128/blog/1112/post/259954/XdZTL7TRDS1QOHAp35pu_%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%202015-04-11%20%E4%B8%8A%E5%8D%8812.03.27.png)

## 其他資料

* [Email寄信-css/html 排版,跑版問題](http://blog.winwu.today/2013/03/email-csshtml.html)
* [Ideas Behind Responsive Emails](https://css-tricks.com/ideas-behind-responsive-emails/)
* [Mastering HTML Email](http://webdesign.tutsplus.com/series/mastering-html-email--webdesign-17696)
* [Preview in Gmail, Hotmail, AOL, and Yahoo](http://info.contactology.com/email-view)

## 結論

此篇大抵就是資料的再整理與截圖。

看完吉米丘寫的[R.I.P. 臉書上的一切精采好內容](http://iphone4.tw/forums/showthread.php?t=215978)之後覺得還是要來寫一下 Blog 比較好啊 XDD

以上。
