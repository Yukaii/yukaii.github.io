---
layout: post
title: |-
  There was a problem with the editor 'vi'.
  Not committing merge; use 'git commit' to complete the merge.
date: '2015-12-28T12:01:57+08:00'
tags:
- git
- editor
- can't
- commit
- vim
- config
tumblr_url: http://hi-tips.tumblr.com/post/136130180901/there-was-a-problem-with-the-editor-vi-not
---
I encountered this problem someday after upgrading vim through homebrew maybe. I forgot :p. However this problem can be simply solved by:

```
git config –global core.editor /usr/bin/vim
```

via http://stackoverflow.com/questions/14607584/using-vim-for-git-commit-messages-broken-after-updating-janus
