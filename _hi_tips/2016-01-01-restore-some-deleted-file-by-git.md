---
layout: post
title: Restore some deleted file by git
date: '2016-01-01T06:01:25+08:00'
tags:
- git
- restore
- deleted
- file
tumblr_url: http://hi-tips.tumblr.com/post/136385649997/restore-some-deleted-file-by-git
---

Find the commit SHA when you've deleted the file, then checkout back to restore it.

```bash
git log --all -- <path-to-file>
git checkout <SHA>^ -- /path/to/file
```

For more you can read from the original post on [stackoverflow](http://stackoverflow.com/questions/7203515/how-to-locate-a-deleted-file-in-the-commit-history).
