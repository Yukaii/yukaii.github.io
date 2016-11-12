---
layout: post
title: "Electron 的 Trello 桌面版應用程式"
---

## 啊 Electron 不就好棒棒

Electron 簡單來說就是加料版的 Chromium，可以跑 node 環境，還能讓 Web App 也能使用原生桌面環境的 API。現在你我的桌面早就被一堆 Web App 攻佔，比如我最常用的：

* [Nylas N1](https://nylas.com/) - Email 軟體
* [Visual Studio Code](https://code.visualstudio.com/) - 微軟出的輕量級編輯器
* [Postman](https://www.getpostman.com/) - REST 測試御用輔助工具
* [Slack](https://slack.com/) - 社內通訊
* [Notion](https://www.notion.so/) - 筆記，雜想
* [Hyper](https://hyper.is/) - 終端機，不過不能輸入中文所以就沒再用了。開發的 [zeit](https://github.com/zeit) 社也滿猛的
* [Caprine](https://github.com/sindresorhus/caprine) - Facebook messenger 桌面版

隨便列就一堆了，相當於電腦裝了十幾個網頁瀏覽器，有夠肥一個就一百多 MB 😅，根本快速開發的代價。

Web App 相對於傳統的桌面軟體迭代應該算十分快速了啦，光 Flexbox Layout 就不知道方便多少了...好啦其實我唯一寫過的傳統桌面軟體也只有用 JavaFX 學校作業，也只花了三天趕工哈哈哈...也是沒慢到哪裡去啦，咦？

## Nativefier

有段時間我很喜歡用瀏覽器的「Pin Tab」功能，現在 SPA 又那麼猖狂，App 用 Chrome 開就行了，只要用 Chrome 的快速鍵 cmd + 1/2 就直接跳回前幾個 pin 好的 App。不過因為使用的 Extension 實在有點多，所以常常感到 Chrome 頓挫，此時用 Electron App 就相當於用一個乾淨的瀏覽器，速度快了許多。~~其實你把 Extension 移乾淨不就得了。~~

社群前陣子也有大大推了一個叫 [`nativefier`](https://github.com/jiahaog/nativefier) 的工具，只要設定一下，就能把網站包到 Electron 的 WebView，立馬變桌面 App。

通常我們在實作這種網頁直接包成 App 的，除了單純用 Electron `BrowserWindow` 或 `webview` API 載入 Web App 網址外，還需要 inject 一些額外的 JavaScript 來整合桌面功能，比如快捷鍵跳轉特定功能啦、改變配色啦。

大神 sindresorhus 的 [Caprine](https://github.com/sindresorhus/caprine) 便是個好例子，還有 terkelg 的 [Ramme](https://github.com/terkelg/ramme)，單純的改變原本就已經用 React 做成 SPA 的 Instagram 的排版，也加入了快速鍵功能，便是一個堪用的 Instragm 桌面應用軟體了。當然真正好用的 IG 桌面 App 還是非原生莫屬，在此推薦一下 [Flume for Instagram](https://itunes.apple.com/us/app/flume-for-instagram/id792425898)。

## Trello feat. Electron

最近重回 Trello 懷抱。為配合我的使用習慣，就找了找對應的桌面軟體，找到了兩個：

* [Paws for Trello](http://friendlyfox.es/pawsfortrello/)
* [Trello Desktop](https://github.com/danielchatfield/trello-desktop)

這兩個 App 幾乎一樣，就是把 macOS 的 titlebar 藏起來讓紅綠燈好看一些。儘管前者在 App Store 上架，可是功能沒比後者多多少哈哈。

![](https://lh3.googleusercontent.com/gDwO7Rcc_DiHTqNTa9W8_u5WggQxm1km4TL630k66fGucUYCKoU4PEjqVodsSsB5wtw1rHket9Mnh-2qSyPWIU0Nn0TZsoLOitMniwifs2JFkgQJysppEK0rTDsPasRDvj1-axpA6oEanesF-42NvwTAWxeVmN-SjspdOPUxgdj8P9LkJPHACA49dCYXPfHrfbqP_SsFYKL7FlReq9b0Xbs5VWQmCPlWewyN12bZQq7-j3_POwhXRwPqFwvb5zfx9in7AibIxgz5wQAgNR-TUVwiPvN0nKIX5XrMVPCLHOgsHsIeFfQuHZuJaJ56kZ5kc-t9m11F5x0_BB6uoODrHd8UEumqEDA9hHGh8P4jnVMXYXmBZjGCcpSaIK_076JKVgUZ_cVxzcC_oDa0_YfS4Si-9qbT8RKEK-AMhvNKYVhsJ6z1N1ZCeG4O57uqmn0TzM6iBMQ_01LXYt1Db4AQxRdoE7m5PgbUU-WOkN_ZhJhRFcsIDxLoZT9gngSJlAZJT0j9t0G9hniF5uImwh7onklliQayTsA8N9PpJc-CpkrVjko3WjAbXyNqzrdm1QCDScI0PV2cEhFy4BJvsFFfZUrXl1W7PdYJJKHYfVUTRrDeAIIfiQ=w971-h626-no)


後者 Trello Desktop 雖然免費，目前有個最大的問題：沒辦法[拖拉視窗](https://github.com/danielchatfield/trello-desktop/issues/6)。

當然也有開發者直接發了 [Pull Request](https://github.com/danielchatfield/trello-desktop/pull/13) ，不過專案好像已經被放生了一樣，坑主都不回的。作為一個略懂 Electron 的開發者就來自行修正一下吧。

仔細看下 [PR](https://github.com/danielchatfield/trello-desktop/pull/13) 其實也就加三行 css，我們要做的就是把這個 PR，patch 到我們本機的 Trello Desktop App 裡。

做法有兩種，其中一種是 clone 改完的版本，重新建構 Electron App，不過太麻煩了，我們就直接 patch 本機的 css 比較快。

## Patch Elecron Client code

[asar](https://github.com/electron/asar) 簡而言之就是 electron 封裝 client code 的格式，一般在建構 Electron App 會預設使用。我們先安裝下：

```bash
npm install asar -g
```

現在就可以解開 asar 檔了，進到 App Package 裡：

```bash
cd /Applications/Trello.app/Contents/Resources/
asar extract app.asar app # 解開 app.asar 檔案到 app 目錄
cd app # 進去 app 目錄
```

編輯 `browser.css` 加入更改的 css

```css
#header {
  -webkit-app-region: drag;
}
```

Electron 預設便會讀 app 目錄或 app.asar 檔案，重新開啟 App 後就可以自由移動視窗啦，跟桌面軟體有 87% 像！

![](https://lh3.googleusercontent.com/w7WF_SiDo7mrJanSRR72ieb_NTOPpblGcK-nMovxrnQvAG_1fcKuZ_EhIC_5_8-kXSIztIoEMk3l2gqoNlQx8VOS1UW98E6YVlBqV8qJOLD7YPAeY_eVZWWpdHkbtuBQh3O5g_xcFU-_IiHlVOcDVWiAdfdQipK7AmN7RHNs5fuTMLEo4ZDRZGH3ic9u92FT4A8n2z75P9y7kWMmYhQ_lyuFWaOSyxwXke_iJR0UY9L1uMnW856i651cuQNlp2br9rGAB-LSGKElDQpI64h2HxTk9eln2tMcvMG1SO9Qz6AF7kcbFanCL7uKv1AoGGX81qlGugiUtkq_I8uNASIVbPaeVxSdJniKa46DJuc-twautw6qdHUQ5jgpJyKOmUtbosVeJK5DS50jrsGzyOQPVbH7SzVpavlpKqnUAxOZ9AEAsxX0sf6cROCPtvHe3UvUqSTEwXk841vTgoxrS27ysLXVRVhdZe8CEE5vSbDIIObdHkV0CVULCWRKspmFInNEd4m-cvE4pHWdycItvMJo7dqN5gydHPIBZzU7z50b8Hn2_wYzUA7miYPvpMpOmsaW4BSOwkW1bPHGG0_2mf0gGWlR8URwayBvSouObSCoaDDGostsNw=w1188-h737-no)
