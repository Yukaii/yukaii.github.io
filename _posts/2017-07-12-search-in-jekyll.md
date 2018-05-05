---
layout: post
title: Jekyll 的文章搜尋功能
---

![Imgur](https://i.imgur.com/3y5O9tZ.gif)

前幾天在 HackerNews 上看到了這篇文章：[A search widget for static web sites](https://news.ycombinator.com/item?id=14717182)，原理就是抓取 RSS 的 xml 檔案當做搜尋的資料庫。

既然都用 Jekyll 了，那產生一個更容易使用的格式，比如 JSON，就不用手寫 regex 去 match XML 啦。

我直接用了 [Jekyll search using lunr.js](https://learn.cloudcannon.com/jekyll/jekyll-search-using-lunr-js/) 這篇文章的做法，在 JavaScript 塞入文章內容：

{% raw %}

```javascript
var store = {
  {% for post in include.posts %}
    "{{ post.url | slugify }}": {
      "title": "{{ post.title | xml_escape }}",
      "url": "{{ post.url | xml_escape }}",
      "author": "{{ post.author | xml_escape }}",
      "category": "{{ post.category | xml_escape }}",
      "content": {{ post.content | strip_html | strip_newlines | jsonify }},
      "url": "{{ post.url | xml_escape }}",
      "date": "{{ post.date | date: "%b %-d, %Y" }}"
    }
    {% unless forloop.last %},{% endunless %}
  {% endfor %}
}
```

{% endraw %}

產生出來的檔案長這樣：

```javascript
var store = {
  "blog-2017-06-28-windows-unix-like-development-guide-nodejs-ruby": {
    "title": "Windows Unix-Like 環境設定踩坑紀錄",
    "url": "/blog/2017/06/28/windows-unix-like-development-guide-nodejs-ruby/",
    "author": "",
    "category": "",
    "content": "...",
    "url": "/blog/2017/06/28/windows-unix-like-development-guide-nodejs-ruby/",
    "date": "Jun 28, 2017"
  },
  "blog-2017-05-04-deploy-redmine-with-puma-nginx-migration-from-mysql-to-postgresql": {
    "title": "使用 puma 和 nginx 部屬 Redmine（加上從 MySQL 搬到 PostgreSQL）",
    "url": "/blog/2017/05/04/deploy-redmine-with-puma-nginx-migration-from-mysql-to-postgresql/",
    "author": "",
    "category": "",
    "content": "...",
    "url": "/blog/2017/05/04/deploy-redmine-with-puma-nginx-migration-from-mysql-to-postgresql/",
    "date": "May 4, 2017"
  },
  // ...
}
```

然後用 `lunr.js` 做搜尋：

```javascript
var idx = lunr(function () {
  this.field('id');
  this.field('title', { boost: 10 });
  this.field('author');
  this.field('category');
  this.field('content');
  this.field('url');
  this.field('date');

  for (var key in store) { // Add the data to lunr
    this.add({
      'id': key,
      'title': store[key].title,
      'author': store[key].author,
      'category': store[key].category,
      'content': store[key].content,
      'url': store[key].url,
      'date': store[key].date,
    });
  }
});

var results = idx.search(searchTerm); // Get lunr to perform a search
```

再把結果產生成列表就行了。花了比較多時間在處裡產生的 store 資料上，比如 `jekyll-mention` 插件會把 `@` 開頭的轉換成 GitHub User 連結，以及 `jemoji` 插件會把 `:smile:` 轉成 GitHub 的 emoji 圖片連結，但在：

```txt
post.content | strip_html | strip_newlines | jsonify
```

這個轉換 pipeline 裡面沒法被消除掉，於是轉出來的 JavaScript 就會出現 SyntaxError（多換行啦、double quote 沒有 escape 等）。

另一個問題是因為用到 HTML5 History API，跟 Turbolink 尬在一起就會炸。現在有時還會「回到上一頁但頁面不切換」之類的問題。就暫時放置 Play 了 XD

更多請參考 commit [`bb3a006`](https://github.com/Yukaii/Blog/commit/bb3a006f690f1ceed8793f9fa0b950f6c043bca9) 囉。

（完）
