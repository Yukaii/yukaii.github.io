---
layout: post
title: '第十三篇 - 弄了一個統一發票查詢的 gem'
date: 2016-03-19 13:17
comments: true
categories: [ruby, gem, 欸嘿]
---
感覺很久沒發文章了。

因為正在準備大重構敝司的程式碼，最近這兩天，就把八個多月前寫的一個「統一發票兌獎」的 gem 給重構了一番，順便找一下手感。

其實無非就是加些 class，修一些 bug，補些測試，還是來這邊記錄一下，畢竟也半年多沒更新了 XD

[Uniform Invoice Lottery(Github)](https://github.com/Yukaii/uniform-invoice-lottery)

## 安裝
一如往常在 `Gemfile` 加上

```ruby
gem 'uniform_invoice_lottery'
```
然後跑 bundle


## 使用範例

```ruby
require 'uniform_invoice_lottery'
prize = UniformInvoiceLottery.check '82930261', time: Time.new(2015, 7, 26)
prize = UniformInvoiceLottery.check '82930261', year: 2015, month: 7 day: 26

puts prize.amount # => "10000000"
puts prize.title  # => "特別獎"
```

大概這樣，資料是從[財政部這](http://www.etax.nat.gov.tw/etwmain/front/ETW183W1)抓的。不過我的 css selector 過了八個月又大修了一番，希望別再無效了 :p