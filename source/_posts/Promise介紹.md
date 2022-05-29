---
title: Promise介紹
date: 2021-06-06 01:33:06
tags:
---

### 先說 callback 是什麼

在 js 中，function 的參數也能塞 function，這種被塞入的 function 就叫做回呼函式(callback function)，用來處理我們接下來想做的事，這樣的好處是可以確保執行順序不會被非同步問題給影響。
舉例來說，我睡醒賴床十秒然後花五秒去廁所:

<!-- more -->

```js
function wakeup(cb){
  setTimeout(
    console.log('wake up');
    cb();
  ,1000*10);
}

function goBathroom(){
  setTimeout(
    console.log('arrive');
  ,1000*5);
}
wakeup(goBathroom);
```

利用 callback 我們能確保 `wakeup()` 等十秒後才會呼叫 `goBathroom()` 等五秒後到廁所。

### promise 用來幹嘛?

promise 是 ES6 提供的新功能，可以用來處理非同步的物件，在過去處理非同步任務時，為了要確保這些非同步任務能按順序執行，我們只能將下一個任務(function)放進上一個任務中(function)，只是這樣程式碼可讀性很差，會出現 callback hell，為了解決這問題，promise 提供了簡潔的語法，我們能透過 `then` 鍊接來確保任務的執行順序在我們的預期中。

假設今天有三個非同步任務，在每個非同步所需的時間不定的前提下要怎麼確保任務能按我們所想的順序來執行呢?
我想要按照順序將 1、2、3 依序印出該怎麼做?

這樣子會印出的結果是 -> 3、1、2。

```js
setTimeout(()=>console.log(1),300);
setTimeout(()=>console.log(2),500);
setTimeout(()=>console.log(3),100);
```

過去的作法會將非同步任務一個個包起來:

```js
setTimeout(()=>{
  console.log(1);
  setTimeout(()=>{
    console.log(2);
    setTimeout(()=>{
      console.log(3)
    },100);
  },500);
},300);
```

使用 promise:

```js
function promise1(){
  return new Promise(r=>setTimeout(()=>{console.log(1);r()},300));
}
function promise2(){
  return new Promise(r=>setTimeout(()=>{console.log(2);r()},500));
}

function promise3(){
  return new Promise(r=>setTimeout(()=>{console.log(3);r()},100));
}

promise1().then(promise2).then(promise3).catch(err=>console.log('peko'))

```

你會發現用 promise 的可讀性高很多，程式碼也變得好改許多，如果改成想印出 ->1，3，2 的話只要把 promise2 和 promise3 對調就好，而callback hell 的做法要整個拆掉來改。上面的程式碼中你會看到 `r()` 這個能讓 then 鍊接成功很重要的東西，下文會繼續解釋。

callback hell 改動:

```js
setTimeout(()=>{
  console.log(1);
  setTimeout(()=>{
    console.log(3);
    setTimeout(()=>{
      console.log(2)
    },500);
  },100);
},300);
```

promise 做法:

```js
promise1().then(promise3).then(promise2).catch(err=>console.log('peko'))
```

### promise 的狀態

promise 物件會有三個狀態:

- pending: 等待中
- fulfill: 完成
- reject: 失敗
狀態的轉換會決定 promise 是否要進展到下個鍊接，調用 then 或是 catch 的 callback。
試著把 promise 的狀態表達出來吧~

```js
function promise1(){
  return new Promise(r=>setTimeout(()=>{console.log(1);r()},300));
}
function promise2(){
  return new Promise(r=>setTimeout(()=>{console.log(2);r()},500));
}

function promise3(){
  throw new Error();
}

promise1().then(promise2).then(promise3).catch(err=>console.log('peko'))
```

promise1: pending -> fulfill，程式順利執行，透過 then 開始執行 promise2。
promise2: pending -> fulfill，程式順利執行，透過 then 開始執行 promise3。
promise3: pending -> reject，程式執行出現錯誤，跳到 catch 執行 catch 相關程式碼。

