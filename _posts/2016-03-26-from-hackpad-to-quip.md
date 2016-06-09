---
layout: post
title: '第十四篇 - 從 Hackpad 搬家到 Quip'
date: 2016-03-26 04:25
comments: true
categories: 
---
你知道，Hackpad 自從[被 Dropbox 買走之後][1]就沒增加新功能了，從 15 年四月[收到信說要開源][2]，也是過了很久的[八月][3]才完成。當然技術公司的整並進去必定要做一堆的交接、磨合、修改成能開源的版本；不過十二月時又有 Dropbox 的[前車之鑑][4]，一路看下來我們還是多做幾個備份好 :p

Quip 的好就不用多說了，由 Facebook 前 CTO Bret Taylor [創立][5]，最近才在 15 年十月拿到 [30M 的 B 輪投資金][6]，結合 [React Native C++ Binding][7] 強大技術力的 Desktop Client，近幾個月來也[持續在 UI、功能][8]上優化，前途真是一片光明啊。

把 Hackpad 轉移到 Quip 確實是個不錯的決定（鋪陳這麼久就為了這句XD），加上 Quip 官方也非常 Nice 的提供此功能：
<blockquote class="twitter-tweet" data-lang="zh-tw"><p lang="en" dir="ltr">Some of our users have asked us for a <a href="https://twitter.com/hackpad">@Hackpad</a> import tool, and we wanted to share it with you. Check it out! <a href="https://t.co/VstwZVEiLb">https://t.co/VstwZVEiLb</a></p>&mdash; Quip (@quip) <a href="https://twitter.com/quip/status/475767564383961088">2014年6月8日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Well, 早在 14 年吶。雖然 Dropbox Paper（前 Hackpad）也有[提供導入功能][9]啦，不過還要寫信到客服信箱，產品變你們家的竟然還沒有無痛一鍵導入是哪招啦 XDD。於是我的 Quip 搬家行動是勢在必行了，以下就略述這個從 hackpad 搬家到 quip 的過程。

<!--more-->

首先進入網頁版的 quip，點選右上的新增文件，選擇 Import、Hackpad：
![Screen Shot 2016-03-26 at 12.49.26 PM.png](http://user-image.logdown.io/user/1128/blog/1112/post/682044/fQIA64NTvGEeUis5Kor4_Screen%20Shot%202016-03-26%20at%2012.49.26%20PM.png)
![Screen Shot 2016-03-26 at 12.49.33 PM.png](http://user-image.logdown.io/user/1128/blog/1112/post/682044/xfyJX8pgRryigqZyf1GL_Screen%20Shot%202016-03-26%20at%2012.49.33%20PM.png)

填入 workspace id，還有依照連結點進去看到的 Client ID 和 API Key：
![Screen Shot 2016-03-26 at 12.49.43 PM.png](http://user-image.logdown.io/user/1128/blog/1112/post/682044/s6j3UXgnSkOgGrjhHJxS_Screen%20Shot%202016-03-26%20at%2012.49.43%20PM.png)
![Screen Shot 2016-03-26 at 12.50.13 PM.png](http://user-image.logdown.io/user/1128/blog/1112/post/682044/75bxVjF0Sc6RSyb5JhCh_Screen%20Shot%202016-03-26%20at%2012.50.13%20PM.png)

點開始匯入到單一 Folder 就完成了，但如果你是 workspace 的 admin，你會發現到，**有些在 workspace 不是由你建立的 pads 是不會被匯入的**，到 Hackpad 的 workspace management 調整 Quip Importer 的權限：

![](http://i.imgur.com/13MF60X.png)
![](http://i.imgur.com/hcLssW5.png)
由 Guest 調整成 Member。

再等一會兒，就會看到你的 Hackpad 們都匯入到 Quip 資料夾中囉。
![](http://i.imgur.com/NgGR1vi.png)
（完）

[1]: https://hackpad.com/Hackpad-is-teaming-up-with-Dropbox-m1Fne5A6Lzn
[2]: https://github.com/hackpad/hackpad/issues/1
[3]: http://venturebeat.com/2015/08/21/dropbox-finally-open-sources-its-hackpad-collaborative-document-editor/
[4]: http://venturebeat.com/2015/08/21/dropbox-finally-open-sources-its-hackpad-collaborative-document-editor/
[5]: https://en.wikipedia.org/wiki/Quip
[6]: https://www.crunchbase.com/organization/quip/funding-rounds
[7]: https://medium.com/@btaylor/react-with-c-building-the-quip-mac-and-windows-apps-c63155c1531b#.cmtg0ajpc
[8]: https://quip.com/blog/
[9]: https://paper.dropbox.com/doc/Dropbox-Paper-beta-Is-Ready-For-You-Xlvlkb3yI23hLvuZxvn2Q