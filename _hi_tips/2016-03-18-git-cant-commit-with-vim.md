---
layout: post
title: git can’t commit with vim
date: '2016-03-18T09:06:22+08:00'
tags:
- git
- vim
tumblr_url: http://hi-tips.tumblr.com/post/141259476321/git-cant-commit-with-vim
---
After a clean install of vim bootstrap, my git can’t commit with vim editor. Simply fix it by:

```
git config core.editor 'vim'
```
