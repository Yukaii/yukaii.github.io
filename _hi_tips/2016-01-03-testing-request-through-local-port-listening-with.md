---
layout: post
title: Testing request through local port listening with netcat (nc)
date: '2016-01-03T18:01:34+08:00'
tags:
- netcat
- testing
- request
- nc
tumblr_url: http://hi-tips.tumblr.com/post/136574420374/testing-request-through-local-port-listening-with
---

Run `nc -l YOUR_PORT`, for example:

```bash
nc -l 9000
```

And set your request endpoint to localhost:9000.

via [http://hilton.org.uk/blog/inspecting-http-requests](http://hilton.org.uk/blog/inspecting-http-requests)
