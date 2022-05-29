---
title: 瀏覽器 CORS 與同源政策介紹
date: 2021-05-03 01:33:06
tags:
---

### 同源政策是用來幹嘛的?

用來阻擋不同源網站間的存取，程式碼非同步請求依舊會送出，但是回傳的資料會被瀏覽器給擋下來，造成資料回傳 undefined，程式碼拿不到資料沒辦法做事，最後網頁報錯。

在瀏覽器中只有同源的網站間在互相存取資料時不會有限制，不同源的網站會被阻擋回應(response)，這樣做是基於資安考量，如果讓任何人可以存取、修改網站資料，是件危險的事，這樣就像大家都有你家的鑰匙，可以隨意進出你家，吃你冰箱內的零食那樣。而同源政策就像你家那能上鎖的大門，在資安上做到第一層防護。

<!-- more -->

### 同源不同源的定義?

同源要看三個地方，如果這三個都一樣才能算是同源:

* 協定(protocol)
* 主機(host)
* 埠號(port)

因為我不希望網址自動轉成連結，所以加了些空格並在不同的地方加上粗體辨識。

| 網址 1 | 網址 2 | 同源嗎? |
| ----------- | ----------- | -------- |
| http : //morecoke.com/ **home**     | http : //morecoke.com/ **company**      | 同源     |
| **https** : //morecoke.com/ home     | **http** : //morecoke.com/ home      | 不同源(協定不同)     |
| https : //**morecoke.com**/ home     | https : //**applecoke.com**/ home      | 不同源(主機不同)     |
| https : //morecoke.com/ home     | https : //morecokes.com**:3000**/ home      | 不同源(埠號不同)     |

### 瀏覽器允許的跨網域存取

這邊說幾個常用的:
`<script>`: 附上連結就能使用該函式庫，不用 npm 下載，像是 React 等等。

```html
  <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>
```

`<link>`: 附上連結可以直接使用 css 的 class，像是 Bootstrap。

```html
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
```

`<img>`:可以直接插入線上圖片。
`<video>`:可以直接插入線上的影片。

### CORS

是對於跨網域(不同源)網站存取資料所做的規範，存取資料(call api)這件事又分成兩種請求，一種是簡單請求，另一種不是簡單請求(因為在文件上沒看到對這請求的命名，只有說這是有副作用的請求，為求方便我叫它風險請求)。
在 CORS 的規範下我們透過瀏覽器送出簡單請求和風險請求，伺服器都會回傳這個 `Access-Control-Allow-Origin` 東西，告訴瀏覽器伺服器允許哪些網址存取資料。
`Access-Control-Allow-Origin: *`: 這是說允許任何網站存取。
`Access-Control-Allow-Origin: https://foo.example`: 這意思是只有 foo.example 這網站才能存取。

### 預檢請求是什麼?

瀏覽器會針對有風險的請求(API)先送出預檢請求跟伺服器打個招呼，伺服器確定這請求沒問題的話會回傳回應(response)給瀏覽器，如果伺服器回應給過，瀏覽器才會送出真正的(有風險的)請求，不然就不會送。
預檢請求會利用 http method option 的方式傳送，他會告訴伺服器風險請求的 header 會有什麼和它的 http method 會是什麼，以下是文件提供的範例，`Access-Control-Request-Headers: X-PINGOTHER, Content-Type` 會告訴伺服器待會要送的風險請求 header 會有 `X-PINGOTHER` 和 `Content-Type`，`Access-Control-Request-Method`  則會說 http method 是 POST。
> OPTIONS /resources/post-here/ HTTP/1.1
> Host: bar.example
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
> Accept-Language: en-us,en;q=0.5
> Accept-Encoding: gzip,deflate
> Connection: keep-alive
> Origin: <https://foo.example>
> Access-Control-Request-Method: POST
> Access-Control-Request-Headers: X-PINGOTHER, Content-Type

然後伺服器會根據這些資訊回傳這是否為合理的請求，`Access-Control-Allow-Headers` 說伺服器允許哪些 header，`Access-Control-Allow-Methods` 說伺服器允許哪些 http method。
> HTTP/1.1 204 No Content
> Date: Mon, 01 Dec 2008 01:15:39 GMT
> Server: Apache/2.0.61 (Unix)
> Access-Control-Allow-Origin: <https://foo.example>
> Access-Control-Allow-Methods: POST, GET, OPTIONS
> Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
> Access-Control-Max-Age: 86400
> Vary: Accept-Encoding, Origin
> Keep-Alive: timeout=2, max=100
> Connection: Keep-Alive

### 參考資料

* [同源政策 (Same-origin policy)](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy)
* [OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)
* [跨來源資源共用（CORS）](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)
* [網站安全🔒 Same Origin Policy 同源政策 ! 一切安全的基礎](https://medium.com/%E7%A8%8B%E5%BC%8F%E7%8C%BF%E5%90%83%E9%A6%99%E8%95%89/same-origin-policy-%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96-%E4%B8%80%E5%88%87%E5%AE%89%E5%85%A8%E7%9A%84%E5%9F%BA%E7%A4%8E-36432565a226)
