---
layout: post
title: Tumblr code highlighting
date: '2015-12-19T17:26:55+08:00'
tags:
- tumblr
- code
- hightlight
- snippets
tumblr_url: http://hi-tips.tumblr.com/post/135539680456/tumblr-code-highlighting
---
Add following snippets into your html header

```html
<header> 
   <!-- some other things here -->
   <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/styles/solarized-dark.min.css"> 
   <script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/highlight.min.js"></script>
   <script type="text/javascript">
     hljs.initHighlightingOnLoad(); 
   </script> 
</header>
```


You can pick one theme(syntax hightlight scheme) from https://github.com/isagalaev/highlight.js/tree/master/src/styles

Then you can wrap your code into html

`<pre><code>` tags.
