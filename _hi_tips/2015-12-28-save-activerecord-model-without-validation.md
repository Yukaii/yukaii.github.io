---
layout: post
title: Save ActiveRecord model without validation
date: '2015-12-28T06:01:16+08:00'
tags:
- rails
- activerecord
- without
- validation
- save
tumblr_url: http://hi-tips.tumblr.com/post/136111758219/save-activerecord-model-without-validation
---
Simple:

```ruby
model.save(validate: false)
```
