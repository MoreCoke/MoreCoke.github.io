---
title: 第一篇文章
date: 2020-04-28
---

## 為什麼要用 hexo 呢?

因為它免費，而且又能練習 git 的指令和 markdown 的寫法。
我主要使用 Medium 作筆記，雖然 Medium 程式碼呈現很差，但線上編輯很好用，加上它的簡潔風格我非常喜歡，目前沒有考慮要換平台。
其實我一直沒有很想架 hexo，我懶的用，而且 medium 用的好好的，可是又覺得不架好像會被別人認為我不會架，東想西想還是架吧!所以說~這次架 hexo 只是一時興起，搞不好寫個幾篇，會回到 medium 繼續寫吧。
<!-- more -->
## 目前想到的問題?

其實還滿多的，像是

* 試試把 medium 文章搬過來，應該有套件能用要找找。
* 圖片要另外存在資料夾還滿麻煩的，因為我圖片用完就刪掉，這樣還要放在本地端。目前想到的方法是放在 imgur 上。

圖片測試: 
![Imgur](https://i.imgur.com/d8haCa9.png)

影片測試:
我很常看 youtube 找資源學習，所以也常會把有幫助的影片貼上來，這邊簡單介紹怎麼貼，這邊用卑鄙源之助介紹。
youtube 網址長這樣 `https://www.youtube.com/watch?v=WVwlR_vni6k`，youtube 的 id 是 `?v=` 後面的亂碼。

方法1 : 用 [hexo 文件](https://hexo.io/zh-tw/docs/tag-plugins#iframe)的 iframe 模板，把網址中的 `watch?v=` 換成 `embed/` 就能用，有試過調寬高但沒效。
```swig
{% iframe https://www.youtube.com/embed/WVwlR_vni6k [640] [360] %}
```
{% iframe https://www.youtube.com/embed/WVwlR_vni6k [640] [360] %}

方法2 : 用 [hexo 文件](https://hexo.io/zh-tw/docs/tag-plugins#Youtube)的 youtube 模板。
```swig
{% youtube video_id %}
```
{% youtube WVwlR_vni6k %}
方法3: 用 [youtube 文件](https://developers.google.com/youtube/player_parameters#Embedding_a_Player)的 iframe ，同方法 1 ，把網址中的 `watch?v=` 換成 `embed/` 就能用了，也能調整寬高。原本以為寬高是寫死的，但用開發檢查工具看響應式發現不會破版，應該根部落格內部設定也有關，還要找找。
```html
<iframe id="ytplayer" type="text/html" width="640" height="360"
  src="https://www.youtube.com/embed/WVwlR_vni6k"
  frameborder="0"></iframe>
```
<iframe id="ytplayer" type="text/html" width="640" height="360"
  src="https://www.youtube.com/embed/WVwlR_vni6k"
  frameborder="0"></iframe>


codepen 測試:
使用 codepen 內建的 embed 功能就可以了，html 或 iframe 都行。
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="js,result" data-user="moreCoke" data-slug-hash="aboPGwB" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="print a number with commas as thousands separators in regular expression">
  <span>See the Pen <a href="https://codepen.io/moreCoke/pen/aboPGwB">
  print a number with commas as thousands separators in regular expression</a> by Donkle (<a href="https://codepen.io/moreCoke">@moreCoke</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## 部屬遇到的問題

### 路徑問題
repository 的命名當時是叫 myblog ，部屬上 github page 時因為路徑關係 css 和 js 載不出來，等了一個晚上一開始還以為是 github 的問題，後來看到這篇[從無到有的 Blog 建置教學 (含 domain 購買轉址)](https://riverye.com/2019/10/23/%E5%BE%9E%E7%84%A1%E5%88%B0%E6%9C%89%E7%9A%84-Blog-%E5%BB%BA%E7%BD%AE%E6%95%99%E5%AD%B8-%E5%90%AB-domain-%E8%B3%BC%E8%B2%B7%E8%BD%89%E5%9D%80/)，稍微有了概念。

如果是在 master 上那 repository 要用這樣的形式命名: (username).github.io ，其實在 [hexo](https://hexo.io/zh-tw/docs/github-pages) 就有說了，但看到文件說用 Travis CI 部屬，就跳過整段沒看了，就跟著文件下方的私人儲存庫做，然後兜了一大圈 debug ，還是找不到原因。

現在找到了，我根目錄的 yml 中的 url 沒配...。 
在[文件](https://hexo.io/zh-tw/docs/configuration#%E7%B6%B2%E5%9D%80)中找到答案，以我的為例:

```yml
url: https://github.com/MoreCoke
root: /myblog/
```
### 程式碼的複製功能怎麼做?
到 next 的 _config.yml 設定，[hexo：Next 一鍵複製程式碼](https://mtwmt.github.io/hexo/hexo_next_copybutton/)
```yml
copy_button:
    enable: true
    # Show text copy result.
    show_result: true
```