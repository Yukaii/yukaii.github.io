---
layout: post
title: "git cherry-pick 小技巧"
---

簡單記錄下今天用到的小技巧。

有的時候會加入多個 git remote，比如說 upstream/origin 或 public/private 等等，針對不同釋出版本用版控劃分，但又想利用 git 維持兩者間的部分差異。這時候就可以用 cherry-pick 指令批次套用 commit 到目前的程式碼基礎上。

使用版本控制系統的好處，就是把每次「修改」都視為可以任意放出收回的，呃，**招式**。這就是為啥 commit 粒度切的越細越好啦。

```bash
git cherry-pick COMMIT_SHA1
```

或是你想要 cherry-pick 多個 commit 也行：

```bash
git cherry-pick START_SHA1^..END_SHA1
```

多加了一個 `^` 是表示開始 SHA1 的前一個 commit。

假如噴出了錯誤，那就手動解決完 merge conflicts，再用 `git cherry-pick --continue` 繼續套用剩餘的 commit。

或是失敗想回到還沒 `cherry-pick` 的狀態，用 `git cherry-pick --abort` 就好。

大 GUY 4 這樣
