---
title: "百日轉職前端工程師：第九週後端基礎 PHP 與 MySQL 《DAY 16》"
slug: Transfer-FrontEnd-100-16
date: 2020-09-06T21:36:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "PHP", "MySQL", "Cookie"]
author: "Boison"

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
- `<?php 程式碼寫在這   ?>`

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


---

## 二、前端與後端的差別？

基本上要一句話解釋就是：

> 前端工程師負責資料的呈現，後端工程師負責處理後台的資料處理。

但要談到前端後端就不得談到它們是怎麼合作的，用個比喻吧你比較好理解：

- 當你 (Client) 進到店裡你點了一道菜
- 這時候就外場服務生就要發出一個需求 (request) 給餐廳的櫃檯，其有點像是 API 的角色，會去確認你的需求/權限是否符合
- 接著通過櫃台把需求傳給餐廳的內場人員
- 而餐廳的內場人員就要根據這需求去調閱餐廳的食材庫 (database) 並且取出適當的食材加工後送回 (response) 給餐廳的櫃檯
- 櫃檯再交給外場的服務生，最後遞交到你 (Client) 手上


而餐廳後台從食材庫取出食材並且加工的一系列工作，這些工作對比到網頁上就是「後端」工程師要負責的事；而餐廳的門面菜單等等的設計以及外場服務生的服務則是「前端」工程師要負責的事。


> **至於此處我們就來稍微聊聊前後端會用到的工具吧**

### 1. 前端三本柱

- HTML
    - 是網頁的骨，可以製作出網頁基本的結構；你所看到網頁所有包含的各個元素，像是文字、連結、圖片、甚至是影片，都是HTML負責
- CSS
    - 是網頁的肉，為製作好的結構增添其他裝飾（字體字形、色彩、排列等）；可以說是網頁化妝師的角色，幫文字加上顏色、調整按鈕的大小並且加上邊框，甚至可以將整個網頁排版的更好看
- JavaScript
    - 讓網站動起來的關鍵，像是圖片輪播、點擊了 A 按鈕會跳出 B 視窗，又或者是點開某一個網頁時會出現一個 popup 視窗......等

### 2. 後端百家爭鳴

- PHP
    - 使用者多，最普及且簡單易學，但因一些古老的設計飽受批評
- Java
    - 老牌語言，國內外工作需求穩定，應用層面廣，但開發速度比較慢
- Ruby
    - 開發快速適合做原型產品，但使用人數較少，適不適合新手有爭議
- Python
    - 簡單易學，常用在數據分析上，但相較運行性能較差
- JavaScript (Node.js)
    - 前後端都可用，執行效率高但不適合 CPU 密集的應用
- Go
    - Google 力推，有很完善的標準庫，效能佳，但目前學習資源較少


