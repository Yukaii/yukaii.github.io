---
layout: post
title: git cannot commit with vim
date: '2016-03-18T09:06:22+08:00'
tags:
- git
- vim
tumblr_url: http://hi-tips.tumblr.com/post/141259476321/git-cant-commit-with-vim
---

After a clean install of vim bootstrap setup, I cannot commit with vim. Simply fix it by resetting default git prefer editor:

```bash
git config core.editor 'vim'
```
