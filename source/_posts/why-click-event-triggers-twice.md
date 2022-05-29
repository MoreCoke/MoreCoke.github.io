---
title: 為什麼寫在 label 上的 click 事件會觸發兩次?
date: 2021-04-17 01:33:06
tags:
---

這是我在寫 react 時遇到的問題，不過這問題跟 react 無關，簡化後的程式碼長這樣。

```js
<label onClick={() => console.log('label trigger')}>
    render from react <input type="checkbox" />
</label>
```

<!-- more -->

觸發點擊事件後 console.log 印了兩次，當時一直搞不懂為什麼，就先把 onClick 事件移到 input 去，解決了重複觸發事件的問題。

```js
<label>
    render from react <input type="checkbox"  onClick={() => console.log('label trigger')} />
</label>
```

或是在 label 上把 onClick 事件加上 stopPropagation 也行。

```js
<label onClick={(e) =>{
  e.stopPropagation();
  console.log('label trigger')
}}>
    render from react <input type="checkbox" />
</label>
```

### 為什麼會這樣?

真的想破頭搞不懂，去 google 後看到 stackoverflow 也有人問了一樣的問題 [Why the onclick element will trigger twice for label element
](https://stackoverflow.com/questions/24501497/why-the-onclick-element-will-trigger-twice-for-label-element)。
解答是說在觸發 label 上的點擊事件時也會將這 click 事件傳到跟這 label 有關的 input 去，然後 input 出現冒泡事件才會讓寫在 label 上的 click 事件被觸發兩次。

當時還是不太懂意思又跑去看 label 的 [mdn 文件](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/label)，在 **其他使用事項** 有看到解釋，在 label 上的事件也會連帶觸發相關標籤(input) 的事件。

### 複習事件捕獲和冒泡

當時看完 stackoverflow 後大概知道跟冒泡事件有關，但我那時還是不知道為什麼 label 的點擊事件會往下傳給 input，難道捕獲事件不是停在 label 而是繼續往下傳嗎?

當時我對事件捕獲和冒泡的想法:

```js
<label onClick={()=>console.log('label click')}>
 render from react <input type="checkbox">
</label>
```

捕獲順序: document -> label
冒泡順序: label -> document

前面有說到因為 label 的特性也會連帶觸發 input 的事件，所以我們能拆成兩件事來看。可以想成我們點 label 的同時也在點 input。

先從 label 來看，捕獲冒泡順序:
捕獲順序: document-> label
冒泡順序: label -> document
這邊我們沒有理解錯誤。

再來是 input 的捕獲冒泡順序:
捕獲順序: document -> label -> input
冒泡順序: input -> label -> document

我們在 label 觸發點擊事件時印出了 label click，與此同時 input 也開始它自己的點擊事件，然後 input 事件開始冒泡到 label 時又會再次觸發到 label 的點擊事件，所以我們會看到 label click 印了兩次。

為了方便理解我們在 input 內多加個點擊事件就能更了解來龍去脈了。

```js
<label onClick={()=>console.log('label click')}>
 render from react <input type="checkbox" onClick={()=>console.log('input click')}>
</label>
```

點擊 label 的字 `render from react` 這樣印出的結果會是這樣:

```js
label click
input click
label click
```

如果是直接點擊 input 會印出這樣的結果:

```js
input click
label click
```

覺得這地雷滿可怕的，沒注意到的話，真的會搞不懂為什麼有時候點擊 label 會觸發兩次，為什麼有時候又會正常只觸發一次，其實差別就是在點字 `render from react` 或是直接點 input。

### 額外補充

事件傳遞的順序是先捕獲再冒泡，那在觸發事件的 label 上是先捕獲還是先冒泡?
這要看監聽事件的先後順序。

```html
<label>
  <input/>
</label>
```

這樣會先印 capture 再來是 bubble。

```js
input.addEventListener('click',()=>console.log('capture'),true);
input.addEventListener('click',()=>console.log('bubble'));
```

這樣會先印 bubble 再來是 capture。

```js
input.addEventListener('click',()=>console.log('bubble'));
input.addEventListener('click',()=>console.log('capture'),true);
```

不過在 react 的話都是先捕獲再冒泡。

```js
<label>
        render from react <input type="checkbox" onClick={() => console.log('label bubble trigger')} onClickCapture={() => console.log('label capture trigger')}/> 
    </label>
```

腦洞大開換 props 順序也是一樣。

```js
<label>
        render from react <input type="checkbox" onClickCapture={() => console.log('label capture trigger')} onClick={() => console.log('label bubble trigger')}/> 
    </label>
```

都是先印 capture 再來 bubble。
