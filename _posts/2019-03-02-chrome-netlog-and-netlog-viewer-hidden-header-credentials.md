---
layout: post
title: 使用 Chrome NetLog 解析隱藏在 DevTools 中的 Header 資訊
date: 2019-03-02 21:30 +0800
---

在檢視 DevTools 網路頁的 Headers 資訊時，有時後會顯示下面這行警告訊息：

![](https://i.imgur.com/P7WZcYY.png)

> Provisional headers are shown

這就代表有些敏感資訊被 DevTools 隱藏。舉例來說，CloudFront 有一套 [基於 Signed Cookie 的驗證系統][1]，某些需要登入後才能存取的資源，比如 **電子書**、影音就需要用這種方式來防止外流。不過有做 DRM 還是比較好啦，畢竟沒 DRM 你的檔案還是跟裸奔一樣啊！😆

## 該是 NetLog 登場的時刻

NetLog 顧名思義，是一套 Chromium Project 內建的網路事件記錄系統 (Network Logging System)，因為我們需要拿取被 DevTools 隱藏的資訊，所以就要靠 NetLog 啦。有 Log 才能 Debug！

1. 打開 [`chrome://net-export/`](chrome://net-export/)，你應該會看到以下的介面
    ![](https://i.imgur.com/8qFazRw.png)
2. 記得選擇第二項 **Include cookies and credentials**，這才是我們要的
3. 點按鈕，選擇完 dump 位置後開始記錄網路事件。開始進行你要 Log 的操作，此時分頁開越少越好，輸出資料的雜訊不會那麼多比較好讀

## 用 NetLog Viewer 來打開 dump 出的 log 檔

那麼輸出的 JSON 檔要怎麼讀？Google 也幫你弄好了。

1. 打開 [NetLog Viewer][2]
2. 把剛剛輸出的 JSON 檔案，拖弋到視窗
    ![](https://i.imgur.com/oJj7teQ.png)
3. Bang!
    ![](https://i.imgur.com/0K3spmz.png)
4. 點開左側的 Event，搜尋框輸入你要找的東西就可以了
    ![](https://i.imgur.com/pPYkL1f.png)
5. 剩下來就不說啦，搜尋你要的 Request 或 Cookie 鍵值，拉出你要的資訊，就和使用 DevTools 一樣

## 嗯

所以說，你要解析的敏感資訊是什麼呢？

光喜自得！

[1]: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-setting-signed-cookie-custom-policy.html
[2]: https://netlog-viewer.appspot.com
