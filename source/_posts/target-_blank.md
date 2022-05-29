---
title: target="_blank" 風險問題處理，加入 rel="noreferrer noopener"
date: 2020-10-20 01:33:06
tags:
---


target 是 <a> 標籤的屬性，點擊此連結會再新開一個分頁。在處理 eslint 提供的警告時，看到了這個。

![](https://static.coderbridge.com/img/MoreCoke/a2e73b0f540849aebdf05faf048898d0.png)

爬文後得知加入 `rel="noreferrer noopener"` 可以避免原本的網站被重新導向,如果沒加，有心人士可以在新開的分頁寫 js 用 `window.opener.location.href='網址'` 把原本的網址重新導向。記得網址要加 `https://`，不然會被視為網址參數的。

<!-- more -->

目前想到可以用來作釣魚網站，有看過澳門首家賭場的老司機就會知道，點擊的影片會導入新分頁而不是在原本的頁面播放，然後原本的頁面會重新載入到奇怪的遊戲網頁。

寫了個簡單的 [code](https://codesandbox.io/s/relnoreferrer-noopener-36c0t) [demo](https://36c0t.csb.app/)

### 參考資料

[javascript window.location not working
](https://stackoverflow.com/questions/35254564/javascript-window-location-not-working)
[[掘竅] 為什麼要使用 rel="noreferrer noopener"，談 target="_blank" 的安全性風險](https://pjchender.blogspot.com/2020/05/relnoreferrer-targetblank.html)
[rel=noopener 這回事](https://yungke.me/rel-noopener/)
