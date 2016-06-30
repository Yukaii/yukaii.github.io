---
layout: post
title: My simple way of setup rails production
date: '2015-12-26T20:43:29+08:00'
tags:
- passenger
- nginx
- rails
- production
- setup
- deploy
tumblr_url: http://hi-tips.tumblr.com/post/136019002411/my-simple-way-of-setup-rails-production
---
With Paseenger & nginx. 

1. Install passenger and nginx from APT source
2. Install ruby using rvm
3. Config nginx passenger path, ruby to rvm source, and root to project public folder

I use ruby gem to install passenger and nginx at first(gem install passenger then rvmsudo passenger-install-nginx-module), but only nginx works. Maybe the mix different tutorial leads to this problem. After I removed passenger & nginx and installed again from APT, it finally worked. 
