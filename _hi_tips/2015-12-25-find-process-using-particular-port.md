---
layout: post
title: Find process using particular port
date: '2015-12-25T06:01:37+08:00'
tags:
- linux
- process
- port
tumblr_url: http://hi-tips.tumblr.com/post/135908729626/find-process-using-particular-port
---
I got three ways in this post

- netstat
- fuser
- lsof

### netstat

  sudo netstat -nlp


### lsof

  lsof -i tcp:43796

### fuser

  fuser 43796/tcp


via 
http://askubuntu.com/questions/227161/how-can-we-find-which-process-is-using-a-particular-port
http://www.cyberciti.biz/faq/what-process-has-open-linux-port/
