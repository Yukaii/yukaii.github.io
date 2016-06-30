---
layout: post
title: Allow public accessing local rails server for testing
date: '2016-03-25T19:55:52+08:00'
tags:
- localtunnel
- rails server binding
- public testing
tumblr_url: http://hi-tips.tumblr.com/post/141696293281/allow-public-accessing-local-rails-server-for
---
I list two different ways below.

```bash
rails s --binding=0.0.0.0
localtunnel npm package
```

For the first method, simplely run up rails server by that command, then access via ip address with port 3000 by default. If you don’t have a public ip address(only LAN address), you might need to configure your Router NAT to make port 3000 alow public access.

The other way is using localtunnel npm package, install by

```bash
$ npm install localtunnel -g
```

then after running rails server command, run

```bash
$ lt –port 3000
```

The localtunnel will give you an unique url like https://gqgh.localtunnel.me. Now you can accessing your local server via this url.
