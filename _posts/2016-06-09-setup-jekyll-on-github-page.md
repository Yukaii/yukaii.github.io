---
layout: post
title:  "在 GitHub 上設定 Jekyll 和自訂網域"
date:   2016-06-09 00:30:54 +0800
categories: jekyll setup github git subtree
---

花了時間把 Jekyll 建起來。Ghost 是很棒沒錯，不過想要完全掌控還要多花許多時間，純粹靜態網頁更為靈活，再配上 `jekyll-assets`，如果真的需要點 ES6 還是可以硬上的。

## Git Subtree

我把原始的 branch 命名為 `jekyll`，用 subtree 把 Jekyll 產生的 `_site` 資料夾切開：

```shell
git checkout -b jekyll # 原本在 master
git branch -D master # 把原本 master branch 刪掉
git subtree split --prefix=_site -b master # subtree 切割
```



到這算切割完畢，接下來每次更新時，都從原本 `jekyll` branch commit，然後 push subtree 的 branch：

```shell
git commit -m "commit on jekyll branch"
git subtree push --prefix _site origin master # 將 _site 的 subtree 推到 origin 的 master branch
```



## GitHub user page custom domain

> 二圖流

{{ 'blog/cloudflare1.png' | img }}
{{ 'blog/cloudflare2.png' | img }}

`CNAME` 檔：

```
www.yukaii.tw
yukaii.tw
```



## 其它自訂

Jekyll 也有 partial 挺方便的，我用來做 image caption，比如以下這段：

```txt
{% raw %}
{% include image.html url="http://i.imgur.com/gqOPCwP.jpg" description="六月不要啊啊啊，應該沒有第二行啦" %}
{% endraw %}
```

就會產生：

{% include image.html url="http://i.imgur.com/gqOPCwP.jpg" description="六月不要啊啊啊，應該沒有第二行啦" %}

需要再 `_includes` 資料夾新增 `image.html`，詳情可[參考這裡](http://stackoverflow.com/questions/19331362/using-an-image-caption-in-markdown-jekyll)
