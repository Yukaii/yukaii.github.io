---
layout: post
title: '第五篇 - 在 Ubuntu 中安裝 Unity3D Game Engine'
date: 2014-06-28 09:39
comments: true
categories: [Unity3D, wine, playonlinux, 再戰十年, 跳坑]
---

![2014-06-28 18:09:58 的螢幕擷圖.png](http://user-image.logdown.io/user/1128/blog/1112/post/207756/qtAMPvNFQrGbmhSiYmQz_2014-06-28%2018:09:58%20%E7%9A%84%E8%9E%A2%E5%B9%95%E6%93%B7%E5%9C%96.png)
首圖炫耀下。

最近準備再來弄一個Unity3D專案，但筆電又被我灌回了Ubuntu 14.04，在考慮行動性下，優先在Ubuntu環境下安裝Unity3D。    

環境：

* Ubuntu 14.04
* Unity 4.5.1f3

<!--more-->

## 安裝

基本上參考[這篇](http://tomaszzackiewicz.wordpress.com/2013/11/16/run-unity3d-on-linux-with-wine-solved/)

裝[playonlinux](http://www.playonlinux.com/en/)（POL），裝Wine 1.7，設置playonlinux，準備好Unity3D安裝檔，使用該篇提供的POL script完成安裝。（附於本文文末）

## 安裝後問題


> unable to find a version of the runtime to run this application

> wine: cannot find L"C:\\windows\\Microsoft.NET\\Framework\\v4.0.30319\\mscorsvw.exe"

和

>	DNSAPI.dll的讀取問題（錯誤代碼找不到orz...）

兩個問題。

以上問題皆因為不明原因，函式庫設定跑掉。
進入wine配置函式庫，改下相應的：

![2014-06-28 18:30:01 的螢幕擷圖.png](http://user-image.logdown.io/user/1128/blog/1112/post/207756/H4qG6XkyRUqHJ6HRl18Z_2014-06-28%2018:30:01%20%E7%9A%84%E8%9E%A2%E5%B9%95%E6%93%B7%E5%9C%96.png)

![2014-06-28 18:30:19 的螢幕擷圖.png](http://user-image.logdown.io/user/1128/blog/1112/post/207756/S6yZ2vCvS5aIAKhwQHBP_2014-06-28%2018:30:19%20%E7%9A%84%E8%9E%A2%E5%B9%95%E6%93%B7%E5%9C%96.png)

## 使用問題

已知Assest Store開了無畫面。

再來就是Project存的位置不能有中文，可能是wine的問題吧。

我是透過在桌面設一個捷徑解決：

{% include image.html url="http://user-image.logdown.io/user/1128/blog/1112/post/207756/WSyankWTyGYKpW07sBLp_2014-06-28%2021:12:39%20%E7%9A%84%E8%9E%A2%E5%B9%95%E6%93%B7%E5%9C%96.png" description="（萬里花的大腿，prpr）" %}


然後Unity開專案時存到此捷徑：

![2014-06-28 21:14:48 的螢幕擷圖.png](http://user-image.logdown.io/user/1128/blog/1112/post/207756/hk8Oh894SymeXgEZs0zT_2014-06-28%2021:14:48%20%E7%9A%84%E8%9E%A2%E5%B9%95%E6%93%B7%E5%9C%96.png)

## playonlinux Script

以下script來自[原帖](http://tomaszzackiewicz.wordpress.com/2013/11/16/run-unity3d-on-linux-with-wine-solved/)的第七版，請複製另存成`XXX.pol`。

```sh
#!/bin/bash
 
# Date : (2014-03-07 10-21)
# Last revision : (2014-03-07 10-21)
# Wine version used : 1.7.14
# Distribution used to test : OpenSuse 13.1
# Authors : waneck-six, Damian-LinuxFan, Tomza (<a class="linkclass" href="mailto:pogtoma@gmail.com">pogtoma@gmail.com</a>), gnumaru, Doctor Jellyface (<a class="linkclass" href="mailto:doctorjellyface@riseup.net">doctorjellyface@riseup.net</a>)
# Contributors: Goran Grncaroski (running MonoDevelop), other people (testing solutions)
# Script licence : GPL v.2
# Only For : </span><a href="http://www.playonlinux.com" target="_blank" rel="noreferrer" style="cursor:help;display:inline !important;">http://www.playonlinux.com</a>
# Depend :
 
# Known Issues : You can't use Asset Store
 
[ "$PLAYONLINUX" = "" ] exit 0
source "$PLAYONLINUX/lib/sources"
 
TITLE="Unity 3D"
PREFIX="Unity3D"
WINE_VERSION="1.7.14"
 
POL_SetupWindow_Init
POL_Debug_Init
 
POL_SetupWindow_presentation "$TITLE" "Unity" "<a href="http://www.unity3d.com/" target="_blank" rel="noreferrer" style="cursor:help;display:inline !important;">http://www.unity3d.com/</a>" "waneck-six, Damian-LinuxFan, Tomza, gnumaru, DoctorJellyface" "$PREFIX"
 
#create prefix
export WINEARCH="win32"
POL_Wine_SelectPrefix "$PREFIX"
POL_Wine_PrefixCreate "$WINE_VERSION"
 
#setup prefix
POL_Wine_InstallFonts
POL_Call POL_Install_d3dx9
POL_Call POL_Install_dotnet20
POL_Call POL_Install_dotnet35
POL_Call POL_Install_dotnet40
POL_Call POL_Install_tahoma
POL_Call POL_Install_vcrun2008
POL_Call POL_Install_vcrun2010
POL_Call POL_Install_mono210
POL_Call POL_Install_d3dx9_36
POL_Call POL_Install_d3dcompiler_43
POL_Call POL_Install_dxdiag
POL_Call POL_Install_dxfullsetup
POL_Call POL_Install_physx
POL_Call POL_Install_corefonts
POL_Call POL_Install_msxml6
POL_Call POL_Install_wininet
POL_Call POL_Install_ie8
 
#Setting OS wer
Set_OS "winxp" "sp3"
 
#Setting mono forcing in MonoDevelop
POL_Wine_OverrideDLL “native, builtin” “mscore”
POL_Wine_OverrideDLL “builtin, native” “dnsapi”
POL_Wine_OverrideDLL "" "mscorsvw.exe"
 
mkdir -p $WINEPREFIX/drive_c/users/$USER/AppData/LocalLow
 
#registry
POL_Wine reg add "HKLM\Software\Microsoft\Windows NT\CurrentVersion" /v ProductId /t REG_SZ /d 12345-oem-0000001-54321
 
POL_SetupWindow_browse "Please select the location of the Unity3D setup executable" "$TITLE"
 
UNITYLOC=$APP_ANSWER
POL_Wine $UNITYLOC
 
POL_Shortcut "Unity.exe" "$TITLE"
 
POL_SetupWindow_Close
exit
```

以上。