- 參考資料
1. [初探網頁架構和前後端語言](https://medium.com/appworks-school/%E7%B6%B2%E9%A0%81%E6%96%B0%E6%89%8B%E5%85%A5%E9%96%80-%E5%88%9D%E6%8E%A2%E7%B6%B2%E9%A0%81%E6%9E%B6%E6%A7%8B%E5%92%8C%E5%89%8D%E5%BE%8C%E7%AB%AF%E8%AA%9E%E8%A8%80-a88a5dc86ee3)


---

## 三、什麼是資料庫？

> **談到資料庫，我們可以拆解成「資料庫」和「資料庫系統」**
- 最直覺的解釋，資料庫 (database) 就是儲存資料的地方，也可以理解為多個資料表的集合
- 但若要有效率地使用與管理資料庫的資料，我們還需要一套資料庫管理系統 (database management system，簡稱 DBMS)，作為使用者（或是應用程式）與資料庫之間溝通的平台

> **而資料庫系統又分成「關聯式資料庫」和「(NoSQL) 非關聯式資料庫」兩種如以下**

### 1. 關聯式資料庫

> 關聯式資料庫 (Relational Database Management System，縮寫為 RDBMS)
- 關聯式資料庫是目前最多軟體開發者使用的資料庫系統
- 其使用簡單，穩定度高，也有很多企業開放支援 SQL 的軟體可供使用
- 常見如 MySQL、PostgreSQL、Microsoft SQL Server、SQLite

※  MySQL 是目前市佔率最高的資料庫系統，但為何 XAMPP 上面是寫 MariaDB 而非 MySQL呢？這是因為當初 MySQL 被甲骨文公司收購後，大家擔心會不會突然有一天轉為商用，因此社群就 clone 出了 MariaDB ，但這兩個系統幾乎是一模一樣的。

#### 資料表 (table)

- 每一筆資料都會以紀錄  (record) 的方式被放入其中一個資料表 (table) 中
- 多個集合起來 table 則組成一個關聯式資料庫。每個資料庫裡都會有資料表 (table)
- 而資料表是由資料行 (column) 與資料列 (Row) 所組合而成
- 可以把資料表想像成像是Excel 那樣子的表格
- 資料行是 (column) 又可稱為欄位 (field) 或屬性 (attribute)
- 而資料列 (row) 又可稱為記錄 (record) 或值 (tuples)。

#### 資料之間的關聯

> **而資料之間大多會有清楚的關聯**
- 比方說 A 學生的各科考試成績存在「段考成績」的 table 中
- 而其住址、電話、父母姓名則存在「學生資料」的 table 中
- 而兩個資料表 table 中 A 學生的資料則可以用在兩個資料庫都存在的 A 學生「學號」關聯起來，以此類推


#### 資料庫結構（Table schema）

- 資料必定會有「名稱」和「類型」還有一些可以設定的欄位/屬性
- 名稱有點像是此筆資料的 key
- 通常第一個欄位都會設為 id 且勾選 AI (auto_increment)

> **常見類型如下**
1. 名稱：類似 key。
2. 類型
    - 數值：INT (整數，例如 id)、TINYINT (-128~127)。
    - 字串：CHAR（固定長度 0~255）、VARCHAR（可變長度 0~65535）、TEXT（~65535，適合放文章內容）。
3. 日期及時間
    - 日期：DATE。
    - 時間：DATETIME，常搭配預設值。
4. 長度/值：限制資料的長度。
5. 預設值：常用的是 CURRENT_TIMESTAMP 設定當下時間。
6. 編碼與排序：utf8_unicode_ci 和 utf8_general_ci 這兩種是最常用的。
7. 屬性：常設定 UNSIGNED（無符號），意思是只允許正整數。
8. 空值：是否允許空值？
9. 索引
    - 主鍵 (PRIMARY)：最主要的 key，不能為空值也不能重複，例如：id 、會員帳號、員工編號等。
    - 唯一 (UNIQUE)：不能重複，例如：email、姓名。
    - 索引 (INDEX)：可以快速搜尋。
10. AI (auto_increment)：資料會自動增加，保證遞增不保證連續，常用在 id。

#### SQL：操作資料庫的一種語言

- SQL (Structured Query Language 結構化查詢語言) 
- 是一種專門用來管理與查詢關聯式資料庫(Relational database)的程式語言
- 透過 SQL，我們能在關聯式資料庫裡執行某個 CRUD （Create, Read, Update, Delete）動作，同時也能建立和修改資料庫模式
- 它的語法簡單直接，一切都是以資料為主角去思考


#### phpMyAdmin：一種可以用圖形化介面管理資料庫的軟體

- XAMPP 是一個把 Apache 網頁伺服器與 PHP、Perl及 MariaDB 集合在一起的安裝包
- 允許用戶可以在自己的電腦上輕易的建立網頁伺服器
- 使用 XAMPP 連接 localhost 後直接輸入網址如下就會看到 phpMyAdmin，是一個用 PHP 寫的 GUI （圖形化介面）資料庫管理軟體。
    - http://localhost:8080/phpmyadmin/


> **在本機端 PHP 語法中必須要宣告伺服器（此處暫以本地端舉例）且去使用資料庫**
```
$sever_name = 'localhost';
$username = 'OOXX';
$password = 'OOXX';
$db_name = 'OOXX';
```
> **也常搭配宣告設定「時區」和「語法」**
```
$conn->query('SET NAMES UTF8');
$conn->query('SET time_zone = "+8:00"');
```

> **SQL 就是一個拿來查詢資料庫的語言**
- 因為他只是一個語言，所以你沒辦法說「我的程式是用 SQL 當資料庫」
- SQL 並不是一個資料庫系統，MySQL 才是
- 現在比較多人用的資料庫系統一般就三個：MySQL, PostgreSQL 還有 Microsoft SQL Server。共通點就是名字裡面都有 SQL

> **Apache HTTP Server（簡稱 Apache）是 Apache 軟體基金會的一個開放源碼的網頁伺服器軟體**
- 可以在大多數電腦作業系統中運行。由於其跨平台和安全性，被廣泛使用，是最流行的 Web 伺服器軟體之一
- 它快速、可靠並且可通過簡單的API擴充，將Perl／Python等直譯器編譯到伺服器中

> **整個網站前後端運作的系統**
1. 網頁伺服器（Apache）
    - 伺服器程式，專門處理 Request 跟 Response，是後端最重要的部分，網址也是由網頁伺服器來進行設定
2. 寫程式（PHP）
    - 網頁伺服器對檔案執行此後端語言，將 Request 處理成 output，而 output 通常是 html
3. 資料庫系統（MySQL）
    - 資料庫程式，專門操作資料庫，提供更有效率查找、修改資料的地方


***

### 2. NoSQL 非關聯式資料庫

> **上述的「關聯式資料庫」雖然好用，然而某些情況下仍然還是有某些缺點如**
1. 不擅長大量資料的寫入處理
2. 不擅長欄位不固定時的應用
3. 不擅長簡單查詢所需的快速回傳結果處理......等


> **因此常在大量資料需要處理的情況下仍會用到「非關聯式資料庫」**
- 尤其是在社群時代常有大量的「使用者生產內容 (user generated content)」情況下
- 比起 SQL 能夠無錯誤地同步處理結構清楚的資料，反而是「處理高速且大量產生的資料
- 但不需要即時同步，也不需絕對地零錯誤。
- 這種需求透過 NoSQL 非關聯式資料庫反而能更好的去解


> **NoSQL 意思是 no only SQL**
- NoSQL資料庫包括十幾種資料庫系統，也很不像關聯式資料庫那樣有一套通用的基礎資料庫理論
- 常用的有 MongoSQL
- 在操作上，NoSQL 並不支持 SQL 語法 與 SQL 的邏輯
- 所以，NoSQL 資料庫通常不使用關聯模型，也並不需要固定的結構 (也就是 schema-free)
- 但有需要時， NoSQL 也可以使用關聯模型與 schema


NoSQL 將聚集後的資料，作為儲存的最小單位，透過縝密豐富的資料結構，有利於將資料分散到多個節點；比起資料的關聯，NoSQL 更關注資料所代表的人（例如使用者）與物（例如一篇分享在社交平台上的文章）的「狀態」變動，例如文章被分享、按讚等。

> **幾種 NoSQL 常見的儲存類型如**
- 鍵值儲存 (Key Value / Tuple Store)
- 文件儲存 (Document Store)
- 圖學式儲存 (Graph Database)
- 寬列儲存 (Wide Column Store / Column Families)

根據網友的經驗，NoSQL 其實很適合搜集數據。例如說現在很多手機 App 其實會偷偷搜集你資料傳回去，做一些數據分析之類的。他可能會搜集：手機廠牌、型號、作業系統版本、安裝過的 App 等等的。這時候如果你是用一般傳統的 SQL 資料庫，你要怎麼定義 Schema？

你的欄位會有超級多個，而且一旦你想要儲存新的追蹤資訊的時候，你就必須去改一次資料庫，這是很麻煩的行為。這時候用 NoSQL 就很方便了，你只要直接把資料存進去就好，什麼都不用想。你不必知道到底有多少項的追蹤資訊。


> 參考資料
1. [SQL/NoSQL是什麼？](https://tw.alphacamp.co/blog/sql-nosql-database-dbms-introduction)
2. [關聯式資料庫 V.S. 非關聯式資料庫 (下)](https://lsyucode.wordpress.com/2017/01/15/%e9%97%9c%e8%81%af%e5%bc%8f%e8%b3%87%e6%96%99%e5%ba%ab-v-s-%e9%9d%9e%e9%97%9c%e8%81%af%e5%bc%8f%e8%b3%87%e6%96%99%e5%ba%ab-%e4%b8%8b/)
3. [閃開！讓專業的來：SQL 與 NoSQL  ](https://ithelp.ithome.com.tw/articles/10187443)

---


## 四、基本的 SQL 語法：Select、Insert Into、Delete 與 Update

### 1. INSERT 新增資料

>INSERT INTO 資料表名稱 (欄位名稱, 欄位名稱, …) VALUES (欄位值, 欄位值)

### 2. SELECT 查詢資料

> SELECT 欄位名稱, 欄位名稱, … FROM 資料表名稱
- 使用 * 查詢所有欄位
- 排序 ORDER BY DESC/ASC

### 3. UPDATE 修改資料
> UPDATE 資料表名稱 SET 欄位名稱 =新要改的欄位值 WHERE 欄位名稱 = 欄位值
- 補充 WHERE 後面條件可用 and/or

### 4. DELETE 刪除資料
> DELETE FROM 資料表名稱 WHERE 欄位名稱 = 欄位值

> 參考資料
1. [Day 60 - PHP 與 MySQL 入門](https://tw.coderbridge.com/@YSKuo/2da4b41f85134ac5a4042325b43b85dc)
2. [後端基礎 — 資料庫系統、Table schema、SQL 語法](https://miahsuwork.medium.com/%E7%AC%AC%E4%B9%9D%E9%80%B1-%E5%BE%8C%E7%AB%AF%E5%9F%BA%E7%A4%8E-%E8%B3%87%E6%96%99%E5%BA%AB%E7%B3%BB%E7%B5%B1-table-schema-sql-%E8%AA%9E%E6%B3%95-29243eeec35)

---

## 五、寫出基本的 CRUD 應用

> **再複習一次 CRUD 的意思就是 Create、Read、Update、Delete**
- 簡單來說你能夠寫出一個程式去串接 API，可能可以對資料庫的資料做以上四種動作。
- 但首先最基本的傳送資料就要用的屬性 method="POST" 或者 method="GET"  
- 這兩者基本上都是附加在 `<form>` 中的屬性
- 而 form 的另一個屬性 action 則是能夠指定接收資料的程式或者是伺服器
- 若是要對 MySQL 中的資料做處理，可以先傳到自己撰寫的 PHP 程式中再用 SQL 指定做更精細的處理

> **關於 GET 和 POST ，基本上需要保密的資料最好透過 POST 傳遞**

### GET
1. 網址差異 : 網址會帶有 HTML Form 表單的參數與資料
2. 資料傳遞量 : 由於是透過 URL 帶資料，所以有長度限制
3. 安全性 : 表單參數與填寫內容可在 URL 看到

### POST
1. 網址差異 : 資料傳遞時，網址並不會改變
2. 資料傳遞量 : 由於不透過 URL 帶參數，所以不受限於 URL 長度限制
3. 安全性 : 透過 HTTP Request 方式，故參數與填寫內容不會顯示於 URL


至於如何透過 PHP 和 SQL 實現 CRUD，此處不贅述，請參考 [實作第一個 CRUD 之 Create、Update、Delete](https://ithelp.ithome.com.tw/articles/10206716)  這篇文章。

> 參考資料 : [FORM 各種送出資料的方式](https://www.cs.pu.edu.tw/~tsay/course/webprog/notes/form)


---

## 六、什麼是 Session？

> **為什麼要有 Session**
- 因為 HTTP 為 stateless 的設計 Server 和 Client 不會一直保持連線狀態，也不會有雙方狀態的即時更新
- 所以 Server 並不知道 Client 的狀態（像是是否已經登入）
- 解決方式簡單來說就要讓 Clients 身上能夠儲存一個「狀態」
- 讓 Server 可以認出重複的 Clients 以提供更好的使用者體驗（你總不會希望你登入網頁後，下一頁又要重新登入吧？）
- 白話文來講 Session 的定義，就是一種讓 Request 變成 stateful 的機制
- 以小明的例子來說，Session 就是一種讓客人之間能互相關聯起來的機制
- 在故事裡面我們用了紙條跟手機裡的資訊來比喻，有多種方式可以達成 Session


>  **Session 更精確的定義是代表著：「具有狀態的一段期間」或者是「上下文」**
- 所以你想要開啟或是建立一個 Session，必要條件就是先有一個機制來建立及保留狀態
- 建立 Session 之後，所儲存的狀態就叫做 Session information，可以翻作 Session 資訊
- 在了解 Session 時必須有個觀念，就算沒有 Cookie 的存在，Session 機制也可以正常運作。

> **理解 Session 的原理 HTTP 上就是一樣的。只是在網頁中有兩種方法讓 Client 取得號碼牌（以狀態來理解）**
1. 一個是用 Cookie
2. 另一個則是把狀態資訊放在網址列上面或是藏在 form 表單中（Cookie 還沒出現前常使用的方式）

---
## 七、什麼是 Cookie？

> **簡單來說 Cookie 就是一個小型文字檔**
- 它的特性是建立後會跟著 Clients 發出的 Request 自動帶到 Server
- Server 可以透過 HTTP Response 把資料寫到 Cookie
- 在 Server 的 Response 中會有一個 Header 叫做 Set-Cookie: ...，裡面就能夠放一些資訊回傳給瀏覽器像是辨識使用者身份的 token
- 舉例以身份辨識來說，所有的 Requset 瀏覽器都會自動把 Cookie 帶上去讓 Server 可以辨識這個身份


> **Cookie 的用法如下（假設要存 token ）**
1. setcookie("token", $token, 到期時間) 
    - 登入時暫存
2. setcookie("token", "", 到期時間設成負值) 
    - 登出時清除 token


---
## 八、Session 與 Cookie 的差別？

> **在最原始的 Session 設計，大多開發者都將資料存在 Server 上**
- 也就是你點了什麼飲料，都是記錄在 Server 裡，可能是 Database、記憶體或是檔案，可以以任何一種形式儲存
- 然後，當你去領飲料時，店員會輸入你的號碼，用你的號碼得知你是否點過餐、點了什麼東西

然而一旦網站的架構複雜化，Server 要處理的任務急遽增加時，後端怎麼存放和共享這個 session 資料，又要兼顧效能和方便維護，就變成是很大的問題。

> **因此 Cookie-based Session 就被提出為一個解決方案**
- 把資料暫存放在 Cookie 中，讓 Client 自己負責保存
- 建立 Session 之後若選擇把 Session information 這些資訊存在 Cookie 裡面，就叫做 Cookie-based session
- 還有另一種方法則是在 Cookie 裡面只存一個 SessionID，其他的 Session 資訊都存在 Server 端，靠著這個 ID 把兩者關聯起來
- 不過因為因為 Cookie 有 4K 資料大小的限制，很多網站服務會選擇 cookie-based 和後端儲存並行的方案

> **有了 Session 之後，會碰到資料被竄改的問題這時候有兩種解決方式**
- 一個方法是 Cookie-based session，意思是你照舊把狀態存在 Cookie，但是加密以後再存
- 另一個方法是把狀態存在 Server 端，靠一個 SessionID 來辨識，這個狀態你可以存成檔案，可以存在記憶體裡，也可以存在資料庫
- 實作方式的不同而已但原理都是一樣的

> 參考資料:
1. [Huli 之 Session 與 Cookie 三部曲](https://blog.huli.tw/2019/08/09/session-and-cookie-part2/)
2. [Web 技術中的 Session 是什麼？](https://fred-zone.blogspot.com/2014/01/web-session.html)


---

## 九、結論

> 「我的資料庫存了一萬個欄位，寫得都是要對妳說的話。」