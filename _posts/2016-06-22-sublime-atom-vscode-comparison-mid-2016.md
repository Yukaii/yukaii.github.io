---
layout: post
title: "2016 年中、編輯器三雄大對決！"
---

## 前言
這題目一直以來都非常想寫，因為我就是一個「完完全全的設定狂」，什麼編輯器都要玩一玩、組合、自訂一下，惡搞到每個編輯器 startup time 都超久...

這次就選了我主要使用的三個編輯器，Sublime Text、Atom、Visual Studio Code，分別簡介一下，以及本文最主要的目的：秀截圖，畢竟功能那些都講到爛了沒啥好說的。

## Sublime Text

最近作者重啟開發，加上有著原生的順暢，所以目前仍舊是我的主要編輯器。[SublimeLinter](https://github.com/SublimeLinter/SublimeLinter3) 套件加上 `eslint` 還有 `rubocup` ，平常主要使用的語言 `javascript` 和 `ruby` 的靜態語言檢查就不成問題；[GitSavvy](https://github.com/divmain/GitSavvy) 這個新興的 git 輔助套件，讓平時的 commit、diff、merge 不成問題（當然有時還是得用 SourceTree 或是 CLI 介面輔助），[EasyMotion](https://github.com/tednaleid/sublime-EasyMotion) 還有內建的 Vintage Mode 讓我幾乎不用動到鍵盤；最近出現的 [Boxy Theme](https://github.com/oivva/boxy/) 能讓我以即為簡便的設定檔方式設定介面，包含各元件的字體，讓我徹底換掉沒時間支援的自訂主題：[Oceanic Seti](https://github.com/Yukaii/Oceanic-Seti-ST3)，以及最近大神推薦的 [primer theme](https://github.com/karelvuong/st-primer/)。

![Sublime Text](https://lh3.googleusercontent.com/1TzDNfasHb9xTGxOQikoklQf3QfROd1VxPgOXvdo1EOuaUiJViBLjIIP9-LXKAEEM34Az8th0As8wMny0EcaD8p4qqGdEK98CcAogGGhl9XF7H8fHQBx8c8oVJoL7YOgiWJLSLs-7CdaaFtJ5RFkY05wjpnD-k-9zu4HoOLQwSgYxmy2oW_DdocLB03-LOjr6pNb39_1eoNtM4DoXrsx-7C_jE3HBrD-lykeyXXr1PPUuSzPB5WtBapeFpgXUfeRuQOtoR83czm0p97qm6xdedotPhRYvj_CwkzFiFjGmjBUQTWmAFFsY_PRf5hve-kDSFfnXBQB5TPu3DqVluwAXNTpr5xYxRO94t6oIm1MUXhyj007ZG5YCHBSvc-Veoio0DYO16GHmCo45Z0Mxr2c0_WIKGXeQV1IDsra_kIIaIF5RyMh00Wev2TySo5qGWa-rbsy1WodZ4sXEGxprIjFqdsoY20P0OubaPtpv6c4JuN7NQZuPqkz-E-3m7nFoTvxJUcbg8M9rIGGP0qxHvYiwCfgD1vg281IhduUcvyFAJqS4S2qh5_X_YmoIfBzMohlcwuCmY_2egbIoltd0zFb_CGDg9RfIi4a=s800)

## Atom Text Editor

套件裝不完系列。有著更佳易用的套件安裝系統，不過在多開編輯檔案時，有時會發現程式碼高亮緩慢，捲動不順。介面單靠 css 就可以自訂，有著比起 Sublime 介面更好的 autocomplete，linter 提示更佳合理。有內嵌的 terminal 插件（`platformio-ide-terminal`），不過多開仍舊頓挫；資料夾內搜尋預設排除 `.gitignore` 列表。tab 無法摺疊（tab folding）十分困擾，不過社群力驚人，或許過不了多久效能又能一大躍進吧。atom 有 `timecop` 這個插件，可以監看每次啟動各個插件所消耗的時間，真是走到另一個奇妙境界了 XD。

![Atom](https://lh3.googleusercontent.com/K_Frf6tKJ4YwwG8LtG6YfqXpzC21AYbcb7vv7sbK0bnp9gYtoy3Wd2zJ3ncA3Nc5OYegV0ii3uLtfy5Z3MzZu5916zQ5AajJsa3kjvp5GEQr5BjVwEyFZE1QKNA3I2EzkovbdR4ZF-nGGssgwKrwzw6NBeQ8JnmClpXGKUJfiAsAPFNa8aOqakICu2SLbG0gfABtx86NPiRjgAD2h1hXZnj3S9ztdOO5F2-0-Fs-PVIhMGpy5yrj4SidVUn0OmjFmyaBGPANY_Xfkl5ulZF0WEl-ffPqu55G3SimXWnP6KEqCXj2ChuuO_HX3qiycazP0j3GMSxJY2MVLptzZXcACkowLs4gWMow09ZdH9oe1MSMxclNneoccvNSAZ9DyGsxTdZejgmcNT4harDopf9RteXI1CH6bwojar72Q4bQAXu00BwtBYQGWPFNknfvjG4HBoMf_8FQKruyTZqJVJx9R_JCsM8Lnr-7dnMLT1AEENRWt7zyxqFIg4t1dMCstbMECcjJueqANMCI--Hh4CxrCuKcNHAUmkOJh8_dBxTHZ9pxEuyrXoeyz5OxR2zxXIBiKtwBdX1RCxZU2Fp4dGSRbgAPYvpZfwd0=s800)

## Visual Studio Code

後起之秀，只能讚歎我軟技術力威能。同源自 Atom 的 Electron Framework，不過卻被我軟寫的更佳滑嫩順暢，順暢程度宛若 Native，<del>早就叫你不要用 CoffeScript 要用 TypeScript 了吼</del>，在即將推出的六月份更新，也加入了 Tab 實作、多開 Terminal 支援（對你沒看錯，官方支援 Terminal），以及新的插件安裝介面。我軟不斷採納 [UserVoice](https://visualstudio.uservoice.com/forums/293070-visual-studio-code/) 上的使用者反饋，推出時最大賣點為內建 intellisense 的 code complete，官方支援的 debugger 還有 git tools，另外最近還出現了官方支援的 react-native 插件......[套件商店](https://marketplace.visualstudio.com)也是蓬勃發展中。你怎麽能不愛他呢？XD

看看下圖，連 syntax highlight 程度都完勝......真是花俏啊，驕傲的兔子。

![VSCode](https://lh3.googleusercontent.com/qrZaLIwG3cYV_uKDbAxb-nh_OHt55TeUjg3krrNLgPsRI1X1_yc6vv6vLYDaCStUHoCWulFTx-AbayRUoGlh44tRnE84POO7WicMuFyWfIqmvfhUbAbGvOkggXT3Ekbr2eAlEKEP2kjfKTFTz3WrWwObX-W2Fti52m0QGdjLCGcjPt6qjPnLPhtdxCOLj_ZCpl_sxiYCHwd9DDrfL_FetwtPWQsWVze4BF7UY7Kz9y2RDL3G8k9b3bCtqAjiv7dIL03WP-WYWl3MDppt0imrxpZhOe_SD7g3csbJss_O2SOO20Og8Tcki4Q2rrQx8C4GkBI-SCwa_4Wi1cuNZX7zQ2_xxpSz0w4b4kEn72OxhK7rRKO7q9C1S9Sm9ULEYvMq2dZd4skEjKmS2xNkETEYgPDtyU_hutgyY5LcrfApAW6akLIY3sHNPK0SjdtmSuU_L_Z2A1bbRFeY548lyt9NizQn9ddUqn2WYBbw45UWBDiW-cCTLpiNCWjj7yED-04h6Tgw2yjEB_g-HAHujS6IGtQtgmHa56Tc_WqjzESOxIaCfCxYWMetCzMD-DDNi8c9wIGDXj-jDifMc3bGOVXQWf-9Jd6PKvDp=w1053-h735-no)

現在最阻擋我使用 VSCode 的功能只剩 minimap ，以及有時 cpu 資源消耗過高的情況。畢竟不是 Native 嘛，但我想再這樣發展下去，追上 Atom 也不是不可能啊。

值得一提的是，雖然 VSCode 的設定不像 Atom 有 GUI 可以勾選，也是和 Sublime 一樣需要編寫設定檔，不過設定檔有 two panel 對照，自行安裝插件的預設設定也會擁有自己的 namespace，被串接到預設設定檔的下方，再配上 VSCode 強大的 autocomplete，可遠超 Sublime 幾條街啊！

## 結論

<del>你怎麽不去用 vim 呢？我都用 [Vim Bootstrap](http://vim-bootstrap.com/)</del>。總之，平時我還是以 Sublime 作為主要工具，但也對 VSCode 的發展十分看好。那麼 Atom 呢？真的就是個 Hackable 的 Editor，十分適合拿來惡搞，只要你想，回到 90 年代也未嘗不可：

![](https://lh3.googleusercontent.com/79NInG4caoVFAZlGOUKajtxlw5692Y3dnQDHwDomsBGKoT3q4hOaYGnSUUQCTyvSJO7ikL1DvsAH6n81kDWsx-36QtQM8VnfK5R-aJiFqP2W5vdQpzfwKjw7iw771dzP_eWf9eMU2DYDVX-WAg_Q_V_LmmQDqJMyx9Jx_Rx05Td15P6hzAmnJhSXuHlKUtVg2dChf22kOwZU2AvtHEOa5qhbTBI2pu6nbUwEODaEKpX5MIG5D0r-fobk9NrgkdGrGr6ZdaLy0iFJtqVNdRB3JolAU3yttOgWG_Nfp3Rihn9YZ-WDexU_d4A9D-Vu241WKnl9B0Y9oZ9gVjC7vbfhkbliGI9hfLWuusUVCAMjdBfRcH1lohoLifYeEnDJz2m7Qa88wbJxmCdoK8pNy50fAks8KehaQYI4FcKHg6GvyBbCoxOqTbX37VcEj8KA7owoF9OaFjWnbM0ngWOrqvjz7xwjMsSQS0l_zZzmQZAmIT7Dw8IPZQ4yb_W-16-Qu3fdw3kjsGfPWpAnb1l2kPMpX-BPwjk8GCX7ao2cHdsJz1R_MDmnu-XtBPiNV05kXlfoTucjmCBnSWK4Agd5TE05YQRFtqWq-iJs=s800)
