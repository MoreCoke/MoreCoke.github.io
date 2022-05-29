---
title: rupa/z, autojump 設定
date: 2021-05-29 01:33:06
tags:
---

這兩個套件可以幫我們記憶透過 `cd` 切換的目錄資料夾，讓我們可以不用打一長串路徑，無形中省下不少時間。

### rupa/z

ex:

```
cd documents/test/subtest
z subtest
```

<!-- more -->

看你想把 z 的檔案放哪，我是習慣放 $HOME 目錄下。
首先打開 iTerms，接著到官網 [rupa/z](https://github.com/rupa/z) 下載專案。
`git clone https://github.com/rupa/z.git`

![](https://static.coderbridge.com/img/MoreCoke/f2aedfebeb524bb98668b4bd47fbbbd9.png)

打開你的 .zshrc，這個檔案是用來設定所有 zsh 相關的文字檔。
`open .zshrc`
![](https://static.coderbridge.com/img/MoreCoke/0c88083436f545bf8b6b9ffd798a3b11.png)

將 `source ~/z/z.sh` 貼上去，`source` 等於 `.` ，用來執行 shell command。
![](https://static.coderbridge.com/img/MoreCoke/87e39f696ee94532b8044fdb73f1da82.png)

接著回到 iTerms 執行 `source .zshrc` 或是 `. .zshrc`，如果用 `.` 執行你會發現 command line 找不到相關的目錄。
![](https://static.coderbridge.com/img/MoreCoke/e43e4054472c4f1fb66ef3fd303a53cb.png)
用 `.` 路徑要寫清楚點算是個坑要注意些，`. ~/z/z.sh` 或 `. ./z/z.sh`。
![](https://static.coderbridge.com/img/MoreCoke/cce05bf37009464bb2c4ba9a40be7491.png)

這樣用 cd 切換目錄幾次後就能抓到該目錄了，直接輸入 `z 目錄名` 就好了。

### autojump

我個人比較喜歡這套件，一樣先到 [github](https://github.com/wting/autojump) 將專案 clone 下來，我的話一樣放在 $HOME 目錄下。
`git clone https://github.com/wting/autojump.git`
![](https://static.coderbridge.com/img/MoreCoke/7cc11c288b3640a7afe7358e39baf4aa.png)

接著照 `READ.md` 的指令做，移動到 autojump 資料夾執行 `.install.py`。

打開 `.zshrc` 設定路徑，你可以在 autojump 的 bin 資料夾看到 `autojump.zsh` 這檔案，把這路徑寫進去，`source ~/autojump/bin/autojump.zsh`。
![](https://static.coderbridge.com/img/MoreCoke/7acc1b4a4c0e4973ba2cea712a56fcec.png)

回到 iTerms 執行 `source .zshrc`，一樣 cd 切換目錄幾次後就能透過 `j` 這指令來切換目錄了。
ex:

```
cd documents/test/subtest
j subtest
/Users/xxx/documents/test/subtest
```

![](https://static.coderbridge.com/img/MoreCoke/cf0092ef8d124566a826f7db0b659117.png)

跟 z 不同的是 autojump 會有紅字提醒說這完整路徑是什麼，不確定 z 有沒有提供，不過我懶得研究了。我用 z 有時候會有抓不到相關路徑的問題讓我挺困擾的，目前也找不到原因。
