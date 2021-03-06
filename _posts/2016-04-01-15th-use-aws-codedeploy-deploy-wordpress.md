---
layout: post
title: '第十五篇 - 使用 AWS CodeDeploy 跑起你的 WordPress - 官方教學筆記'
date: 2016-04-01 14:46
comments: true
categories: [aws, codedeploy, wordpress, official, tutorial, workthrough, notes]
---
為何要部屬 WordPress？就因為他是 [CodeDeploy][1] Tutorial 的第一個範例。AWS Documention 有個特點，就是到處 Reference 來 Reference 去，你很難在單一流程完整跑完所有的任務，非得要搭著其它份文件一起對照。筆者英文也沒那麼好，看著看著就會漏東漏西，文件又滿滿的都是字，沒有圖片可以參考，真令人崩潰。

CodeDeploy 是 AWS 眾多部屬方案中，看起來比較單純的一種。寫好 Setup 程式的 Shell Script，再聲明好 `appspec.yml`，定義每個 deploy life cycle 不同的階段要跑的動作，用 AWS CLI Tool 把 repo 打包上傳到 S3，再到 CodeDeploy console 定義應用程式部屬規則、後臺發起一個 deployment（一個部屬行動），CodeDeploy Service 便會把你的 code 部屬到你設定的 Deployment Group，依照 Group 設定的機器不同，你可以在同一個機器部屬很多個 Application，或是同一個 Application 部屬到很多台機器，端看設定，啦。

當然，AWS 系列就是啥都可以用 CLI 跑，只要開好對應權限的 role 並拿到 Access ID 和 Secret Key，我們也可以交由第三方 CI 服務，比如說 CodeShip，來幫我們完成「打包程式碼並上傳 S3」、「發起部屬行動」這種的工作。

以下便大致記錄一下本日折騰的感想與流程。

<!--more-->
## 概念

CodeDeploy 約略是 14 年 11 月推出的服務，在此幹一張官圖，看圖說故事。
![CD](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/sds_architecture.png)

1. 在你的開發機器上寫好 code 、設定好 appspec.yml，上傳到 github 或是打包傳到 s3
2. 在 CodeDeploy 設定好部屬邏輯
3. 每一台 instance 的 agent 把 codebase 從 s3 或 github 拉下來
4. 跑部屬 script！裝裝裝

大概醬，英文版可以看[這][3]，另外強烈建議 aws 系列最好都先從 [Concept][4] 開始看，要不然鐵定被無限連結地獄搞死，崩潰。

## 名詞解說

可以開好[官方教學][5]的分頁，淨身準備一下，接下來講解教學中會出現的名詞。其實名詞解釋放在教學開頭實在是挺不好的，我自己跑教學也是跟著實作之後才有點感覺，在還沒進行操作前做的說明都是廢話。話雖然是這樣說啦，可是這是筆記又不是演講，你翻到下面還是可以翻回來看啊，所以我還是寫在這了。呵呵。滿多中文名詞都我隨便翻的 XD，中文文件越少，我就掌握著更多的話語權（才怪），大家趕快來寫文件吧哈哈。

AWS 在各種服務開 API 開很大開不用錢的狀況下，權限的管理便十分重要，阿罵爽雲端福利社使用 IAM 角色權限管理服務，來區分出不同服務或使用者，對於 AWS 服務使用的權限隔離。舉例來說，CodeDeploy 在 AWS 裡算是一個服務(Service)，雖然通通都是在 AWS 裡啦，但是你還是要給他一些對 AWS 資源的存取權限才可以，比如說當我們對好幾臺 EC2 Instances 設定了標籤（Tag），我們也要給 CodeDeploy 讀取 EC2 Intance 標籤的權限，CodeDeploy 才知道要部屬到哪幾台機器上啊，這就是權限管理的作用。

