---
title: querySelector 和 getElement 差別
date: 2021-04-12 01:33:06
tags:
---

querySelector 取得的 NodeList 是靜態的， getElement 取得的 NodeList 是動態的。

<!-- more -->

### 以下這兩段寫法會得到什麼結果呢?

```html
<ul>
  <li></li>
</ul>
```

```js
var list = document.querySelector('ul');
var listChild = list.querySelectorAll('li');
for(var i = 0; i < listChild.length; i++){
  const li = document.createElement("li");
    list.appendChild(li);
  console.log(i);
}
```
Ans: 只跑一次迴圈。

```js
var list = document.querySelector('ul');
var listChild = list.getElementsByTagName('li');
for(var i = 0; i < listChild.length; i++){
  const li = document.createElement("li");
    list.appendChild(li);
  console.log(i);
}
```
Ans: 無窮迴圈。

基本上 NodeList 是會動態變化的，透過 querySelector 的方式可取得靜態的 NodeList，可以想成存在 `var listChild` 變數內的資料 `list.querySelectorAll('li')` 會是取得當下的狀態，當時 ul 下只有一個 li，所以長度只會是 1；用 getElement 系列的方式會直接取得動態的 NodeList，所以每跑完一次迴圈時， listChild.length 也會加 1，進而造成無窮迴圈。

附上 [codepen](https://codepen.io/moreCoke/pen/ZELrOqv?editors=1111)。

### 題外話~跟 querySelector 和 getElement 無關，這樣寫會怎麼樣呢?

```js
var list = document.querySelector('ul');
var listLength = list.querySelectorAll("li").length;

for(var i = 0; i < listLength; i++){
  const li = document.createElement("li");
    list.appendChild(li);
  console.log(i);
}
```
Ans: 只跑一次迴圈。

```js
var list = document.querySelector('ul');
var listLength = list.getElementsByTagName('li').length;

for(var i = 0; i < listLength; i++){
  const li = document.createElement("li");
    list.appendChild(li);
  console.log(i);
}
```
Ans: 只跑一次迴圈。

用 getElement 不是會動態產生 NodeList 嗎?怎麼不會跑無窮迴圈?
其實跟這無關，這是存變數的問題，這樣寫是因為我們事先將 ul 中 所有 li 的長度記錄起來了，當時存變數時長度就是 1。
所以想避免無窮迴圈的話也能透過改變存變數的方式避免無窮迴圈。

一開始我這樣寫 code 想試著讓 getElement 跑出無窮迴圈結果都沒有，找了很久都不知到哪裡有問題，紀念一下這錯誤，花了我兩個小時 debug。



### 參考資料

[querySelectorAll 方法相比 getElementsBy 系列方法有什么区别？](https://www.zhihu.com/question/24702250)

[重新認識 JavaScript: Day 12 透過 DOM API 查找節點](https://ithelp.ithome.com.tw/articles/10191765)