---
published: true
layout: post
title: 用 Gmail 的 SMTP 來寄信，但你的郵件伺服器沒有 SMTP 功能
---
## 你到底再工啥小

在啦幹。解釋一下，敝單位由於歷史因素及安全考量，所以內部的郵件系統在外網時，只開放收信（IMAP）而沒有寄信（SMTP）功能。這摸不方便的狀況一定要來 workaround，免費 Gmail 就有 SMTP 寄信功能，我們就直接用吧。值得一提的是這方法是我成功之後才去 Google 驗證解法的，難得一波直覺流 <3

## 設定步驟

1. 設定電子郵件客戶端的寄信伺服器為 Gmail 的 SMTP，伺服器填：`smtp.gmail.com`，帳號：`xxx@gmail.com`，密碼為 gmail 密碼。若你有兩階段驗證，密碼就去[應用程式密碼][2-factor]產生
2. 到 Gmail 的 **帳戶和匯入** -> **以這個地址寄送郵件** -> 點選 **新增另一個電子郵件地址**
3. 把**視為別名**給取消勾選、填入你要用來寄信的 mail
5. SMTP 伺服器欄位，填入 Gmail 的 SMTP（跟上面第一點填的一樣）
6. 收信驗證 mail
7. 搞定

最近 Gmail 準備要推出改版了，希望這部分的 UI 不要差太多。另外本篇就先不截圖了，非開發者比較需要 XD

via https://webapps.stackexchange.com/a/72975

[2-factor]: https://myaccount.google.com/apppasswords
