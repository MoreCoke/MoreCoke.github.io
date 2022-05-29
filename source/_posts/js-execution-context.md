---
title: JS Execution Context
date: 2022-05-29 01:33:06
tags:
---

## 簡介

JS 的執行環境(Execution Context)
分成兩種:

1. Global Execution Context，在讀取 script 標籤時建立，只會有一個
2. Function Execution Context，在建立 function 時建立，會有多個

在執行環境中會有兩個階段:

1. Creation Pharse，建立 variable object，建立範疇鍊，建立 this，將宣告的變數和函式提升
2. Execution Pharse，逐行執行，定義 this，取決於怎麼呼叫的，變數賦值

```js
var a = 1;
let b = 2;
window.a // 1
window.b // undefined
```

用 var 宣告的變數如果是出現在函式命名外會被加進 global object，let 不會。

## 參考資料

[[JavaScript] Javascript 的執行環境 (Execution context) 與堆疊 (Stack)](https://medium.com/itsems-frontend/javascript-execution-context-and-call-stack-e36e7f77152e)
[[JavaScript] Javascript 的作用域 (Scope) 與範圍鏈 (Scope Chain)：往外找](https://medium.com/itsems-frontend/javascript-scope-and-scope-chain-ca17a1068c96)
[[week 16] JavaScript 進階 - 什麼是閉包？探討 Closure & Scope Chain](https://hackmd.io/@s_jpXuNwRQiUuGCOQAOZuA/S175l74wY)
[JavaScript Execution Context – How JS Works Behind The Scenes](https://www.freecodecamp.org/news/execution-context-how-javascript-works-behind-the-scenes/)
[[JavaScript] 執行環境（Execution Context）與堆疊（Stack）](https://hackmd.io/@s_jpXuNwRQiUuGCOQAOZuA/S175l74wY)