不同的權限，在 IAM 裡就叫做 Policy（方針）。當人有了多項 Policy 之後，能力越大，責任越重，你所要承擔的就是職責了（屁話），在 IAM 裡我們把他稱作角色職責 (Role）。在 AWS 裡面有不同的服務，Role 的類型也都不同，比如 EC2 Instance 的 Role 就叫 Instance Profile，用來管理 EC2 能有那些權限，在 CodeDeploy 官方教學裡，EC2 的職責是把用 agent 把 codebase 從 s3 拉下來，所以在啟動 Instance 之前，我們必須先設定好能夠存取 S3 的 Instance Profile，並在啟動(Launch) Instance 把 Role 指派給他。

![](https://i.imgur.com/0ulM7UH.png)
不同的 Role Type。CodeDeploy 教學中用到的分別是 CodeDeploy 和 EC2 的 Role Type。

![](https://i.imgur.com/on614NX.png)
CodeDeploy 的 Service Role，直接掛上 AWS 建立好的 Policy，可以直接看出使用了那些權限。

## 跑教學...咦

我前面嫌的要死，就因為這次跑教學真的打死也找不到安裝 CodeDeploy agent 的步驟，反反覆覆翻了教學幾次，卻一直找不到哪裡出錯。以下來回顧一下。

首先，[照著教學][5]的說明先在 EC2 Launch 一個 Instance。他第一步教學的作用其實就是給你一個預先裝好 aws cli tools 的環境，好使用 cli 界面打包上傳 code 到 s3，用你自己的電腦其實就可以了。設定好 Keypair，透過 pem 檔 ssh 進機器後，先來[安裝 codedeploy agent 到機器上][6]。裝到一半你會發現連 agent 的 aws s3 bucket 都抓不下來，原來是我們還沒設定 EC2 的 [Instance Profile][7]，也就是 EC2 類型的 Role。請去 IAM 設定，並如同連結裡面加上 S3 的存取權限。這權限一定要開，即使現在用 wget 直接拉 bucket 的 agent install script 下來裝完，之後要從 s3 部屬時沒開也沒法抓啊。

```json
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": [
          "s3:Get*",
          "s3:List*"
        ]
      }
    ]
  }
```
_範例 S3 的 widecard 權限，當然還是詳細指定比較好。先新增 Policy，在建立 EC2 Type 的 Role Attach 上去，或是建立 Role 時寫成 Inline Policy。_

是的。就是這兩點搞死我。**第一，EC2 Role 要先設定 S3 存取權，並在 Instance 建立時掛載。第二，CodeDeploy Agent 要先裝上 Instance**。

教學是都搜尋的到啦，可是排成一個完整版不好嗎，都已經拆 Step by Step 了，還 Lost 一堆，崩潰啦，難怪各種 AWS 代管服務蓬勃發展啊無誤 XDDD

## 再跑一次

* 首先在 IAM 建立兩個 Role ，一個是給 EC2 的，一個是給 CodeDeploy 的。EC2 Role 直接用上面提到的 S3 Bucket 權限寫進去，EC2 的 Role 又稱 Instance Profile，呵。CodeDeploy 的 Role 直接掛上官方的 CodeDeployPolicy，如圖：
![](https://i.imgur.com/F15UVaN.png)

* 依照教學 Launch Amazon 的 Linux Instance，記得掛上剛剛建立的 EC2 Role。成功 ssh 連進去後安裝 [code deploy agent][6]，ubuntu 就選 apt 的那個，Amazon 自家 Linux 就選 yum 的那個，把他裝起來。
* 依照教學，把 WordPress 的 Repo 載下來，建立相應的 init script 還有 `appspec.yml`。
* 依照教學，利用 aws cli 建立 application，上傳 s3，這邊依照教學要開一份 IAM 的 User，掛上 s3 put 的權限，下載 User 的 credential (id & secret key)，跑 aws configure 填入設定，這樣才可以在這臺暫時的工作機上傳檔案到 s3。依照教學打包 WordPress 上傳 s3。
* 到 CodeDeploy Console 建立 Deploy 設定。設定 DeployGroup，權限記得掛上之前建立的 CodeDeploy Service Role，其它照著跑。
* 建立部屬行動(Deployment，the 名詞)，看有沒有部屬成功。假如連 Events 都沒顯示，大概是 IAM 權限設定有問題，看一下 EC2 Role 和 CodeDeploy Role 有沒有設定正確

大概就這樣啦，這還只是 WordPress 而已，我要部屬的可是 Rails App 欸，之後想必有更細節的其它地方。這樣跑完了一次（還順便被雷了一下午），對 AWS 的理解就更深了，請期待之後的後續文章，哈哈哈（不要說一說就不寫了）

（完）


[1]: http://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html
[2]: http://www.slideshare.net/masonmei37/aws-59233295
[3]: http://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html
[4]: http://docs.aws.amazon.com/codedeploy/latest/userguide/concepts.html
[5]: http://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-set-up-instance.html
[6]: http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-run-agent.html
[7]: http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-configure-existing-instance.html