---
layout: post
title: '第一篇 - Linux 開機卡在 Checking battery status........[ok]'
date: 2014-03-22 10:39
comments: true
categories: [ubuntu, problem, cannnot, boot, driver, video_card, ElementaryOS]
---
## 屁話

其實從我接觸*ubuntu*以來，各種與windows的戰爭就一直沒停過XD

各種比較表啦，無痛轉換平台教學，把Windows習慣帶到linux上之類的，還是看鳥哥卡實在。

究竟誰比較好，<del>當然是OSX樂勝兩者啦</del>，不過現在我是用[Elementary OS](http://elementaryos.org/)，算是把linux的圖形介面提升至一個新的高度，而不只是各種華麗的炫技(雖然也是有，畢竟特效為必要之惡)，骨子裡還是ubuntu 12.04，而且安裝完佔用空間超小der<del>因為一堆套件都沒裝orz</del>。

最近又看到朋友轉貼一則推坑*ubuntu*的懶人包，看到穩定度的描述，只能報以微笑啊呵呵，大家是用來架站又不是日常使用。剛好看到ubuntu正體中文站論壇的這篇[抒發一下....接觸了六年的Ubuntu](http://www.ubuntu-tw.org/modules/newbb/viewtopic.php?topic_id=79494)，於我心有戚戚焉。

## 原因

我的狀況是驅動問題，我這台老筆電的顯卡`HD4330`跑起來倒也順暢。下午裝了幾個套件，重開個機就掛了...各種相衝啊。<del>一定是桌布不夠宅，怒換</del>

## 解決

`ctrl`+`alt`+`f1`登入，下指令移除fglrx

		sudo apt-get remove --purge fglrx fglrx-amdcccle

然後重新開機

		sudo reboot

就完成了。

via [checking battery state / lightdm problem?](http://elementaryos.org/answers/checking-battery-state-lightdm-problem)
