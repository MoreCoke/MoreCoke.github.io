---
title: windows edge swipe 設定
date: 2021-05-23 01:33:06
tags:
---

在 windows 10 的觸控模式下想避免使用者利用邊緣滑動叫出工具列可以怎麼做?

可以透過鎖定 windows 的邊緣滑動達到需求。

## 提醒: 改機碼要萬分小心，要非常清楚自己改什麼，不小心沒弄好是要重灌的!

<!-- more -->

### 懶人包

將以下內容複製到記事本存成 reg 檔，點擊後就能使用。

關閉 edge swipe:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EdgeUI]
"AllowEdgeSwipe"=dword:00000000

```

復原 edge swipe:

```
Windows Registry Editor Version 5.00

[-HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EdgeUI]
```

### 怎麼做?

方法1:

1. 到 windows 工具列搜尋 regedit 點開登錄編輯程式。
![](https://static.coderbridge.com/img/MoreCoke/0a49bd25715d453a980c96791a5b928d.png)
2. 進到這路徑下 HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows 建立 EdgeUI 的機碼。
![](https://static.coderbridge.com/img/MoreCoke/91b6e08a6e6c4ddb8d9a3c1d8b5f340b.png)
3. 在 EdgeUI 下 新增 AllowEdgeSwipe 的 DWORD(32-bit) Value 檔案。
![](https://static.coderbridge.com/img/MoreCoke/f8b92e3fc60445f4aa0551cfee3fcd00.png)
4. 確認 AllowEdgeSwipe 是 0 就大功告成了，0 就是 false，這樣就是關閉 edge swipe 了。
![](https://static.coderbridge.com/img/MoreCoke/60c8ed2eab2a4d29bc3cee89648efe20.png)
5. 這時邊緣滑動已經鎖起來了，如果發現還能透過邊緣滑動叫出工具列，那就重開機試試。

### 想回復邊緣滑動怎麼辦?

把 EdgeUI 整個資料夾或是 AllowEdgeSwipe 這檔案刪掉就好。

### 每次都要這樣點資料夾很麻煩，有偷懶的方式嗎?

可以建立 reg 檔。右鍵點 EdgeUI 按匯出。
![](https://static.coderbridge.com/img/MoreCoke/221cb75d32fb4da4839c54c0287c3d80.png)![](https://static.coderbridge.com/img/MoreCoke/d2b4542a696142efb7fabe994e8980ad.png)

這樣就能看到上面懶人包的程式碼了。

關閉 edge swipe:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EdgeUI]
"AllowEdgeSwipe"=dword:00000000

```

對於回復滑動的做法我是直接將 EdgeUI 這機碼整個刪除，所以會看到 `HKEY_LOCAL_MACHINE` 前多了個減號。

復原 edge swipe:

```
Windows Registry Editor Version 5.00

[-HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\EdgeUI]
```

方法2:
適用版本: windows 10 pro。

1. `win + r` 輸入 gpedit.msc 開啟本機群組原則編輯器。
![](https://static.coderbridge.com/img/MoreCoke/3edb25145c4d4a499c5cf8e305d13201.png)
![](https://static.coderbridge.com/img/MoreCoke/1d02ec226ef34c29b2dc12d9f6b9bf2a.png)
2. 在電腦設定\系統管理範本\Windows 元件\邊緣 UI 點選允許邊緣撥動
![](https://static.coderbridge.com/img/MoreCoke/f65a5638234a4dd8a50703860b35b22b.png)
![](https://static.coderbridge.com/img/MoreCoke/1db4ef07c46943cabe45bbb1845ede44.png)
3. 設定停用
![](https://static.coderbridge.com/img/MoreCoke/ea884e0b8dd0418e90560e8a802cf683.png)

### 參考資料

* [如何在 Windows 10 中開啟登錄編輯程式](https://support.microsoft.com/zh-tw/windows/%E5%A6%82%E4%BD%95%E5%9C%A8-windows-10-%E4%B8%AD%E9%96%8B%E5%95%9F%E7%99%BB%E9%8C%84%E7%B7%A8%E8%BC%AF%E7%A8%8B%E5%BC%8F-deab38e6-91d6-e0aa-4b7c-8878d9e07b11)
* [How to Disable Edge Swipe Gesture on Touch Screen in Windows 10](https://www.top-password.com/blog/disable-edge-swipe-gesture-in-windows-10/)
* [Electron JS kiosk mode with touch screen](https://stackoverflow.com/a/52321420)
* [怎樣關閉 Windows 10 的邊緣滑動手勢](https://answers.microsoft.com/zh-hant/windows/forum/windows_10-other_settings-winpc/%E6%80%8E%E6%A8%A3%E9%97%9C%E9%96%89-windows-10/48e7cf2b-f62d-4a1c-8df4-c0716ab086e0)
* [如何使用登錄檔(.reg)進行新增、修改或刪除登錄機碼和值](https://blog.miniasp.com/post/2008/11/30/How-to-add-modify-delete-registry-subkeys-and-values-by-using-a-registration-entries-reg-file)

### 補充資料

* [登錄編輯程式](https://blog.xuite.net/keryosm/Ts/181938519)
* [What is the difference between regedit and gpedit.msc?](https://superuser.com/questions/961413/what-is-the-difference-between-regedit-and-gpedit-msc)
