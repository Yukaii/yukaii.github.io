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

1. `rails s --binding=0.0.0.0`
2. localtunnel npm package

For the first method, simply run up rails server by this command, then access our server through your local machine public ip address with port 3000(by default). If you don't have a public ip address(only LAN address), you might need to configure your Router NAT to make port 3000 public visible.

The other way is using localtunnel npm package, you can install it by:

```bash
npm i localtunnel -g
```

After run up rails server(`rails s`), run

```bash
lt –port 3000
```

The localtunnel will give you an unique url, something like `https://gqgh.localtunnel.me`. Now you can share this url with people who want to access your local server.
