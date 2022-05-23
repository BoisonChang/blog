---
title: "百日轉職前端工程師：第一週用 CLI 命令電腦《DAY 5》"
slug:  Transfer-FrontEnd-100-5
date: 2020-06-15T20:18:21+08:00
draft: false
tags: ["FrontEnd", "transfer", "CLI"]
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

大家好，這是百日轉職前端工程師的 Day5，也是 6/15（一），今天來聊聊命令列介面 CLI (Command-line interface) 。

CLI 是用來命令你的電腦執行動作的語言，早期還沒有圖形化介面時你想要使用電腦，就必須要用 CLI，一直到今天它在軟體開發上還是佔據非常重要的位置。

---

## 一、什麼是 CLI (Command-line interface)？

現在很多人可能不知道 CLI 是什麼，因為我們現在的電腦像是桌面之類，都是後來發展出來比較好操作的圖像化介面 GUI（Graphical User Interface）。

比如你現在的桌面、資料管理員，或者垃圾桶都是 GUI 一環。

但是有些在軟體開發上的指令你還是要用 CLI 才有辦法執行，滿推薦大家都能了解一下的。

總之我理解 CLI 是用來命令你的電腦執行動作的介面，本質上背後也是由程式碼撰寫而成，又叫做文字使用者介面（character user interface, CUI）。

---

## 二、CLI (Command-line interface) 怎麼用？

Mac 上叫做終端機 (Terminal)，Windows 系統上叫做命令提示字元（command），就是打開後一個小小的黑盒子黑壓壓一片的東西就是了。

### 1. CLI 的常用基本指令

指令 (Mac OS / Linux)           | 用途  | 例子      <--標題列 (必要)
-------------------------------|-------|------|------     <--分隔列 (必要)
pwd 
(print working directory)      | 印出目前路徑位置 | pwd 
ls 
(List Segment)    | 列出檔案清單 |  ls<br>ls -a
(列出隱藏的檔案)<br>ls -l
(列出檔案相關資訊)<br>ls -al
(列出隱藏+資訊)
cd 
(change directory)  | 切換資料夾 | cd Desktop
(到桌面資料夾)<br>cd ~
(回到使用者層)<br>cd /
(回到根目錄)
man 
(manual)      | 指令使用說明書 | man la
clear      | 清空畫面 | clear