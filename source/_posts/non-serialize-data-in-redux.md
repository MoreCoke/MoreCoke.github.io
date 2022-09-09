---
title: non-serializable-data-in-redux
date: 2022-08-30 07:09:44
tags:
---

# non serializable data in redux

在 redux 中處理資料要去思考資料能否序列化(serialization)，這些資料能否在取用時轉換成自己需要的格式，並在使用後回復成原本的資料格式。

在 redux 的官方文件中建議我們設計 store 的變數時盡量用 array 、 object 或是 primitive type value 進行管理。

### 參考資料

- [序列化 維基百科](https://zh.m.wikipedia.org/zh-tw/%E5%BA%8F%E5%88%97%E5%8C%96)
- [Can I put functions, promises, or other non-serializable items in my store state?](https://redux.js.org/faq/organizing-state#can-i-put-functions-promises-or-other-non-serializable-items-in-my-store-state)
- [Is it ok and possible to store a react component inside a reducer?](https://github.com/reduxjs/redux/issues/1248)
- [What Should Go in State?](http://web.archive.org/web/20150419023006/http://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#what-should-go-in-state)
- [What are the actual risks of storing non-serializable data items in Redux store?](https://stackoverflow.com/questions/64205930/what-are-the-actual-risks-of-storing-non-serializable-data-items-in-redux-store)

## non serializable data 和 serializable data 差別

- serializable data

資料從物件轉成純文字後再轉回物件不會遺失內容，ex: `data2 = JSON.parse(JSON.stringify(data))` ，從 data 深拷貝過來的 data2 能保留內容，不過還是有例外的情況，如果物件中有 method 會遺失。

> data can be converted to pure text without losing information.

```js
const obj = {
  a: 1,
  b: () => {
    console.log(2);
    }
};

const obj2 = JSON.parse(JSON.stringify(obj));

// obj2: {
// a: 1
// }

```

- non serializable data

像是 Map/Set, Promise, Class 等等。

#### 參考資料

- [What is the difference between serializable and non-serializable data?](https://hashnode.com/post/what-is-the-difference-between-serializable-and-non-serializable-data-civ6ljzwm0eyc2a53v9le803a)
- [Do Not Put Non-Serializable Values in State or Actions](https://redux.js.org/style-guide/#do-not-put-non-serializable-values-in-state-or-actions)

## 遇到問題的情境

在 接 API 拿資料時使用了 class 物件來整理資料，出現了 non-serializable data 的錯誤，於是將 class 物件改成 function 的方式來處理。

```js

const data = yield call(fetchAPI, arg);

const list = data.map(d => new Model(d));
```

```js
class Model{
  constructor(params) {
    this.a = params.a;
  }

  get aText() {
    return a.toString();
  }
}
```

```js
const _convertA = (num) => num.toString();


function model(params) {
  return {
    a: params.a,
    aText: _convertA(params.a)
  }
}


```

#### 參考資料

- [work with serializable data](https://redux-toolkit.js.org/usage/usage-guide#working-with-non-serializable-data)
