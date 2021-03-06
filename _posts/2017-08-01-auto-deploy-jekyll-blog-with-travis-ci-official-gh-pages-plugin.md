---
layout: post
title: 用 Travis CI 自動部署 Jekyll 部落格到 GitHub Pages
published: true
---

弄完 [ics-scheduler](https://github.com/Yukaii/ics-scheduler) 專案後沒多久就開了 [Travis Auto Build 的 issue](https://github.com/Yukaii/Blog/issues/2) 放置 Play。可惜我實在太懶，而現階段的部屬流程也很簡單： 建構、提交再推送。建構跑 `jekyll build` 指令，包成 bash script 更是秒秒鐘；提交完，fish 又有基於歷史的指令自動補全，推送的 `git subtree` 指令打到一半，後面的自動補完出來按個 enter 就好了。

自動化永遠不嫌多，把建構出的結果一起加到進版控系統追蹤也不夠優雅的，每次新增一篇文章 diff 又一大堆。正巧今天早上收信的時候，看到以前關注的 GitHub 議題：「[將 jekyll-assets 整合到 github pages 官方 gem](https://github.com/github/pages-gem/issues/189#issuecomment-319070628)」有更新，留言提到 [Travis 現在也有 GitHub Pages 自動部屬的預設配置](https://docs.travis-ci.com/user/deployment/pages)了。機不可失，官方有支援，設定就簡單。來設定自動部屬吧 XD

## 設置

交互參照 [Travis CI 的文件](https://docs.travis-ci.com/user/deployment/pages)及 [Jekyll 的文件](https://jekyllrb.com/docs/continuous-integration/travis-ci/)，最後的 `.travis.yml` 長這樣：

```yaml
language: ruby
rvm:
- 2.3.3
 
script: ./bin/cibuild
 
before_script:
 - chmod +x ./bin/cibuild
 
deploy:
  provider: pages
  skip_cleanup: true
  github_token: $GITHUB_TOKEN
  repo: Yukaii/yukaii.github.io
  local_dir: _site
  target_branch: master
  on:
    branch: master
```

分三段來看。

### `language: ruby`

因為用的是 Jekyll，所以該有的 ruby 環境也要設定。Travis clone repo 下來之後便會自動 `bundle install` 裝完套件。

### `script` & `before_script`

這部份就是建制網站，cibuild 這個腳本長這樣：

```bash
#!/usr/bin/env bash
 
set -e
 
JEKYLL_ENV=production bundle exec jekyll build
```

就是 jekyll build 指令啦。

### `deploy`

最後 deploy 的部分，因為我部屬到 GitHub Pages 的 使用者頁面(User Site, **username**.github.io)，所以 `repo` 和 `target_branch` 要額外設定。其它就照著 [Travis 文件](https://docs.travis-ci.com/user/deployment/pages)預設填就好了。

Jekyll 建制完的檔案預設會在 `_site` 目錄底下，所以 `local_dir` 也要設為 `_site`

## `No such file or directory @ rb_sysopen` 錯誤

在 Travis CI 上面跑 Jekyll 就會炸掉 XDD 就照著 [jekyll/jekyll#2938]( https://github.com/jekyll/jekyll/issues/2938)，在 `_config.yml` 的 `exclude` 加入 `vendor` 解掉。

## 成果

以後只要用線上的 Markdown 編輯器寫完文章（比如 [HackMD](https://hackmd.io) ），再貼到 GitHub 線上 commit，部落格就能自動更新啦。也可以用 [prose.io](http://prose.io) 這類的線上 GitHub 內容管理工具，編輯提交一手包，還有草稿可以用 :p

Travis 建構的缺點是 CI 服務裝完套件還是得花個幾分鐘，沒有以往那麼迅速。寫個文章而已嘛，還是可以忍受的，geek 到心理愉悅最重要（？）

（完）
