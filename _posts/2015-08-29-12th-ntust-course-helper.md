---
layout: post
title: '第十二篇 - 台科課程匯入小幫手'
date: 2015-08-29 13:44
comments: true
categories: [呵呵好久沒發文了, 你說我最近在幹嘛, 不告訴你, 呵呵, 似乎真有點忙]
---
[台科課程匯入小幫手](http://school-agent.herokuapp.com/)

## 屁話

這幾天串了 118 的學生資訊系統、 Colorgy 的 API 整合了 Table，做出了無痛匯入課程的小工具，算是開了 Colorgy 第三方 App 的第一槍 =w=（自以為）

直接從學生資訊系統匯入課程這功能， [NTU Sweety Course](http://ntusweety.herokuapp.com/) 和 [NTHU Course](http://nthu-course.cf/)、[NCTU+](http://plus.nctu.edu.tw/) 都有類似的實作，112/113 的版本似乎直接串學校，就不知道是官方還非官方了XD。當然一提到校園模擬選課就必須拜一下碼頭，[CrossLink](https://www.crosslink.tw/)。

找回一個玩樂心態來弄這個小玩意，沒啥特別計劃，時機到累積完就做出來了 Q_Q，感謝捏神支援。

照慣例把玩具放在 Heroku，說到他們最近政策也滿妙的，之前一直寄信說每天只能開機十八小時，現在過了 8/16 了也都照常使用，41J 也沒碰上過啥問題，還沒付過 heroku 錢欸 XD，It's the future.


Gihub: [https://github.com/Yukaii/SchoolAgent](https://github.com/Yukaii/SchoolAgent)
<del>Repo 原始英文超中二的 XDDD，不忍直視，請不要去翻 commit 穴穴尼</del>

## 一些介面

<!--more-->

![1](http://user-image.logdown.io/user/1128/blog/1112/post/293646/G8b1uJywRMeCJRa6OVmy_FireShot%20Capture%20-%20%E8%AA%B2%E7%A8%8B%E5%8C%AF%E5%85%A5%E5%B0%8F%E5%B9%AB%E6%89%8B%20-%20http___school-agent.herokuapp.com__logout=true.png)

![2](http://user-image.logdown.io/user/1128/blog/1112/post/293646/Q2VZBHYmRECUvZEgSpE4_FireShot%20Capture%20-%20%E8%AA%B2%E7%A8%8B%E5%8C%AF%E5%85%A5%E5%B0%8F%E5%B9%AB%E6%89%8B%20-%20http___school-agent.herokuapp.com_import.png)

![3](http://user-image.logdown.io/user/1128/blog/1112/post/293646/2aQjwQQCQHyEOE2Xmf5Z_FireShot%20Capture%20-%20%E8%AA%B2%E7%A8%8B%E5%8C%AF%E5%85%A5%E5%B0%8F%E5%B9%AB%E6%89%8B%20-%20http___school-agent.herokuapp.com_result.png)

完全的 mobile first，也懶的弄 RWD 了 XD

## 尾刀

雖然這邊沒啥人看，不過就只發在這裡了，等待有緣人發現啦w。有一天這功能大概會被默默整合進 Table 裡，呵。

還有要告解的地方嗎？
