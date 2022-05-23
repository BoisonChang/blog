---
title: "百日轉職前端工程師：第一週人生不能重來但 Git 可以《DAY 6》"
slug: Transfer-FrontEnd-100-6
date: 2020-06-16T14:04:21+08:00
draft: false
tags: ["FrontEnd", "transfer", "Git"]
author: "Boison"
showToc: false
TocOpen: false
hidemeta: false
comments: true
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
---

大家好，這是百日轉職前端工程師的 Day6，也是 6/16（二）。標題那句話是出自五倍紅寶石的高見龍，我覺得很好的表現了 Git 的核心精神，今天要來聊聊 Git 這個超好用的版本控制工具，簡單來說你在玩 RPG 的時候不是會各種存檔嗎？

要比喻的話 Git 大概就是類似的功能，只要你學會 Git ，從此你的專案就能夠開啟無限的平行世界線了（咦

## ㄧ、Git 是什麼？

簡單來說我的理解， Git 就是可以控制專案的版本的存檔工具，而且可以在多人協作下控制專案的版本，有點像是多人一起破一個開放世界的 RPG 遊戲，大家分工合作去解不同的支線，過程中可以各自存檔互不干擾/或者接力破關，等到確定解完支線再開啟合併（merge）功能，合併破的支線任務和拿到的道具到主線劇情中。

> Git 是一種「分散式版本控制系統」，可記錄每一個版本的所有變化。

---

## 二、Git 的架構

> **Git 採兩段式更新**

1. 使用 git add 陸續將檔案更新狀態由「工作區」移入「暫存區」
2. 檔案由 untracked 加入版本控制 staged
3. 告一段落後再 git commit 將其由「暫存區」提交到「儲存庫」

> **Git 的提交流程就像是一個生產流水線**

1. 你先生產一批產品（修改過的檔案）
2. 把生產好的檔案放到一個存放的地方(git add 到「暫存區」)
3. 當你覺得這批貨不管是質量還是數量都沒問題了，就把這批貨打包裝箱(git commit)

> **Git 的三個區域**

1. 工作區 (Working Directory)
   - 就是你在本端程式儲存的資料夾
2. 暫存區 (Staging Area)
   - 儲存庫就是每次提交 commit 版本前放置的區域
   - 檔案都要透過 git add 的方式新增到索引區 Git 才能追蹤這個檔案
3. 儲存庫 (Repository)
   - 工作區有一個隱藏目錄 .git，這個不算工作區，而是 Git 的版本庫
   - 也就是每次 commit 版本放置的區域
   - 當程式在工作目錄 (Working Directory) 修改到一個階段，也把更改的檔案放到暫存區(Stage)
   - 此時便可以將資料提交 commit 到儲存庫建立新的版本

！[](https://dotblogsfile.blob.core.windows.net/user/jackeir/1506/201563016550438.png)

> 資料來源：[Git 筆記 Ch2 上](https://dotblogs.com.tw/jackeir/2015/06/30/151690)

---

### Git 版本控制的進階小常識 by 高見龍

> **在 Git 的世界裡 Blob、Tree、Commit 以及 Tag 這四種物件一定要認識一下**

- Blob 物件主要是存放檔案內容
- Tree 物件主要是存放目錄以及檔案資訊
- Commit 物件，看名字就知道是存放 Commit 的資訊
- Tag 物件，也就是存放跟 Tag 相關資訊的物件（更正確的說，其實是 Annotated Tag 而不是一般的 Tag）

> **這四種物件應用的情境**

1. 當開始把檔案加到 Git 的暫存區(或稱之索引區)，Git 便會開始生出一些 Blob 物件
2. 當開始進行 Commit，Git 便會產生出一些 Tree 物件，指向剛剛那些 Blob 物件
   - 或是可能會有其它的子目錄，所以 Tree 物件也可能會指向其它的 Tree 物件
3. Commit 的同時，也會產生出 Commit 物件，它會指向某一個 Tree 物件
   - 而除了最一開始的 Commit 之外，所有的 Commit 物件也都會指向它的前一個 Commit 物件
4. 最後 Tag 物件則會指向某一個 Commit 物件

---

## 三、Git 怎麼用？

同樣是可以直接透過終端機來操控，所以必須先熟悉命令列介面，可以見上篇文章介紹用 CLI (Command-line interface) 命令電腦。

### 1. Git 基本指令

#### git init

> git 專案初始化（生成隱藏檔案 .git）

#### git status

> 檢視目前狀態

#### git add

> 決定檔案是否加入「版本控制 staged」

- git add test.js

#### git -rm cached

> 將加入版本控制的檔案移除

- git rm test.js
- git rm --cached test.js : 讓檔案不再受 git 版本控制

#### git commit

> 新建一個版本

- git commit test.js

#### git commit -am

> 合併 add 跟 commit 兩個指令

- git commit -am test.js

#### git log

> 歷史紀錄

- git log
- git log -p : 多列出版本之間差異
- git log test.js : 檢視 test.js 的紀錄)

#### git mv

> 請 git 幫你變更檔名 (直接進暫存區)

- git mv test.js test2.js

#### git diff

> 比對工作區與暫存區全部檔案的差異

- git diff

#### git reset

>
