---
title: JS 時間日期處理
date: 2021-07-25 01:33:06
tags:
---

## 時間名詞介紹

- UTC: 衛星、航空等等都採用該時間規範。
- GMT: 格林威治時間，基本上一般使用上和 UTC 時間差不多。
- UNIX: 是指 unix系統表示的時間，以 UTC +0 時區 1970 年 1 月 1 日 0 時 0 分 0 秒 開始的**秒**數來記錄時間。
- timestamp: 以 UTC +0 時區 1970 年 1 月 1 日 0 時 0 分 0 秒 開始的**毫秒**數來記錄時間。
- ISO: 日期字串格式。

<!-- more -->

### 參考資料

[到底是 GMT+8 還是 UTC+8 ? - PanSci 泛科學](https://pansci.asia/archives/84978)

## js Date 物件常用內容

### new Date() 參數

- 能塞毫秒數
- 或是時間字串。

getMonth， getYear 滿多坑的，我也很少用就不說了。要注意的是這些 method 都會把時間轉成**當地時間**。

### new Date().toISOString()

轉成 ISO 規範的日期字串。

### new Date().getTime() 和 Date.now()

都是取得當前毫秒數，以 UTC +0 時區 1970 年 1 月 1 日 0 時 0 分 0 秒 開始的**毫秒**數來記錄時間。

## 正確的日期字串格式 ISO 8601

只要遵守這些規定，在同個時區下使用不同瀏覽器的呈現結果都是一樣的。

- T: 可以想成是時間的開頭。
- Z: 代表 +0 時區(zero hour offset) 。在軍事中使用 Z 代表 UTC 時區，在無線電用語中 Z 代表 Zulu 所以又稱 Zulu time。Z 也能用 +00:00 或 -00:00 的方式表示不同時區的時間。
- sss: 代表毫秒，1000 毫秒等於一秒，記得毫秒前的字串**是點點**不是冒號。

![](https://i.imgur.com/5l7LTh4.png)

關於 Z 的例子:

現在台灣時間是 2021 年 7 月 25 日下午 4 點 34 分，套用格式會是  `2021-07-25T16:34:16.976+08:00` ，等同 +0 時區的 2021 年 7 月 25 日早上 8 點 34 分 `2021-07-25T08:34:16.976Z` 。

### 參考資料:

[ECMAScript 2015 Language Specification - ECMA-262 6th Edition](https://262.ecma-international.org/6.0/#sec-date-constructor)

[[已解決] UTC, GMT, CST是什麼的縮寫？Zulu Time又是什麼意思？](https://english.bruceli.net/2011/09/utc-gmt-cstzulu-time.html)

## 日期字串解析慎用!

new Date('dateString') 的用法等同 Date.parse('dateString')。文件上說不鼓勵使用，但沒解釋很清楚，意思是說如果你沒使用正確的字串格式在不同瀏覽器上可能會有非預期的結果。

![](https://i.imgur.com/xesDJ34.png)


```jsx
new Date('2021/07/20 13:00')
// 在 chorme 上: Tue Jul 20 2021 13:00:00 GMT+0800 (GMT+08:00)
// 在 safari 上: Invalid Date
```

這字串並不符合正確格式，所以 safari 說是無效日期，而 chrome 之所以能出來是因為它在背後幫我們做了很多事，只要字串不要錯得太離譜都能解析出來。

### 參考資料

[Date - JavaScript | MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Date)

[Converting a string to a date in JavaScript](https://stackoverflow.com/questions/5619202/converting-a-string-to-a-date-in-javascript)

## 踩到的坑

所以 chorme 就沒問題嗎?並沒有，像是下方的時間都是 2021 年 7 月 20 日，但是出來的時間居然差了 8 小時?

```jsx
new Date('2021-07-20') // Tue Jul 20 2021 08:00:00 GMT+0800 (GMT+08:00)
new Date('2021/07/20') // Tue Jul 20 2021 00:00:00 GMT+0800 (GMT+08:00)
```

回去翻 Date.parse() 文件在字串格式中有給對應的解釋，如果沒給時區，那麼在時間部分會自動用當地時間。

第一個字串符合 ISO 格式，台灣是 +8 時區，所以加了 8 小時。

第二個字串則被當成當地時間來解析。

![](https://i.imgur.com/yOD9pWs.png)

### 參考資料

[淺談 JavaScript 中的時間與時區處理](https://blog.techbridge.cc/2020/12/26/javascript-date-time-and-timezone/) 標題: 日期時間需注意的地方

[Date.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse#date_time_string_format)

## 時區處理

目前還沒遇到，之後再研究。

## 其他資料

[竹白記事本: 日期時間](https://chupai.github.io/posts/200516_js_date/)

[The Will Will Web](https://blog.miniasp.com/post/2016/09/25/JavaScript-Date-usage-in-details)