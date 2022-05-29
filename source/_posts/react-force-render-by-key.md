---
title: React 用 key 強迫 component 重新渲染
date: 2021-04-29 01:33:06
tags:
---


這應該不是好做法，因為在官方文件上並沒有看到相關的用法，不過這用法很方便就是...。
除了 React 外，Vue 也能這樣玩，Angular 沒用過不知道。

<!-- more -->

### key 的用法

我們經常會用陣列的資料搭配 map 來做出多個 component 列表，並在這些 component 都加上 key 用來辨識，日後重新 render 列表時 React 就能透過這些 key 來追蹤哪些 component 沒動過，哪些有被改到，決定是否要保留現有的 DOM 元素要對它更新就好，還是說要砍掉該 DOM 元素再另外建個新的，進而提升渲染的效率。

### 為什麼能提升效率?

因為在重新 render 時 React 會比較更新過的 state 資料，來更新 Virtual DOM tree 最後才去改 DOM tree，會使用遞迴去檢查內部的東西有沒有改變，這時就可以透過 key 進行初步判斷，先快速找到新舊陣列間兩個新舊元素之間的關聯，最後再將差異反映在 DOM 元素上，而不是每次重新渲染直接把 DOM 元素刪掉後再重做個新的。

```js
// 假設改資料前的陣列是 => [{name:'apple',id:'a'},{name:'banana',id:'b'}]
// 透過 setState 改陣列資料後是 => [{name:'apple',id:'a'},{name:'pineapple',id:'p'}]
<ul>
 {
   array.map(e=><li key={e.id}>{e.name}</li>)
 }
</ul>
```

DOM 的前後變化:

```html
// 改資料前的樣子
<ul>
  <li key='a'>apple</li>
  <li key='banana'>banana</li>
</ul>

// 改資料後的樣子
<ul>
  <li key='a'>apple</li>
  <li key='p'>pineapple</li>
</ul>
```

如果沒有 key，React 看到上面這情況會使用遞迴去看 li 中的內容(children, 也就是 apple 和 banana)所對應到陣列中的哪個元素，有了 key 的幫忙可以更快找到新舊 DOM 元素之間的關係，可以更快知道 `key='a'` 這值沒變，直接把舊的 Virtual DOM tree 上的 `<li key='a'>apple</li>` 移到新的 Virtual DOM tree 就好； `key='b'` 不見了，舊的 Virtual DOM tree 上的 `<li key='b'>banana</li>` 就不會移到新的 Virtual DOM tree；多了一個新的 `key='p'`，在新的 Virtual DOM tree 上建一個新的 `<li key='p'>pineapple</li>`。

### key 的更動代表什麼?

相信大家都知道，如果map 出來的列表是會變動的，key 不要綁索引值，這樣 key 無法讓新舊的 Virtual DOM 元素產生對應， key 的變動都會讓 DOM 元素新增或刪除。

* 多了新的 key: 建立一個新的 DOM 元素。
* 少了一個 key: 把跟該 key 相關的 DOM 元素刪掉。

### 用 key 強迫更新

其實任何 DOM 元素都能加 key，利用更新 key 的方式就能重新 render 該 DOM 元素。當我們更動 key 就相當於:

* 舊的 key 不見了 => React 把這個 DOM 元素刪掉(unmount)。
* 發現新的 key => React 替這個 key 新增一個 DOM 元素(mount)。

### 範例

我今天有個 video，我想在這個 video播完影片後就去播下個影片，我可以怎麼做?

我在影片結束時呼叫 onEnded 用來播下個影片，在 source 插入動態連結，這樣有個問題，第一個影片播完後就不會播了，雖然 source 有變，但 react 繼續復用 video 這個 DOM 導致影片連結更新但沒有反應。

```js
const videoNames = [
  "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm",
  "https://www.w3schools.com/tags/movie.ogg"
];

export default function StandbyVideo() {
  const [videoIdx, setVideoIdx] = useState(0);
  const playNextVideo = () => {
    const nextVideoIdx = (videoIdx + 1) % videoNames.length;
    setVideoIdx(nextVideoIdx);
  };
  return (
    <div>
      <video
        className={css(styles.videoContainer)}
        autoPlay={true}
        onEnded={playNextVideo}
        controls
      >
        <source src={videoNames[videoIdx]} />
      </video>
    </div>
  );
}
```

加入 key 後就能解決問題了，這樣的意思是每次我的 videoIdx 變了，React 就會把舊的 video 給刪掉(unmount)，再重生一個 video (mount)。

```js
<video
  key={videoIdx}
  className={css(styles.videoContainer)}
  autoPlay={true}
  onEnded={playNextVideo}
  controls
>
  <source src={videoNames[videoIdx]} />
</video>
```

---
題外話，不用 source 直接將 src 寫在 video 上也能解決需求，因為我想展示 key 重新 render 的效果，所以才這樣寫。

```js
<video
  className={css(styles.videoContainer)}
  autoPlay={true}
  onEnded={playNextVideo}
  src={videoNames[videoIdx]}
  controls
>
</video>
```

附上 [codesnadbox](https://codesandbox.io/s/vigorous-ives-t962q?file=/src/StandbyVideo.js:520-784)

### 參考資料

[Virtual Dom && Diff原理，极简版](https://zhuanlan.zhihu.com/p/57974487)
[[Day 04] 理解React Virtual DOM](https://ithelp.ithome.com.tw/articles/10234155)
[Reconciliation](https://zh-hant.reactjs.org/docs/reconciliation.html)
[Virtual DOM and Internals](https://reactjs.org/docs/faq-internals.html#what-is-react-fiber)
