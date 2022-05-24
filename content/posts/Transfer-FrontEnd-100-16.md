---
title: "百日轉職前端工程師：第九週後端基礎 PHP 與 MySQL 《DAY 16》"
slug: Transfer-FrontEnd-100-16
date: 2020-09-06T21:36:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "PHP", "MySQL"]
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


大家好，這是百日轉職前端工程師的 Day16，也是 9/06（日），這週開始進到後端，真是令人既興奮又期待，畢竟一直聽說後端好像很難，但一直迷迷糊糊搞不清楚具體在幹嘛，而且之前在金融業 MA 學過 MySQL 還考了白癡的筆試，但一次也沒實際用過，終於要用了！終於要進後端了～～～～我好興奮呀 >////<


復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、 PHP 是什麼？

> **PHP 語言的全名是 PHP: Hypertext Preprocessor （PHP: 超文本預處理器）**
- PHP 和 ASP、JSP 等都是「動態網頁」開發語言
- 不過，PHP擁有跨平台的能力，可以加入 HTML 執行，也可獨立執行
- 無論是在Linux（最適合）、Unix、 Windows 都可以執行運作
- 不像微軟的 ASP 只能在 Windows 平台上執行
- 而且 PHP 是免費的，並可結合多種資料庫伺服器，如：MySQL、PostgreSQL、dBase、mSQL、Informix、ODBC、Oracle 等


> **PHP 用在撰寫「動態網頁」上**
- 它可以比 CGI 或者 Perl 更快速地執行動態網頁
- PHP 是將程序嵌入到 HTML 文檔中去執行，執行效率比完全生成 HTML 標記的 CGI 要高許多
- 因此尤其適用用在網頁開發
- PHP 還可以執行編譯後代碼，編譯可以達到加密和優化代碼運行，使代碼運行更快

> **PHP 語言是伺服器端 (Server) 執行的網頁**
- 不像一般 HTML 網頁，只要單機下開啟檔案就可以檢視網頁
- PHP 必須先在伺服器端執行完後，再將結果傳至使用者端 (Client) 的瀏覽器中檢視結果
- 所以必須使用網站伺服器，且伺服器要支援 PHP

> **在此我們要稍微提及上述說到的「動態網頁」，事實上網頁可以概略分為兩種**
1. 靜態網頁
    - 主要以 HTML、CSS、JavaScript 組成的網頁，沒有連接伺服器、資料庫
    - 在做靜態網頁的過程中只涉及 HTML 和 CSS 的工作我們俗稱叫做「切版」
2. 動態網頁
    - 有連接伺服器或資料庫，可以傳送資料，對資料進行動作。


### PHP 常用語法

其實 PHP 是一種相對容易好上手的語言，其語法跟 JavaScript 幾乎都是一致的，以下只有幾個比較不同的點如下：

#### 1. 使用 PHP 之前
- <?php 程式碼寫在這   ?>

#### 2. 宣告變數
- 用 $ 符號
- ex: $username = "Peter";

#### 3. 取用物件中的變數
- 用 -> 符號
- ex: $conn->prepare();

#### 4. 取用資料夾中其他 library
- 用 require_once();
- ex: require_once('conn.php');

#### 5. 印出變數、字串等
- 用 echo
- ex: echo "hello World"

#### 6. 印出物件
- 用 print_r();
- ex: print_r($row);

#### 7. 連接字串
- 用 .
- ex: echo $a . $b; 

#### 8. 用 while 取出 table 中所有 row
- while ($row = $result-> fetch_assoc()) {}
- 可以想把每行 row 中要輸出的內容夾在 { 之後，再宣告一個 <?php } ?>

#### 9. 讀取資料
- fetch_array()
    - 讀出的資料同時以數字與欄位名稱各自存一次在陣列中。
- fetch_assoc()
    - 讀出的資料 Key 值設定為欄位名稱。
- fetch_row()
    - 讀出的資料 Key 值設定為依序的數字。

#### 10. 取得陣列長度
- sizeof()
- ex: for($i = 0; $i <= sizeof($arr); $i +=1) {}

#### 11. 檢查是否有此變數/有值
- 是否有此變數：isset()
- 是否有值：empty()

#### 12. 輸出消息後退出程式
- exit() 和 die() 效果是完全相同的
- ex: exit($conn->error);



#### 2. 宣告變數
- 用 $ 符號
- ex: $username = "Peter";

#### 2. 宣告變數
- 用 $ 符號
- ex: $username = "Peter";

#### 2. 宣告變數
- 用 $ 符號
- ex: $username = "Peter";

#### 2. 宣告變數
- 用 $ 符號
- ex: $username = "Peter";
