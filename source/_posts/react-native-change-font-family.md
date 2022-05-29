---
title: React Native 手動換字型
date: 2021-03-12 01:33:06
tags:
---


使用版本: 0.63
適用版本: 0.6 以上

React Native 在 0.6 版本後就不再支援 react-native link，所以要用其他方法換字型。

我是跟著這篇文章([Ultimate guide to use custom fonts in react native](https://medium.com/@mehrankhandev/ultimate-guide-to-use-custom-fonts-in-react-native-77fcdf859cf4))手動部分的教學做的，但還是遇到文章沒提到的問題，所以再另外寫這篇出來。

遇到的地雷:

* 使用自訂字型後[不能使用 fontWeight](https://github.com/facebook/react-native/issues/25852#issuecomment-646469383)或其他[會改變字體形狀的 style 可能也不行](https://stackoverflow.com/a/58765980)，不然無法套入字型
* 字型用 ttf 格式比較穩， otf 有時無法套入?
* 關於 Xcode 上介面操作問題，如果資料夾顏色是藍色而非黃色，代表 Xcode 沒有成功追蹤到該資料夾，字型會無法套入。(下面附圖和流程)

<!-- more -->

教學:
先去下載字型。

安卓部分滿簡單的，進到 `android/app/src/mains` 目錄下， 加入資料夾就能用了，我這裡建了兩層資料夾 `assets/fonts`。
![](https://static.coderbridge.com/img/MoreCoke/e100ed8677c34c04a5691e82e03eeec5.png)

iOS 的話比較麻煩，要到 Xcode 上面改，打開 ios 資料夾，點選 `.xcodeproj` 結尾的檔案。
![](https://static.coderbridge.com/img/MoreCoke/75bef777879743d6bf7ce75a93cde90d.png)

雖然 [Apple 開發者文件](https://developer.apple.com/documentation/uikit/text_display_and_fonts/adding_a_custom_font_to_your_app)說可以用拖曳檔案的方式加入，但我這樣做檔案沒辦法追蹤。
![](https://static.coderbridge.com/img/MoreCoke/2cfaaf0f723e49f0b864531cdaf7cfc1.png)
拖曳到 SampleProject 裡。
![](https://static.coderbridge.com/img/MoreCoke/bdc903fa871c4e8eaba4e71d0e7b5779.png)

資料夾顏色還是藍色的，代表 Xcode 沒有追蹤成功。
![](https://static.coderbridge.com/img/MoreCoke/32a22779ada247f1846a6ee57eb39acd.png)

把這個檔案刪掉重用。
![](https://static.coderbridge.com/img/MoreCoke/d8680f53b5ef461a98dbf054ac4fc9a7.png)

![](https://static.coderbridge.com/img/MoreCoke/fe8156bb2d894a65be759ac822c930c8.png)

用加入 `New group` 方式最穩最安全，只要資料夾顏色是黃色就代表成功了。
![](https://static.coderbridge.com/img/MoreCoke/b9b53330816d494882138ca537373612.png)

可以在右側欄位改資料夾名字。
![](https://static.coderbridge.com/img/MoreCoke/a5f21744a3eb4eddb0ab69716f60b6ef.png)

這時可以看到 fonts 的資料夾變成黃色的，代表成功被 Xcode 追蹤了。接著我們對這資料夾按右建再將字型檔案加入。

![](https://static.coderbridge.com/img/MoreCoke/1479a9e5dac54c19bec86e835daab35b.png)
![](https://static.coderbridge.com/img/MoreCoke/3fe1a96e96334f5ea67a0d41e5e9b667.png)

接著麻煩的地方來了，找出 `Info.plist` 這個檔案。
![](https://static.coderbridge.com/img/MoreCoke/21fb6f95a1b5421198229d7c2772feec.png)

會進到這個列表，點選 + 號的按紐。
![](https://static.coderbridge.com/img/MoreCoke/2549c75d98384971b530bbcb1d22ce6f.png)

新增這個欄位 `Fonts provided by application`， 然後再把字體的檔案名打上去。
![](https://static.coderbridge.com/img/MoreCoke/dc51e44656534334a64e94e3c0e135c3.png)

回到 vscode 會看到以下的更改:

![](https://static.coderbridge.com/img/MoreCoke/8ee3bbfd0aab4c9e8476147156769c8b.png)

![](https://static.coderbridge.com/img/MoreCoke/cf1d6ce72ca447beb77089fb5479a7ee.png)

通常 `cmd + r` 重新整理就能看到新的字體了，沒有的話就關掉模擬器重跑 `react-native run-ios --simulator='iPhone 12 Pro'`。安卓也是同樣道理，可以先到 android 資料夾使用指令清空暫存 `./gradlew clean` ，如果不行再重開 `react-native run android`。
