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

![](https://static.coderbridge.com/img/techbridge/images/kdchang/cs101/git-workflow.png)

> 資料來源：[Git 與 Github 版本控制基本指令與操作入門教學](https://blog.techbridge.cc/2018/01/17/learning-programming-and-coding-with-python-git-and-github-tutorial/)

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

> 將暫存區恢復到工作區

- git reset : 預設為 HEAD^ --mixed 這個模式會把暫存區的檔案丟掉，但不會動到工作區的檔案
- git reset HEAD^ --hard : 完全當作沒發生過
- git reset HEAD^ --soft : 工作區跟暫存區的檔案都不會被丟掉

#### git commit --amend

> 改寫最後一次 commit 的 訊息

- git commit --amend

#### .gitignore

> 不想被控制的檔案就寫進去

- touch .gitignore
- vim .gitignore
- secret.yml : 忽略 secret.yml 檔案
- /db/\*.sqlite3 : 忽略所有 db 目錄下附檔名是 .sqlite3 的檔案

---

### 2. Git Branch 相關指令

分支（Branch） 只是一個指向某個 Commit 的指標，所謂的分支，其實就只是一個有 40 個字元的檔案而已。

#### git branch

> 開新分支

- git branch week1

#### git branch -v

> 檢視現有分支

- git branch -v

#### git branch -d

> 刪除分支

- git branch -d week1

#### git checkout

> 回到某個指定版本（branch）

- git checkout week1
- get checkout -- . : 全部回覆到上一個 commit 狀態

#### git merge

> 將分支合併回 master

- git merge week1

> HEAD 是一個指向某一個分支的指標，在分支切換 (checkout) 的時候，HEAD 會跟著指向切換過去的那個分支。
> 在 .git 目錄裡有一個名為  HEAD  的檔案，它會記錄目前是在哪個分支的資訊

---

## 四、Git 從本地連上遠端

Github  則是一個支援 Git 程式碼存取和遠端管理的平台服務，有許多的開放原始碼的專案都是使用 Github 進行程式碼的管理。

> Git 可以分為 Local (本地) 和 Remote (遠端) 兩個環境
> 由於 Git 屬於分散式的版本控制系統，所以開發者可以在離線 local 環境下開發，等到有網路時再將自己的程式推到 Remote 環境或 pull 下其他開發者程式碼進行整合。

### 常見問題 1：要用 clone 還是 pull？

如果已經有之前的版本想要更新可以直接 git pull url 網址，而如果是想要把整個資料夾拉下來則使用 git clone url。

### 常見問題 2：git push 後面要加名稱嗎？

如果沒加上名稱，你現在在哪個分支就會 push 哪個分支上去。

> 而如果有加上名稱則會以你要 push 的分支為主

- 舉例來說你現在位於 branch week4
- 但你 git push -u origin master 則會推送分支 master 上遠端。（遠端會自動辨識要同步哪個分支）

### 常見問題 3：為何 pull 和 push 都會加上 origin？

一般情況下遠端只會有一個倉儲（repository），就叫做 origin。

若是 push/ pull 不加 origin 則默認為 origin，若是有多重倉儲想要同步到不同的就要加上倉儲的版本號。ex: version USA

#### git remote

> 與遠端檔案庫操作有關的指令

- git remote add origin url
- git remote remove origin

#### git push

> 把本地端檔案庫推上遠端檔案庫

- git push -u origin week1

#### git pull

> 將遠端分支資料拉回並合併本地分支

- git pull origin master

#### git clone

> 複製其他人 Github 上的專案

- git clone URL

---

## 五、Git 操作實例

> 如何使用 add 、 commit、 push ，以及 pull 去管理我的文字作品？

先在本地開一個 branch 寫作品，然後 merge 到 master，接著推到 GitHub，經過別人在 GitHub 上做些更改再同步回我的本機。

1. 創建資料夾 HW4joke：mkdir HW4joke
2. 跳到資料夾 HW4joke： cd HW4joke
3. 版本化控制資料夾 HW4joke：git init
4. 創建初始文件 joke：touch joke
5. 編輯 joke：vim joke
6. 將 joke 加入暫存區：git add joke
7. 將 joke commit，建立 master 分支：git commit -m "firstVersion"
8. 創建第一個分支：git branch week1hw4
9. 創建文件 joke2：touch joke2
10. 編輯 joke2：vim joke2
11. 將 joke2 加入暫存區：git add joke2
12. 將 joke2 commit：git commit -m "兩個笑話笑呵呵"
13. 切換回 master：git branch master
14. 將 branch week1hw4 合併到 master：git merge week1hw4
15. 在 GitHub 上按右上角 + 新增 Repositories （記得不要勾 read.me
16. 在 GitHub Clone with HTTPS 複製網址
17. 本地加入一個遠端 Repositories：git remote add origin https://github.com/BoisonChang/WEEK1HW4.git
18. 把本地版本推上遠端 Repositories：git push -u origin master
19. 遠端做些小更動： create new file "attention"
20. 遠端同步回本地端：git pull origin master

---

## 六、總結：學好 Git，人生沒煩惱

總之呢，Git 的威力在專案越大，人數越多協作時更能發揮出來，永遠可以回復到上一個版本，也能夠快速找到戰犯是誰（咦），要學好 Git 建議去讀高見龍一書，本文為概述基本會用到之功能。

> 資料來源

- [為你自己學 Git](https://gitbook.tw/)
- [《為你自己學 Git》 閱讀筆記](https://medium.com/@leokao0726/%E7%82%BA%E4%BD%A0%E8%87%AA%E5%B7%B1%E5%AD%B8-git-%E9%96%B1%E8%AE%80%E7%AD%86%E8%A8%98-f76e4026dbce)
- [電子書 kingofamani 的 GIT 教學]()
