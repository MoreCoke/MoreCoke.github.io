---
title: git author 設定
date: 2020-08-19 01:33:06
tags:
---


### 在用不同電腦操作時常遇到的問題就是 git 送出 commit 時的使用者問題

git config user.name 名字
git config user.email 郵件
以上設定都只是區域設定，這些設定只會保留在當前專案部會改到全域的設定。

<!-- more -->

檢查全域設定

git config --global user.name
git config --global user.email

在 commit 時設定使用者

git commit --author="MoreCoke <my email>" -m "my commit"

在這專案設定一次就好，之後的 commit 會以設定的 author 為主。
