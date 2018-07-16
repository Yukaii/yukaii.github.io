---
layout: post
title: Chrome Extension - Kabegami New Page
date: 2018-07-16 20:30 +0800
---

## 先看 Demo

![Imgur](https://i.imgur.com/CW9l7iA.gif)

**TL;DR**，這是一個讓瀏覽器新分頁**每次隨機顯示桌布**的擴充功能。

瀏覽器成為第二個桌面，我自幹這個功能也是相當合理的 =w=

## 到哪下載？

[這裡](https://chrome.google.com/webstore/detail/kabegami-new-page/fbbdincgjgdmbbkongmineooghpadbgk)

[![Chrome Web Store](https://img.shields.io/chrome-web-store/v/fbbdincgjgdmbbkongmineooghpadbgk.svg)](https://chrome.google.com/webstore/detail/kabegami-new-page/fbbdincgjgdmbbkongmineooghpadbgk)

## 技術架構

- 好久沒玩的 Vue.js，做點小玩意兒剛剛好
- JS 好朋友 TypeScript，沒這個真的不知怎麽寫前端了
- Imgur API，流量無限我他媽還不傳爆！

值得一提的是這次用 TypeScript Generics 搭配 localforage 弄了一層簡單的 [ActiveModel-like API](https://github.com/Yukaii/kanahei-wallpapers/blob/master/src/app/lib/store.ts#L26)，莫名的成就感啊 😂

## Show me the code

按照慣例一向開源開爆的，GitHub repo 在 [https://github.com/Yukaii/kanahei-wallpapers](https://github.com/Yukaii/kanahei-wallpapers)。為何名字裡有 kanahei？原因是一開始純粹想做他們家限定的桌布，沒想到回饋不錯只好一般化，讓大家可以上傳自己想用的桌布。之後可能會改名吧，再說。
