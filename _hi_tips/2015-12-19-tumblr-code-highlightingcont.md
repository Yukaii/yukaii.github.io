---
layout: post
title: Tumblr code highlighting(cont.)
date: '2015-12-19T23:56:23+08:00'
tags:
- highlightjs
- tumblr
- code
tumblr_url: http://hi-tips.tumblr.com/post/135558385801/tumblr-code-highlightingcont
---

## "No line wrap" configuration for highlight.js

Add following snippets into your css.

```css
pre, code {
    white-space: pre;
    overflow-x: scroll;
}

.hljs {
    display: inline-block;
    overflow-x: scroll;
}
```

via [http://stackoverflow.com/questions/31152421/highlight-js-with-blogger-cant-disable-auto-line-wrap](http://stackoverflow.com/questions/31152421/highlight-js-with-blogger-cant-disable-auto-line-wrap)

And what's more, if you wants line numbers looks like:

![](http://67.media.tumblr.com/c5e5c42ece8fd3970ad59b59985287a5/tumblr_inline_nznco3uAic1svfyli_500.png)

try
[http://idodev.co.uk/2013/03/syntax-highlighting-with-highlightjs/](http://idodev.co.uk/2013/03/syntax-highlighting-with-highlightjs/)
