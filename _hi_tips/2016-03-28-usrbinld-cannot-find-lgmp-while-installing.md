---
layout: post
title: "/usr/bin/ld: cannot find -lgmp while installing bson ruby gem on Ubuntu"
date: '2016-03-28T08:19:24+08:00'
tags:
- ruby
- gem
tumblr_url: http://hi-tips.tumblr.com/post/141841907026/usrbinld-cannot-find-lgmp-while-installing
---

Install the missing library to solve the problem.

```bash
sudo apt-get install libgmp3-dev
```
