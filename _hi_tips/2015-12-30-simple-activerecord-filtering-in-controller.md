---
layout: post
title: Simple ActiveRecord filtering in controller
date: '2015-12-30T06:01:41+08:00'
tags:
- ruby
- rails
- controller
- filtering
- model
tumblr_url: http://hi-tips.tumblr.com/post/136256287331/simple-activerecord-filtering-in-controller
---
In Search and Filter Rails Models Without Bloating Your Controller this article, he described that you can filtering by multiple params like:

```ruby
params.slice(:status, :location, :starts_with).each do |key, value|
  @products = @products.public_send(key, value) if value.present?
end
```


Alternatively you can write:

```ruby
YourModel.where(params.slice(*[your_filtering_params_array]))
```
