---
title: React Navigation 設計根目錄
date: 2021-04-05 01:33:06
tags:
---

使用版本 5.x

### 要在根目錄上做什麼?

通常會在 src 的資料夾建 index.js 用來當 react navigation 的根目錄，使用 NavigationContainer 這個組件會把 Navigator 包起來，NavigationContainer 只會在根目錄上用到。

<!-- more -->

```js
const AppStack = createStackNavigator();
function App() {
  return (
    <NavigationContainer>
      <AppStack.Navigator>
        xxxxx
      </AppStack.Navigator>
    </NavigationContainer>
  );
}

```

### 怎麼分配頁面?

需要使用 createNavigator 這個 function 來建立 navigator，它會回傳一個物件裡面有 Screen 和 Navigator 這兩個屬性，Screen 用來處理跟頁面有關的組件，然後這些 Screen 必須包在 Navigator 裡面才能使用 react navigation 提供的功能。

```js
const AppStack = createStackNavigator();

function HomeScreen() {
  return (
    <AppStack.Navigator>
      <AppStack.Screen name={'A'} component={AScreen}/>
      <AppStack.Screen name={'B'} component={BScreen}/>
    </AppStack.Navigator>
  );
}
```

### 注意事項

Navigator 數量沒有使用的上限，能在現有的 Navigator 裡再使用多個 Navigator，使用起來非常方便，所以要特別小心在設計頁面上盡量避免包太多層的 Navigator ，因為每個 Navigator 都有自己的狀態，能保存導覽的歷史紀錄、設定進場動畫等等，太多巢狀的 Navigator 到最後會弄得腦袋很亂，會很難管理。

```js
const AppStack = createStackNavigator();
function A1Screen() {
  return (
    <AppStack.Navigator>
      <AppStack.Screen name={'Aa'} component={AaScreen}/>
      <AppStack.Screen name={'Ab'} component={AbScreen}/>
    </AppStack.Navigator>
  );
}

function AScreen(){
  return (
    <AppStack.Navigator>
      <AppStack.Screen name={'A1'} component={A1Screen}/>
      <AppStack.Screen name={'A2'} component={A2Screen}/>
    </AppStack.Navigator>
  );
}

function HomeScreen(){
  return (
    <AppStack.Navigator>
      <AppStack.Screen name={'A'} component={AScreen}/>
      <AppStack.Screen name={'B'} component={BScreen}/>
    </AppStack.Navigator>
  );
}
```

根據上面程式碼的結構會長這樣:

```js
-Home
  -AScreen
    -A1Screen
      -AaScreen
      -AbScreen
    -A2Screen
  -BScreen

```

其實可以簡化成這樣:

```js
const AppStack = createStackNavigator();
function HomeScreen(){
  return (
      <AppStack.Navigator>
        <AppStack.Screen name={'A'} component={AScreen}/>
        <AppStack.Screen name={'A1'} component={A1Screen}/>
        <AppStack.Screen name={'Aa'} component={AaScreen}/>
        <AppStack.Screen name={'Ab'} component={AbScreen}/>
        <AppStack.Screen name={'A2'} component={A2Screen}/>
        <AppStack.Screen name={'B'} component={BScreen}/>
      </AppStack.Navigator>
  );
}

```

整理後的結構長這樣:

```
-Home
  -AScreen
  -A1Screen
  -AaScreen
  -AbScreen
  -A2Screen
  -BScreen
```

巢狀頁面我通常都會用來處理 Tab 或 Drawer 頁面。

```js
const AppStack = createStackNavigator();
const AppTab = createBottomTabNavigator();

function AScreen() {
  return (
    <AppTab.Navigator>
      <AppTab.Screen name={'A1'}/>
      <AppTab.Screen name={'A2'}/>
    </AppTab.Navigator>
  );
}

function HomeScreen() {
  return (
      <AppStack.Navigator>
        <AppStack.Screen name={'A'} component={AScreen}/>
        <AppStack.Screen name={'B'} component={BScreen}/>
      </AppStack.Navigator>
  );
}
```
