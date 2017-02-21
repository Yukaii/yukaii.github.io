---
layout: post
title: "用 restore-source-tree 套件來還原網站原始碼"
---

## 前提是在 Production 環境下有 sourcemap 可以拿辣！

本篇內容也不多，大概就是介紹一下思路。

前幾天看到這篇 [Dissecting Twitter's Redux Store](https://medium.com/statuscode/dissecting-twitters-redux-store-d7280b62c6b1) 覺得十分優雅，今天猛地想起某國內網站也是用 React/Redux 弄的，便依樣畫葫蘆，想從 redux store 裡看出些端倪。

打開 Chrome Devtool，不開還好，一開便發現他們的 sourcemap 在 production 竟然拿得到啊 😂，於是就來找了 `restore-source-tree` 套件。

由於該套件會把 webpack 產生的註解給移掉，而新版 webpack 註解格式又和舊版不太一樣，如下：

```javascript
const WEBPACK_FOOTER = '/** WEBPACK FOOTER **'; // 舊版註解

const WEBPACK_FOOTER = '// WEBPACK FOOTER //'; // 新版註解
```

會造成該套件解析不出原始碼，不對啊，DevTool 明明就看得到。

更麻煩的，Extract css plugin 產生出來的註解又和新版的不太一樣（搞啥啊），所以我在 [`74b32d`](https://github.com/Yukaii/restore-source-tree/commit/74b32d5beb73f28393a2a6fa152ba8a5e2633907) 這個 commit，乾脆就把移除註解的方法拿掉了 XD

若大家想要使用我修改過版本的 `restore-source-tree` 請直接在終端機打上以下指令安裝吧：

```bash
npm install -g Yukaii/restore-source-tree#98ccfc2
```

## 然後呢？

比如說某站 production 的 js bundle file 長這樣：

```bash
https://xxxx.com/build/bundle-54fbd34f.js
```

**如果**有 sourcemap 的話，檔案結尾註解部分會長的像這樣：

```javascript
//# sourceMappingURL=bundle-54fbd34f.js.map
```

那我們就把 sourcemap 載下來：

```bash
curl https://xxxx.com/build/bundle-54fbd34f.js.map > bundle-54fbd34f.js.map
```

然後用剛剛裝好的 `restore-source-tree` 還原原始碼：

```bash
restore-source-tree -n --out-dir <dir> bundle-54fbd34f.js.map

# -n 參數是包含 node_modules 資料夾
# --out-dir 參數後面加要存進的目錄
# 請參考原專案 github 說明：https://github.com/alexkuz/restore-source-tree
```

登愣，就好啦！（好了是長怎樣？不告訴你）

## 欸嘿

看到這邊就知道，其實我們要逆向操作網站的開源也是可以的，可以寫一個 cronjob，定時把 sourcemap 抓下來，如果 bundle hash  有更新的話，就重新還原一次並 commit 進某 repo。

當我沒說啦(X)

（完）
