---
title: Git 常忘指令
date: 2021-08-14 01:33:06
tags:
---

### git rebase <bugfix> <master>

不管目前位在哪個分支，把 bugfix 分支移到 master 上，也能用 `git checkout master` `gitrebase bugfix` 解決。

**注意** : 別在 master 上用，這樣別人的 git history 會被你影響。

<!-- more -->

### 合併錯了要怎麼取消?

兩種方法

1. 使用 git reflog 調紀錄，再用 git reset。
2. 使用 git reset ORIG_HEAD --hard ，ORIG_HEAD 這參數會記錄這類的重大事件。

### git diff --cached HEAD

比對現在修改了什麼，現在 commit 的狀態和當前 HEAD 內容進行比對，指令也能省略呈 git diff --cached 。

### git commit --amend

主要用來修改最新 commit 的內容，有時 -m 的文字內容會打錯，可以用這修改。

### 怎麼捨棄目前做的修改?

用 git checkout --檔名，把這個檔案目前修改的內容全都捨棄不要了。

### 如何更新 master 分支進度?

在使用 git rebase 時會發生接上去的分支進度比 master 前面，可以在 master 上用 git merge 分支來觸發 fast forward。

### git tag 名字 commit節點

標上去的 tag 可以用來記錄 commit 的重大事件，假如我想在現在的進度貼個標籤可以這樣做， git tag new_version master，這樣我現在的 commit 節點就會有 new_version 的節點。

### git describe commit節點

會找出距離該節點最近的 tag，假如有個 finish 的 tag，這時在 master 上用 git describe master，會得到類似這樣的回覆 finish_3_2jdfe3 ，意思是距離現在 master HEAD 的節點再經過 3 個 commit 會有個 finish 的 tag ，然後它的 SHA 值會是 2jdfe3。

### git 遠端流程

git fetch + git merge = git pull

git fetch 是用來更新本地儲存庫，在本地端儲存庫遠端的分支會用 o/master 來表示， git fetch 就是用來更新這些遠端進度的。

有時想將本地資料推送到遠端時會失敗是因為 o/master 進度不同，這時要先將本地 commit 更新至相同進度。記得要在本地的 master 分支用 pull 才能拉到遠端的 master 分支。想讓 commit 的歷史更好看可以加入 --rebase 參數。

* git push origin 來源(本地):目的地(遠端)。
* git pull origin 來源(遠端):目的地(本地端)。
* git fetch origin 來源(遠端):目的地(本地端)。

### 刪除遠端分支

有兩個方法

1. git push origin :分支名，來源不放東西即可。
2. git push origin --delete 分支名 。

### 把遠端分支抓到本地端

用 git checkout 遠端分支名 就可以了。

### 怎麼改 branch 名稱

名字不小心打錯藥怎麼改? git checkout 到分支後用 git branch -m 新名字 ，這樣就成功喽。

### ~ 和 ^ 的差別

都是相對位置的設定，差在哪呢?
~ 是往上爬找父 commit。
^ 只有一個的話是往上找一個父 commit，如果是像 ^^ 這樣連續使用就是要跨分支，這部分挺抽象的，可以看 [What's the difference between HEAD^ and HEAD~ in Git?](https://stackoverflow.com/questions/2221658/whats-the-difference-between-head-and-head-in-git)。

### 如何跟 Lidemy 的儲存庫同步，更新自己遠端的儲存庫?

方法 1
更改自己 remote 的 url，用 `git remote set-url origin 網址` 更改，可以使用 `git remote -v` 確認目前指向的網址，接著 `git pull origin master`。改完後再 `git remote set-url origin 網址` 改回自己的網址。

方法 2
`git pull Lidemy網址 master`，接著會跳到 Vim 介面再 `:wq` 就能離開。

最後不管用哪個方法都要記得 `git push origin master` 推回自己的 master 分支上。

### 暫存目前進度

常會有事情做到一半被 PM 通知有 bug 要修，這時可以用 `git stash` 把現在的進度暫存，之後回來再用 `git stash pop` 就能繼續處理原本在做的事。

### git revert 反轉 commit

跟 reset 有點像，都是用來取消 commit 用的，差別在 reset 會 **直接清除** 要取消的 commit ，而 revert 會 **保留** 要取消的 commit，並再推出一個新的 commit，這個 commit 用來清除要取消的 commit。
revert 在多人開發和需求變動頻繁的情況下滿常用的。

### 用 git bisect 抓 bug

遇到一個 bug 但是不知道是哪個 commit 改壞，除了一個個看 commit message 外，還有其他方法嗎?
可以透過 `git bisect start <bad> <good>` 先設定範圍， bad 指的是出現 bug 的 commit，good 是沒有 bug 的 commit，加入這兩個 commit 的 SHA 值後，git 會利用二分搜尋法透過指令 `git checkout` 到 bad 和 good 中間的 commit，如果這 commit 沒有 bug 就輸入 `git bisect good`，反之 `git bisect bad`。最後就能得知 bug 是在哪支 commit 中出現的。如果已經有頭緒知道是哪支 commit 改壞，想提前結束搜尋可以用 `git bisect reset`，git 會恢復成使用 git bisect 前的狀態。
文件: [git bisect 指令](https://git-scm.com/docs/git-bisect)
