---
layout: post
title: El Capitan openssl ruby native gem not build issue
date: '2015-12-19T06:19:08+08:00'
tags:
- ruby
- gem
- openssl
- native-extension-build-error
tumblr_url: http://hi-tips.tumblr.com/post/135505306426/el-capitan-openssl-ruby-native-gem-not-build-issue
---

![](http://66.media.tumblr.com/5311feff623435ffcdf5315586c506a4/tumblr_inline_nzlzo1Qa8E1svfyli_540.png)

Take `eventmachine` gem for example, run:

```bash
bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include
```

then run

```bash
bundle install
```

will build.

via [https://github.com/eventmachine/eventmachine/issues/602](https://github.com/eventmachine/eventmachine/issues/602)
