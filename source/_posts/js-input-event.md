---
title: JS input 事件介紹
date: 2021-03-28 01:33:06
tags:
---

光是一個 input 就有好多種類型，這篇主要是討論 `input type='text'` 的情況。

<br/>
事件觸發順序為 keydown => keypress(過時) => beforeinput => input => keyup => change
<br/>

<!-- more -->

### 事件觸發條件

* keydown: 按下鍵盤時觸發。
* keypress: 按著鍵盤時觸發(過時事件，官方不建議使用)。
* beforeinput: 在 input 事件開始前觸發。
* input: 只要 input 內的值改變就會觸發。
* keyup: 手指離開鍵盤時觸發。
* change: 在 input 輸入完字串後，離開(blur) input 或是在 input 內(focus)按下 enter 時觸發。

除了 change 事件外，其他事件只要敲鍵盤就會觸發，敲幾次就觸發幾次。
input 會有兩種狀態，分別是 focus 和 blur，focus 指目前聚焦在 input 上，會看到一閃一閃的直線(caret)，想成是可以打字的狀態； blur 是離開 input ，此時不會看到一閃一閃的直線。

### 其中最常用的事件有三個

* input: 用這事件可以得知目前 input 內的 value 變化和當前的 value。
* change: 只想知道 input 最後一次編輯時的 value 是什麼就用這事件。

```js
const input = document.querySelector('input');
input.addEventListener('input',showValue);

input.addEventListener('change',showValue);

function showValue(e) {
  console.log(e.target.value);
}
```

* keydown(或 keyup): 通常用這事件是想針對特定按鍵像是空白鍵或是刪除鍵進行額外處理。
這兩個事件可以接收 keyboardEvent 的物件，物件內有很多東西，其中有兩個超常用到，分別是 key 和 code，兩個都是對應手指按到的鍵，只是回傳的東西有點不太一樣。
舉例來說，按 k 時 key 會回傳 `k` 而 code 會回傳 `KeyK`；按 1 時 key 會回傳 `1` 而 code 會回傳 `Digit1`，其他特殊鍵的話 key 和 code 回傳的幾乎都一樣像是刪除鍵都是回傳 `Backspace`，不過空白鍵的話 key 會回傳 `` 一個空字元，而 code 會回傳 `Space`。

大概知道這幾個就好，其他的可以 [查表](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/code)。

```js
const input = document.querySelector('input');

input.addEventListener('keydown', (e)=>console.log(`${e.key} ${e.code}`));
```

### 參考資料

[The Input (Form Input) element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)
[HTMLElement: change event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event)
[Document: keydown event](https://developer.mozilla.org/en-US/docs/Web/API/Document/keydown_event)
[KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
