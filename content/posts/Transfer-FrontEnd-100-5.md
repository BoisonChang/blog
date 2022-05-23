---
title: "百日轉職前端工程師：第一週用 CLI 命令電腦《DAY 5》"
slug:  Transfer-FrontEnd-100-5
date: 2020-06-15T20:18:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "CLI"]
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

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F114b5c67-c532-4e77-9ac0-733e8cbaacac_1460x1364.png)

> 資料來源: [Windows CMD 與 MacOS / Linux 終端機指令列對照表](https://happycoder.substack.com/p/windows-cmd-macos-linux-?s=r)

### 1. CLI 的常用基本指令

#### pwd (print working directory): 印出目前路徑位置

#### ls (List Segment): 列出檔案清單

- ls
- ls -a : 列出隱藏的檔案
- ls -l : 列出檔案相關資訊
- ls -al : 列出隱藏+資訊

#### cd (change directory) : 切換資料夾

- cd Desktop : 到桌面資料夾
- cd ~ :回到使用者層
- cd / :回到根目錄

#### man (manual): 指令使用說明書

- man la

#### clear: 清空畫面

---

### 2. CLI 的常用檔案相關指令

#### touch: 建立檔案 / 更改時間

- touch test.txt

#### rm (remove): 刪除檔案

- rm test.txt
- rm -f test.txt : 強制刪除 test.txt
- rm -r TEST : 刪除 TEST 資料夾&裡面檔案

#### mkdir (make directory) : 建立資料夾

- mkdir TEST
- rmdir TEST : 刪除 TEST 資料夾

#### mv (move) : 移動檔案或改名

- mv test.txt TEXT : 檔案 text.txt 移至 TEXT
- mv test.txt test2.txt : 檔案 text.txt 改名 test2.txt

#### cp (copy) : 複製檔案

- cp test.txt test2.txt : 檔案 text.txt 複製一份命名 text2.txt
- cp -r TEXT TEXT2 : 複製資料夾 TEXT 和裡面所有檔案

#### vim : 文字編輯器

- vim text.txt
- 按 i 可以打字
- 按 esc 進入普通模式
- :q 離開
- :wq 存檔離開

#### open: 打開

- open test.txt
- open .. : 打開資料夾)

---

### 3. CLI 的常用其他指令

#### cat : 列印出檔案內容

- cat test.txt

#### less : 分頁印出檔案

- less test.tst

#### echo : 列出出打的東西

- echo 123

#### grep : 抓取關鍵字 (ex: A)

- grep A test.txt

#### wget : 下載檔案（有可能需安裝

- wget url

#### curl : 送出 repuest

- curl api url : 秀出 response
- curl -I url : 秀出 header 資訊

#### > (redirection) : 重新導向 input output

- echo 123 > test.txt

#### >> (append) : 可以新增內容進檔案

- echo 123 >> test.txt

#### | (pipe) : 左邊指令的輸出變成右邊指令的輸入

- cat test.txt | grep A

#### date : 輸出日期

#### top (tab of process) : 查詢系統目前的工作狀態

---

## 三、CLI 應用實例

假設你想要完全不透過圖形介面，建立一個叫做 wifi 的資料夾，並且在裡面建立一個叫 afu.js 的檔案，該怎麼做？

1. 跳到桌面：**cd Desktop**
2. 建立資料夾 wifi： **mkdir wifi**
3. 跳到資料夾 wifi：**cd wifi**
4. 建立檔案 afu.js：**touch afu.ja**
5. 查看所有位置所有檔案：**ls**

---

四、結論：可以開啟來玩玩看

沒什麼結論，要當工程師就要熟悉 CLI，沒有要當工程師不用，但還是可以開啟來玩玩看。
