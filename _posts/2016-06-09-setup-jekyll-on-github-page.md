---
layout: post
title: 在 GitHub 上設定 Jekyll 和自訂網域
date: '2016-06-09 00:30:54 +0800'
categories: jekyll setup github git subtree
comments: true
published: true
---

花了時間把 Jekyll 建起來。Ghost 超讚，不過要自訂版型、部屬還是比較花時間的（部屬還要 $$），純粹靜態網頁更為容易，還可以直接上到免費的 GitHub Pages 上，再配上 `jekyll-assets`，寫 sass 或 ES6 都 OK。

## 切出 Git Subtree

我先把原本的分支重新命名為 `jekyll`，再用 git subtree 把 Jekyll 建置出的 `_site` 資料夾切成 master 分支：

```bash
git checkout -b jekyll # 原本在 master
git branch -D master # 把原本 master branch 刪掉
git subtree split --prefix=_site -b master # git subtree 指令將 _site 資料夾切成 master branch
```

接下來每次更新網站都從原本的 `jekyll` 分支 commit，然後推送 subtree 到 GitHub Page 的 remote：

```bash
git commit -m "commit on jekyll branch"
git subtree push --prefix _site origin master # 將 _site 的 subtree 推到 origin 的 master branch
```

其中我的 `origin` 為 `https://github.com/username/username.github.io`。請再參考 [GitHub Pages](https://pages.github.com/) 的設定。

## 設定 GitHub User Page 的自訂網域

> 二圖流

![](/assets/img/blog/cloudflare1.png)

![](/assets/img/blog/cloudflare2.png)

`CNAME` 檔：

```
www.yukaii.tw
yukaii.tw
```

## 自訂 Template

Jekyll 也有 partial，我用來做 image caption，比如以下這段：

```txt
{% raw %}
{% include image.html url="http://i.imgur.com/gqOPCwP.jpg" description="六月不要啊啊啊，應該沒有第二行啦" %}
{% endraw %}
```

就會產生：

{% include image.html url="http://i.imgur.com/gqOPCwP.jpg" description="六月不要啊啊啊，應該沒有第二行啦" %}

在 `_includes` 資料夾新增 `image.html`，詳情請[參考這裡](http://stackoverflow.com/questions/19331362/using-an-image-caption-in-markdown-jekyll)
