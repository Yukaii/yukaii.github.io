---
layout: post
title: '第十一篇 - 第一次自幹爬蟲就上手 - 使用 Ruby'
date: 2015-05-03 11:04
comments: true
categories: 
---
## 目標
我們將會解析中正大學選課系統的選課資料，將它轉換成我們想要的格式 (JSON)，把它存起來。

## 預先知識
* 已經裝好 ruby 環境了
* 如果是 OSX 記得把 Command Line Tool 裝起來

<!--more-->

## PART A:準備檔案
```bash
    $ mkdir ccu
    $ cd ccu
    $ touch Gemfile crawler.rb
```

建立專案資料夾及檔案，`Gemfile` 是 `Bundler` 用來管理 ruby 套件相依的工具，[沒有裝的先來裝個](http://bundler.io/)。

接下來前往[中正大學課程網站](https://kiki.ccu.edu.tw/~ccmisp06/Course/)下載該學期的課程資料，網頁下方有一個 **開課資料壓縮檔下載** 的區塊，下載並解壓縮，放到我們的專案目錄底下。

現在資料夾下檔案應該如下圖
![step1.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/dbdLytaARcKx03aTMwJR_step1.png)


## PART B:設置套件
編輯 Gemfile 檔，設置這次中正課程爬蟲所需要的套件。加入以下兩行：

```ruby
source 'https://rubygems.org'

gem 'nokogiri'
gem 'pry'
```

其中 `nokogiri` 是超常用，用來解析 HTML/XML 標簽語言結構的套件。
它可以將你的 HTML 檔解析成 Nokogiri 的物件，方便查詢、篩選。

[`pry`](http://pryrepl.org/) 你可以先把它想像成一個加強的 irb，附加上 syntax highlighting, plugin architecture, runtime invocation 等等強大的功能。我一般都用來 debug。

寫完之後跑 bundle，安裝完這兩個套件

```bash
bundle
```

裝完長這樣
![gemfile.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/hCNL1HqMSyGU5VFhGHNc_gemfile.png)

## PART C:玩一下
先觀察一下課程網頁的結構

![table.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/RtVfu4fbTZq6hrl7nEUV_table.png)


每個課程網頁裡面都有一張表格，表格每一列有我們需要的課程資料，我們要做的就是把每一格的資料從網頁中抓出來。

打開 Chrome 開發人員工具，對表格中的其中一格選擇檢查元素，可以看到下面的結構

![structure.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/J49HGBWgTKecrv9Fcz6R_structure.png)


```html
<table>
  <tbody>
    <tr>....</tr>
    <tr>
      <td>...</td>
      <td>...</td>
      ...
    </tr>
  </tbody>
</table>
```

`<td>`標記中存的就是我們要的資料。它的結構是 table 包 tbody 包 tr 包 td。

<hr/>

稍微的來跟 pry 玩一下。

打開 Terminal 確定我們在專案目錄下，啟動 pry 進度 interactive console 模式：

![step2.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/uK7BV41mTQGR5lNaf3kn_step2.png)

將任意一個課程網頁檔讀進來

```ruby
str = File.read('1031/1014.html');
```

![step3.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/QSrjA3FsQuC389J9WPpi_step3.png)

我在行末加上一個分號，這是讓 pry 不會預覽回傳值。當沒加的時候，就會預覽回傳值，也就是讀進來的 html

![step3-1.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/Db5ei9I9TUugtynGB4PP_step3-1.png)

在此界面下，j/k 上下捲動，d/u 捲動半頁，q 離開回到 console。

再來載入 `nokogiri`，將剛剛讀取進來的 html 資料字串轉為 Nokogiri 物件，我們將物件存為 doc 變數：

```ruby
require 'nokogiri';
doc = Nokogiri::HTML(str);
```

![nokogiri.png](http://user-image.logdown.io/user/1128/blog/1112/post/243459/yRsJw8cxT7WOdnOnfqdx_nokogiri.png)

<hr/>

在此我們就進入爬蟲的精華了！(自己說)

接下來我們將使用 CSS selector(選擇器) 語法，幫助我們從落落長的 html 文件中拿取我們要的資訊。

css selector 基本的規則分成幾個：

* HTML tag selector
* id selector
* class selector
* attribute selector

html 長這樣

```html
<table class="hi" id="table1" width="500"></table>>
```

也就是

```html
<tag class="class-value" id="id-value" attribute="attribute-value"></tag>>
```

在使用 css selector 就會用

```
table.hi#table1[width="500"]
```

來選擇，選擇也可以是多層的，例如 ```<table><tr><td></td></tr></table>```就用

```
table tr td
```

更多詳細精確的用法，請自行 Google。

<hr/>

繼續剛才的進度，我們把 Nokogiri 的物件存成了 doc，我們可以開始用這個物件下 css selector，例如

```ruby
doc.css('table')
doc.css('table tr')
doc.css('table tr td')
```

可以分別試試看結果。

Nokogiri 在 select 時也可以是嵌套的，請記住，`.css('')`方法回傳是一個 nokogiri 物件的陣列，所以我們可以這樣用

```ruby
doc.css('table').first.css('tr').first.css('td')
```

代表第一個 `table` 中的第一個 `tr`(table row)中的第一個 `td`(table data)

## PART D:動工

```ruby
require 'nokogiri'
require 'pry'
require 'json'

courses = []
Dir.glob('1031/*.html').each do |filename|
  str = File.read(filename)
  doc = Nokogiri::HTML(str.encode("utf-8", :invalid => :replace, :undef => :replace))

  doc.css('table tr:not(:first-child)').each do |row|
    datas = row.css('td')

    courses << {
      grade: datas[0] && datas[0].text,
      serial: datas[1] && datas[1].text,
      class_type: datas[2] && datas[2].text,
      name: datas[3] && datas[3].text,
      lecturer: datas[4] && datas[4].text,
      credits: datas[6] && datas[6].text,
      required_or_elective: datas[7] && datas[7].text,
      time_location: datas[8] && datas[8].text,
      type: datas[10] && datas[10].text,
      outline: datas[11] && datas[11].css('a')[0] && datas[11].css('a')[0][:href],
      note: datas[12] && datas[12].text
    }
  end
end

File.open('courses.json', 'w') {|file| file.write(JSON.pretty_generate(courses))}

```
這是最終完成的[結果](https://github.com/colorgy/crawler-CCU-course)，實際上和我們剛才在 pry 裏試玩時差不了多少，一些函式如果不理解都可以 Google，有幾點注意。

#### encode

```ruby
str.encode("utf-8", :invalid => :replace, :undef => :replace)
```

在這段我將檔案讀出的編碼去掉不合法和未定義的部分，否則在輸出成 JSON 時會出錯。

#### 賦值

```ruby
datas[2] && datas[2].text
```

先確定 data[2] 非 nil，再取用方法，比用 if 判斷 `data[2].nil?` 簡潔

## Debug

在你想要的斷點加入

```ruby
binding.pry
```

一般來說我會在插入的該行下方再多加入一行無意義的輸出，例如：

```ruby
binding.pry
puts "asdf"
```

因為假如 binding.pry 的下一行是 end 或是方法結束，斷點會直接跳回上一層，很不方便。

在斷點的地方你可以直接取用你所在區域的變數，以及可以用的方法，如此你可以邊寫邊確認你的程式碼是正確、或是你想要的，就不用在每次 print 出來啦！
