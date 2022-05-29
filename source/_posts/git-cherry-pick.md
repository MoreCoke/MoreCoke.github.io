---
title: Git cherry pick 實戰
date: 2021-08-13 01:33:06
tags:
---

### 前言

我在提交 wee7 作業時，發現自己過去不小心在分支上混到 master 的 commit，原本想說要按照 MTR04 的教學影片暴力解移檔案，但後來發現這樣做會洗掉自己過去的 commit，因為自己的 commit 有些訂正紀錄想留下來日後忘了能回去看，所以就去找 git 有沒有什麼方法能解，就找到 cherry pick 了。

<!-- more -->

[pr 混到 master commit 的網址](https://github.com/Lidemy/mentor-program-4th-MoreCoke/pull/9)

![](https://static.coderbridge.com/img/MoreCoke/e6ffe19f5407483cbae598c0e98b1d61.png)

### 首先

我先切回 master 分支，更新到最新進度。

```
git pull https://github.com/Lidemy/mentor-program-4th.git master
git push origin master
```

刪除剛推的遠端分支。

```
git push origin :week7
```

### 接著

開個新分支叫 week7-v2。

```
git checkout -b week7-v2
```

用 sha 值挑出你要的櫻桃(commit)。

```
git cherry-pick 96742ce b30854f 226d433 4367007 d36c054 b32de27 134e3a0 f090cc5
```

關於 sha 值的查找可以用 `git reflog` 去查。

### 最後

把 week7-v2 推上遠端!

```
git push origin week-v2
```

### 稍等片刻

![](https://static.coderbridge.com/img/MoreCoke/486094d6646947a6b04afd440d173f7f.jpg)

### 成功啦~

建立 week7-v2 的 PR，再次檢查就會只剩下自己的 commit 喽!

[成功網址](https://github.com/Lidemy/mentor-program-4th-MoreCoke/pull/10)

![](https://static.coderbridge.com/img/MoreCoke/cf25985ff03b4a1f9efee99fc1bf17a3.png)

最後確認無誤後就能把本地的 week7 分支刪掉了，因為該分支的 commit 還沒 push 到遠端，所以要強制刪除。

```
git branch -D week7
```

### 參考資料

[【狀況題】如果你只想要某個分支的某幾個 Commit？](https://gitbook.tw/chapters/faq/cherry-pick.html)
[Git: Cherry-pick - 選擇某個分支的某些提交記錄](https://cythilya.github.io/2018/05/30/git-cherry-pick/)