### then catch 串接方式

then 可以接收兩個東西:

- 上個 promise 的 resolve() 括弧中的東西
- 透過 return 回傳的值

catch 用來收:

- promise 的 reject() 括弧中的東西
- 錯誤

在 promise 中，我們會放入一個 callback 並塞入兩個參數，我們習慣稱為 resolve 和 reject，如果 promise 執行成功會呼叫 resolve()，失敗的話會呼叫 reject()，我們能把想往下傳的東西塞入括弧中，在這範例中我想在程式執行成功時印 1，失敗印 2。

```js
const example = new Promise((resolve,reject)=>{
    if(true){
      resolve(1);
    } else {
      reject(2);
    }
});
example.then((resolve)=>console.log(resolve)).catch((reject)=>console.log(reject));
```

then 會去接收上個 promise 中的 resolve 收到的值，所以在這個例子中會印出 1。

```js
const example = new Promise((resolve,reject)=>{
  if(false){
    resolve(1);
  }else{
    reject(2);
  }
});
example.then((resolve)=>console.log(resolve)).catch((reject)=>console.log(reject));
```

同樣的道理，在 false 的情況下會透過 reject 將值傳給 catch ，所以上面的例子會印出 2。

再來說 return 的情況，then 也能接收透過 return 回傳的值，不過透過這樣的方式傳值時要注意同步和非同步的問題。可以看看下方的例子，並試著想這樣會印出什麼？

```js
function promise1(){
  return new Promise(r=>setTimeout(()=>r(1),300));
}
promise1().then((r)=>{
  console.log(r); 
  return 2;
}).then((r)=>{
  console.log(r);
  console.log(3);
}).catch(err=>console.log('peko'))
```

這樣會印出 123。我再做個變化，想想看這樣會印出什麼：

```js
function promise1(){
  return new Promise(r=>setTimeout(()=>r(1),300));
}
promise1().then((r)=>{
  console.log(r);
  setTimeout(()=>{
    return 2;
  },500);
}).then((r)=>{
  console.log(r);
  console.log(3);
}).catch(err=>console.log('peko'))
```

這樣會印出 1 undefined 3，第二個 then 並不會等第一個 then 的 setTimeout 結束，而是直接執行，所以印出 undefined。
解決方法就是在 setTimeout 外包 promise 並 return，這樣下個 then 就能接收到了。

```js
function promise1(){
  return new Promise(r=>setTimeout(()=>r(1),300));
}
promise1().then((r)=>{
  console.log(r);
  return new Promise(r=>{
    setTimeout(()=>{
       r(2);
    },500);
  });
}).then((r)=>{
  console.log(r);
  console.log(3);
}).catch(err=>console.log('peko'))
```

重點整理：
非同步行為要用 promise 包起來並 return，不然 then 是不會等非同步行為，而是會直接接著執行的。
也能看看這篇討論 [Return from a promise then()](https://stackoverflow.com/a/43155226)。

### promise method

目前還沒用過，先記著以後要用再學。

- all
- race
- allSeltted
- any

### 參考資料

- [使用 Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Using_promises)
- [Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [JavaScript基本功修練：Day25 - Promise](https://ithelp.ithome.com.tw/articles/10251786)
- [Excuse me？这个前端面试在搞事！](https://zhuanlan.zhihu.com/p/25407758?columnSlug=liril&fbclid=IwAR1Ix7nxCqeNJbQHYbme9klEjAkcvQ4jkz_SCgLVDYfs_Ln7kJwv3Y96GUo)
- [Understanding Promises in JavaScript](https://betterprogramming.pub/understanding-promises-in-javascript-13d99df067c1)
- [[筆記] 理解 JavaScript 中的事件循環、堆疊、佇列和併發模式（Learn event loop, stack, queue, and concurrency mode of JavaScript in depth）](https://pjchender.blogspot.com/2017/08/javascript-learn-event-loop-stack-queue.html)
- <https://blog.huli.tw/2015/08/26/javascript-promise-generator-async-es6/>
