---
title: React Native AppState 狀態介紹
date: 2021-03-09 01:33:06
tags:
---


### 手機 app 的行為

在手機 app 運行時會有兩種狀態，分為前台模式和後台模式，從使用者的角度來說這兩個的差別就是看不看的到。前台模式就是你螢幕顯示 app 的狀態，後台模式就是你沒看到這 app 在執行，可能是你在用其他的 app 或是關掉手機螢幕，如果你沒有強制關閉 app 進到手機的多工模式把 app 往上滑的話，這個 app 其實還是在運作的，所以稱為後台模式，也能把這行為想成是 app 的待機模式。

<!-- more -->

### 怎麼用

在 RN 提供了三個 app state:

* active: 前台模式，目前正在使用這個 app
* background: 後台模式，離開 app 到 home 頁面或是改用其他 app 等等
* inactive: 只有 iOS 才有的多工模式，前台模式和後台模式的過渡頁面，如圖

![](https://static.coderbridge.com/img/MoreCoke/c6816c2c011b425cb567bb61367b038b.png)
圖片取自 [[教學] iPhone X 刪除多工處理後台 APP 技巧](https://mrmad.com.tw/iphone-x-multitasking)
<br/>

我們會掛上監聽事件 `addEventListener`，搭配事件類型 `change` 取得目前最新的狀況。可以透過 `AppState` 的 `currentState` 取得最新的狀態:

class component

```js
import React, {Component} from 'react';
import {AppState} from 'react-native';

class Test extends Component {
  state = {
    appState: AppState.currentState
  };
  
  updateState(newState) {
    this.setState({
      appState: newState;
    },()=> console.log('Current state: ',this.state.appState););
  }
  
  componentDidMount(){
    AppState.addEventListener('change',this.updateState);
  }

  componentWillUnmount() {
    AppState.removeEventListener('change',this.updateState);
  }
}

```

記得要清除監聽事件，不然會有記憶體殘留的問題。

function component

```js
import React, {useState, useEffect} from 'react';
import {AppState} from 'react-native';

function Test() {
  const [appState, setAppState] = useState(AppState.currentState);

  const updateState = (newState) => {
    setAppState(newState);
    console.log('Current state: ', newState);
  };
  useEffect(() => {
    AppState.addEventListener('change',updateState);
    return () => {
      AppState.removeEventListener('change',updateState);
    }
  },[]);
}

```

### 參考資料

[AppState](https://reactnative.dev/docs/appstate)

[Working with App State and Event Listeners in React Native](https://rossbulat.medium.com/working-with-app-state-and-event-listeners-in-react-native-ffa9bba8f6b7)
