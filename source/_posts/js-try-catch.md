---
title: JS try catch 使用方式
date: 2021-03-03 01:33:06
tags:
---

### try catch 的用途

try catch 的使用時機就是避免程式碼出錯造成網頁或是 app crash 的緊急處理，所以也會有人稱 try catch 為錯誤處理，但我個人並沒有很喜歡這個稱呼，我覺得叫做「例外處理」比較貼切，至於原因下面會說。

<!-- more -->

### 介紹 try catch 語法

```jsx
try {
 要做的事情
} catch(e) {
 例外處理
}
```

這個語法會先將 try 區塊中的程式碼執行一次，如果裡面的程式碼有問題就會立刻中止執行，執行步驟會直接跳到 catch 區塊的程式碼，如果 catch 的程式碼又有問題，那網頁就會跳錯。

### 介紹 throw

上面說到如果程式碼遇到問題會終止執行，而這個問題大部分都是我們在瀏覽器上看到的編譯錯誤，最常看到的錯誤有:

- js 的語法錯誤(SyntaxError)，像是 `Unexpected token.`。
- 變數名稱拼錯(ReferenceError)，像是   `xxx is undefined.`。
- 找不到 function(TypeError)，像是 `xxx is not a function.`。

瀏覽器會將這些編譯成 Error 物件， 這個物件會被自動拋出(就算不加 throw 也會被拋出)，終止編譯，然後我們就會看到網頁一片空白，開發者工具出現錯誤提示。

所以就算在 try catch 中不用 throw 丟出 Error 物件， catch 也能捕捉到。

```jsx
try {
  new Error('hi');
}catch(e){
  console.log(e); // Error: hi
}
```

throw 不是只能丟 Error 物件而已，他能丟任何東西，丟 string 、function 等等都行，所以不要誤會了，覺得 try catch 錯誤處理這樣念起來很順口，就覺得他只會處理 Error 物件而已，任何你認為是例外的東西，都能透過 throw 丟出來讓 try catch 處理。

```jsx
try {
  throw 123;
}catch(e){
  console.log(e); // 123
}
```

### throw 和 return 的差別

兩者都能中斷程式碼執行，那是不是用哪個都沒差呢?

先說 return ，只能在 function 中使用，用來回傳值並中斷 function 執行。

回傳值。

```jsx
function test(){
  return 123;
  xxxxxx // 這邊不會執行
}
test(); //123
```

如果沒寫會回傳 undefined。

```jsx
function test(){
  return;
  xxxx //這邊不會執行
}
test(); // undefined
```

再來是 throw，任何地方都能用，用來拋出例外並中斷程式碼執行。在瀏覽器會用紅色來呈現例外。

```jsx
throw 123; // Uncaught 123
```

不能單獨使用，一定要丟出東西，不然會出錯。

```jsx
throw; // Uncaught SyntaxError: Unexpected token ';'
```

最後來整理一下:

- 兩者都能中斷程式碼。
- return 只能在 function 中使用， throw 沒限制。
- return 後面可以不用寫值， throw 後面一定要有值。

### finally 語法

接在 try catch 後面，可加可不加，在執行 try 或 catch 區塊後的程式碼執行，如果在 try 或 catch 中使用 return 並不會中斷程式碼執行，而是將 try 或 catch 裡的 return 移到 finally 區塊的程式碼最後面執行，看 code 會比較清楚:

```jsx
function test() {
  try {
		return 1;
	}catch(e){
		return 111;
	}finally{
		return 222;
	}
}
test();
```

最後會回傳的內容是 222，對 finally 來說，程式碼會長這樣:

```jsx
finally{
	return 222;
	return 1;
}
```

### 跟 if else 的差別

只單看文字的話可以很清楚的知道 if else 和 try catch 是完全不同的，if else 單純的是非判斷， try catch 則是對於意料外的行為所做的處理。

然而我還滿常搞混的，我覺得這兩個有時候用起來很像，因為我有時寫 if else 時的想法是 if 用來處理可預期的行為， else 用來處理其他沒考慮到的問題。

```jsx
if(如果我想的事成真的話){
  做這些事...
} else {
  做其他事...
}
```

這樣的想法也能用 try catch 寫，但盡量不要這樣: 

```jsx
try {
  if(如果我想的事沒有成真) {
		throw '例外';
  }
	做這些事...
} catch(e) {
	做其他事...
}
```

不要這樣處理已知的行為流程，如果在專案裡面狂用 try catch 可能要停下來想想自己是不是還沒有真正搞懂哪些功能的行為是屬於正常情況，哪些又是不想讓它發生的例外。在處理非同步、打 api 有時會用到 try catch 避免網頁 crash，這時就要多花點時間想想這方面的問題。

再舉個實務上的例子，錯誤處理交給前端做，假設今天有個新增 todo list 的 api，如果新增空字串會跳錯，按上面的寫法再做一次:

這種可解決的錯誤，可以用 if 來擋。

```jsx
if(inputValue){
  打 api
} else {
  提醒用戶不能用留空
}
```

```jsx
try {
  打 api
} catch(e) {
	提醒用戶不能用留空
}
```

兩種方法都行。

那再加一個條件呢? 如果新增重複的內容會跳錯，會丟 Error，可以怎麼改?

單純的 if else 好像就做不到了，要怎麼知道目前新增的 todo 有沒有重複?

```jsx
if(inputValue || 沒有重複的 todo){
  打 api
} else {
  提醒用戶不能用留空
}
```

這時就能用 try catch 了。

```jsx
try {
 if(!inputValue) return;
 打 api;
} catch(e) {
 提醒已經有重複的內容了
}
```

### 結論

if else 用來處理正確、可預期的流程， try catch 用來應對例外的問題，至於哪些是正確的流程，這個判斷就只能用經驗去累積了。例外之所以是例外就是因為沒想到會有這情況呀!

對於例外的定義:

- 隕石需求。
- 不該發生的特殊情況。
- 未知的問題。

## 參考資料

- [Handling Errors in JavaScript: The Definitive Guide](https://levelup.gitconnected.com/the-definite-guide-to-handling-errors-gracefully-in-javascript-58424d9c60e6)
- [過度焦慮的 try-catch](https://ithelp.ithome.com.tw/articles/10208079)
- [流程控制與例外處理](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)
- [return](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/return)