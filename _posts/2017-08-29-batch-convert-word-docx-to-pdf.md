---
published: true
title: 用 PowerShell 批次轉換 docx 到 pdf
---
## 近況

內容與標題不符。

我從今年的八月[開始登入](https://alive.yukaii.tw/)替代役 online。替代役自由多了，回想起成功嶺的那兩周，不僅餐餐吃不飽，還因為腸胃炎而再次掉了兩公斤多 Zzz，實在有夠悲劇。

可能因為這麼悲慘的運氣補償機制吧(喂)，我來到了國訓中心開始了一年的役期，也多了些「閒暇時間」可以利用，無論如何都是非常幸運了 QAQ

進入正題吧

## 批次轉換 docx 成 pdf

需求很簡單，也有現成軟體可以達成，不過身為一個開發者，連文書也要做的潮才行。以往一直沒什麼機會在 Windows 下進行自動化(多半是當時還太菜)，有了這個機會當然要好好利用。

雖然沒寫過 PowerShell，不過 Google 第一項結果在 [StackOverFlow](https://stackoverflow.com/a/16537996) 就有了，輕鬆愉快。

### 啟用腳本執行權限

首先設定讓 powershell 可以執行腳本，以系統管理員身分執行 PowerShell 並跑起以下指令：

```cmd
Set-ExecutionPolicy RemoteSigned
```

### 寫 PowerShell 腳本

然後把以下腳本存成副檔名為 `.ps1`，比如 `word-to-pdf.ps1`。其中 `$documents_path` 改成目標資料夾路徑。

```powershell
# From https://stackoverflow.com/a/16537996

$documents_path = 'C:\Users\user\Documents\blahblahblah'
$word_app = New-Object -ComObject Word.Application

# This filter will find .doc as well as .docx documents
Get-ChildItem -Path $documents_path -Filter *.doc? -Recurse | ForEach-Object {
    $document = $word_app.Documents.Open($_.FullName)
    $pdf_filename = "$($_.DirectoryName)\$($_.BaseName).pdf"
    $document.SaveAs([ref] $pdf_filename, [ref] 17)
    $document.Close()
}

$word_app.Quit()
```

注意的是在 `Get-ChildItem` 方法我加上了 `-Recurse` 遞迴參數，讓所有子目錄底下的 `docx` 都能被轉換到。

點擊兩下執行，所有 pdf 就會慢慢產生在 `docx` 檔的旁邊啦，瞬間開始耍廢！(喂)

之後若有更多的 PowerShell 輔助使用腳本，我都會放在 [powershell-study](https://github.com/Yukaii/powershell-study) 這個 repo，歡迎自行取用 :p

(完)
