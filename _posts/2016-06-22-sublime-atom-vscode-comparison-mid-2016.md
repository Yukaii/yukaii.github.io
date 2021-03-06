---
layout: post
title: 2016 年中、編輯器三雄大對決！
comments: true
published: true
---

**TL;DR**: 立馬下載 [VSCode](https://code.visualstudio.com/)！！本文將會持續更新

## 前言

這題目一直以來都非常想戰啊，身為一個設定狂，什麼編輯器都要玩玩，惡搞到每個編輯器開啟時間都變超久...XDrz

這次選了三個主流的編輯器：Sublime Text、Atom、Visual Studio Code，下面就來簡介一下各編輯器的特性、好用插件，以及本部落格最愛的：秀截圖 😆

## Sublime Text

原生就是順暢來著，又擁有豐富成熟的插件體系，一直以來都受到前端社群的愛好與推廣，可惜現在來看更新頻率還是比較慢的，[dev builds](https://www.sublimetext.com/3dev) 大概三四周不定期發佈，也要 $$ 使用者才能下載 XD。小弟最愛的 [Font Ligature](https://sublimetext.userecho.com/topics/4719-does-sublimetext-support-programming-ligatures-fontlike-fira-code/) 似乎遙遙無期啊（BTW，這串留言實在很有趣）！以下推薦幾個我用的插件：

* [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter3) 配上給 JavaScript 用的 `eslint` 和 Ruby 用的 `rubocup`，搞定靜態語法檢查
* [GitSavvy](https://github.com/divmain/GitSavvy) ，Sublime 的 Git 輔助套件，有如 emacs 底下的 magit 般，讓日常 commit、diff、merge 輕鬆寫意，有時還是得用 GUI 或 CLI 工具輔助啦
* [EasyMotion](https://github.com/tednaleid/sublime-EasyMotion) 還有內建的 `Vintage Mode`，身為一個 vim 控，這兩個插件讓我不動滑鼠就能輕鬆編輯
* 最近突然喜歡起亮色系主題，使用 [Ayu Light](https://github.com/dempfi/ayu) 和 Monaco 字體。為了簡潔我把 Tab 關掉了，反正我也沒在用（咦）

![Sublime Text](https://lh3.googleusercontent.com/I-ggZpsG4xXRmwlqr3kC3Aax3_656hf2zbJyMRi0aV7NjgtgkThU_fTR2rRq82OHmTXFvCBqwfZAkvHbJ8Y93ebi2w5z2heodfxDI9P8vU3zcdQzzrxUxF9zkFM2jJ1c3Mq4xjLPABXBdawvScurqbrtJFEJniwstfLNpiFwPNrXItvPGfl_3DWsjSS6ovPgMI6CeeDfGIgBJGIOaV4wVpeja0OtxROyXn44oNhX1t1FdzGCJIuUiGmClpFttAeKyy1JPYMMduNP59M0Euj-B5TM3PwMJgHDluv7uoSKbUlr1ZisCu86nqf8YYXj5oMM46cUTky2_6Vt7sQwmNDcjbAX1zuEyiWB3IHbS31tZbqxWfPaLIlmz-sCuTQ-y2CXcHGYYCEmawW51OP94DYtQW1MS1TcUmkAgaWSzfWea9NrxVl5zhDvfgh5CLwbTmONVX2IZz0AoHwN1RUjLh56EwR-YV4obV0JNJvGvA5solfLocNoaO9A70Qc4EXWeIKgx55e1mkD_YhrmIlSCqLRHv42_1ZDnJGwnIljPIDkbi3MIEzncmExbBA4lUCscEC5zGjbz8I9AN1UXhzQbx4fDnpv2bZnUBhEk-4CTSBOKW1Vpc4KuV8LLaJC4g=w1080-h768-no)

## Atom Text Editor

* 套件裝不完編輯器系列。有著比起 Sublime 更好用的套件管理界面，不過在我的 MBA 13 early 2014 上，開啟大量檔案編輯、或開啟稍大的檔案時（也才數 MB 吧 XDD），程式碼高亮緩慢，外加捲動頓挫感明顯。
* 介面單靠 css 就可以自訂，有著比起 Sublime 更加一致性的自動補全（Auto complete）及錯誤提示介面。
* 有內嵌的第三方 terminal 插件（`platformio-ide-terminal`），不過多開仍舊頓挫（汗）
* 資料夾內搜尋**預設**排除 `.gitignore` 列表。不會像 Sublime **預設**一樣搜出一堆 `node_modules` 的套件（不用另外設定就是方便）
* 內建 `timecop` 插件，可以監看每次啟動時各個插件所消耗的時間。真是走到另一個奇妙境界去了，是有多需要調教啊（~~的確很需要~~）
* 社群能量驚人（看那精美的星星數），或許過不了多久效能就能大躍進...吧。[會有軟黑出沒](https://github.com/atom/atom/issues/10188)

下圖是 Atom 1.18 版的截圖，VSCode 的 Git 整合似乎驚動了 Atom 圈，迫使 Atom 推出了一樣的功能（笑）我用的是內建的 One Light 主題和 [kary-foundation-light](https://atom.io/packages/kary-foundation-light) syntax scheme

![Atom](https://lh3.googleusercontent.com/uJIO6dyRP2DqYcQGmRac7NCZFkXD2O8_5A4Gxkfv31QmvG98CtA-1Y2gxTLN21df1nqIow9VWgjViuub3lsFQtJ9bAuRKdnwQnHR2ahy94Kl8Q_MUtSzcMT97iOqZJmeSHeCXF-Df1Cy_Q1n5ZtXNR9JZQeenEUlRGvyOVqCSKB6gcIk_Ng180j078ZM2oTamj0tEcAuM7nXXIfdmCrYoioYhcM5E5Fuc52poqu10e8SHlwfL0ge_QSe8nj_Vt1S2LCCTenpqgN19Pedj1_CWULicD7SxGwbvGsW4bXsmLQ7gBPVFHOjAbftcVD1om8Uj1It2Pcn5kxwdDJ7t1un-g494zKmcCkocCmfm0tOo5j1vRSPiEFrr91OPfzuijeVyQ13qHdcEkoPNlEPoAbcdaHaVeCZTpQF1B7kNnkBfWccZyf6v5e7qWYyLgnVDVPrGqFsKRGoDMXapGfMUbssYDYjf_GHoesPOiUeVbRWGq2ihPO7AjKPqXsa0zd3piHUuglie8mnQjN74WCL2fuBwWWW6ZpcZiurutNIafzw5ZpkRGpPufWIjvanXN7K8xRBo4EUg6rngr344OnqCGkvZgXULz0-Jfk8DDDSUBKl1jHbrs7yyQeX3dZPCg=w1080-h685-no)

## Visual Studio Code

* 三者中的後起之秀，快讚歎我軟技術力威能。
* 基於 Atom 同樣使用的 [Electron](https://github.com/atom/atom/issues/10188) 框架開發，不過卻被我軟寫的更佳滑嫩順暢，順暢程度宛若原生應用，~~早就叫你不要用 CoffeScript 要用 TypeScript 了吼~~（欸）
* 在即將推出的六月份更新，加入了 Tab 、內建 Terminal 支援，以及新的插件安裝介面。Tab 竟然是後期才加入的，害我被調教成不需要 Tab 的形狀了
* 我軟不斷採納 [UserVoice](https://visualstudio.uservoice.com/forums/293070-visual-studio-code/) 上的使用者反饋，GitHub 上詳細的列出開發 VSCode 的[產品規劃 (Product Roadmap)](https://github.com/Microsoft/vscode/wiki/Roadmap)、[迭代計劃 (Iteration Plan)](https://github.com/Microsoft/vscode/wiki/Iteration-Plans)、[開發程序 (Development Process)](https://github.com/Microsoft/vscode/wiki/Development-Process) 等等，甚至開發討論和會議記錄都會在上面，十分**公開透明** www，每個月的 [Iteration Plan](https://github.com/Microsoft/vscode/issues?utf8=%E2%9C%93&q=is%3Aissue%20label%3Aiteration-plan%20) 更是有如等待漫畫月更一般期待 😍
* 內建 intellisense、debugger 還有 git 工具，界面一致性、統一的 API 更是**碾壓**前兩者。作為開源專案，當然連實作規格也要公開啊，[Language Server Protocol(LSP)](https://github.com/Microsoft/language-server-protocol) 希望作為一個統一標準，語法檢查、自動完成、重構等功能都交給 Language Server，編輯器或 IDE 當前端接。~~當然我軟有大一統的想法就是政治不正確了（誤）~~ 在 [wiki](https://github.com/Microsoft/language-server-protocol) 列出了目前有實作 LSP 的編輯器或 IDE 們
* [套件商店](https://marketplace.visualstudio.com)蓬勃發展中，套件數達到了 3776，逐漸追上了 Sublime Text [Package Control](https://packagecontrol.io/stats) 上的 4229 個欸嘿 😎 (2017.8.3)
* 本身就是個超大型的開源 TypeScript 專案，由 GoF 四人幫的 Erich Gamma 領軍，值得一提的是插件在獨立的 Extension Process 執行，寫爛的 Extension 不會影響到 VSCode 本身的速度，請參考官方的[介紹](https://code.visualstudio.com/docs/extensions/our-approach)，不知道有沒有針對哪個編輯器啦（摁！）

![VSC settings](https://i.imgur.com/orxXv4J.png)

上圖是幾經迭代之後的 VSCode 編輯設定檔介面，和 Sublime 一樣直接用文字界面編輯，但在 Sublime 下，每個套件的設定檔都是分開的，在 VSCode 卻是用 namespace 的方式寫在**同一個**設定檔裡面，方便管理許多，加上 Intellisence 支援，寫設定值自動跳出**該設定的說明**及**可以使用的值**，易用性碾壓 Sublime 啊 😆

![vscode-without-activity-and-status-bar](https://lh3.googleusercontent.com/_tjXgabmWCEdPJSosgaUshNNNMz8Ohfjhu2ohPxetmLu4UdYSVCAFTTAmaDwcpNLvDlYDOrNlTBPepSzLjzne5eqx651rGe7pt9GamQwfQ9kXQqtHzzLAqypjEwBMAIY9cewdqN931JXhgekWWOPTSCRnyOznIQexXV4XyeFOW4OzlqJUh2_OIvcJ0jji2bZ-sgJh1HHSldGewvyqu3E-BqKeEt3TwxE5j21G3Z8H9_YLf5JYvPj6NPx6Pum4IRen7JeJ0ltRgwt6NXA67kpLVJnH2KFZEg3QQJ_VhuJ3s9g18b-4_XNStZRDmgWnVWeGS40Bfk_Fn-RJLioGFGdWwh6_iW5vT0KsEE8n-7ckZYKLnPyUwKsc2mGOrwHfapwO9mDRuvPjLq69egOfRZIIOxD-qymahAowjqceik0sDcIg57s6AqW14s2X7AtgYNzeziPg8z0HpoP_wsCdA-eWoshoy7TbHwZro2AHAgMVEF62Vga2wCVQ-eRTbN-Hr6XQpq24rDBKuKuGsXK3asfyfsCZ0JjX1DeMXLdQVUgIZzpuA3ITyCiB1JLBy2_pa1Ukjgz_SAMZhLN3XvvTcDcoVnueIfFoiPjiYGiR41eZ1cl9Zj9Bg8_bQ=w1093-h776-no)

在自訂介面上，雖然不如 Atom 和 Sublime 那樣自由，改到媽媽都不認得，但介面都是由 HTML/CSS 描述，你要自己 hack 也是可以的，套件 [vscode-custom-css](https://github.com/be5invis/vscode-custom-css) 安裝後就可以插入自訂的 css。目前 VSCode 支援了還算完整的顏色自定義(Theme Color)，從編輯器介面到內建終端機(Terminal)的顏色都可以改，算是暫時對自訂介面的議題有了個交代。當然大家要的還不止這些啊，但要做的功能實在太多了，只好改天吧（？）

## 結論

**TL;DR**: 快去下載 [VSCode](https://code.visualstudio.com/) 就對惹。

**我已經變成 VSCode 的形狀**了，Sublime 也被打入冷宮，偶爾拿出來回味一下倒還可以。說到本文沒提到的 vim，在伺服器上偶爾編輯設定檔、臨時修改原始碼倒還可以，但我已經被自動補全和除錯器慣壞，回不去了。那麼 Atom 呢？毫無反應，就是個 Hackable 的編輯器，十分適合拿來惡搞，只要你想，回到 90 年代也未嘗不可，不過永遠就是卡，嗯。

![](https://lh3.googleusercontent.com/79NInG4caoVFAZlGOUKajtxlw5692Y3dnQDHwDomsBGKoT3q4hOaYGnSUUQCTyvSJO7ikL1DvsAH6n81kDWsx-36QtQM8VnfK5R-aJiFqP2W5vdQpzfwKjw7iw771dzP_eWf9eMU2DYDVX-WAg_Q_V_LmmQDqJMyx9Jx_Rx05Td15P6hzAmnJhSXuHlKUtVg2dChf22kOwZU2AvtHEOa5qhbTBI2pu6nbUwEODaEKpX5MIG5D0r-fobk9NrgkdGrGr6ZdaLy0iFJtqVNdRB3JolAU3yttOgWG_Nfp3Rihn9YZ-WDexU_d4A9D-Vu241WKnl9B0Y9oZ9gVjC7vbfhkbliGI9hfLWuusUVCAMjdBfRcH1lohoLifYeEnDJz2m7Qa88wbJxmCdoK8pNy50fAks8KehaQYI4FcKHg6GvyBbCoxOqTbX37VcEj8KA7owoF9OaFjWnbM0ngWOrqvjz7xwjMsSQS0l_zZzmQZAmIT7Dw8IPZQ4yb_W-16-Qu3fdw3kjsGfPWpAnb1l2kPMpX-BPwjk8GCX7ao2cHdsJz1R_MDmnu-XtBPiNV05kXlfoTucjmCBnSWK4Agd5TE05YQRFtqWq-iJs=s800)

（完）
