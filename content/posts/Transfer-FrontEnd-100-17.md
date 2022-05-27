---
title: "百日轉職前端工程師：第十一週資訊安全復盤 《DAY 17》"
slug: Transfer-FrontEnd-100-17
date: 2020-11-12T15:42:21+08:00
draft: false
tags: ["FrontEnd", "Transfer"]
keywords: ["FrontEnd", "Transfer"]
author: "Boison"

---

大家好，這是百日轉職前端工程師的 Day17，也是 11/12（四），這週開始進到資訊安全，「資訊安全」是前端工程師滿重要的一環，要確保你的網頁不會被使用者亂搞就壞掉，或者被駭客竊取資料。

直接存明文密碼在資料庫內在資訊安全上非常危險，若資料庫被駭、或者管理員居心不良，都可能取得你的常用密碼，因此密碼需要經過處理過後才存入資料庫，除此之外也要預防使用者透過一些輸入去攻擊更改你網站的資料！

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、什麼是雜湊（Hash function）？

> **雜湊（Hashing）是電腦科學中一種對資料的處理方法**

- 通過某種特定的函式/演算法（稱為雜湊函式/演算法）將要檢索的項與用來檢索的索引（稱為雜湊，或者雜湊值）關聯起來，生成一種便於搜尋的資料結構（稱為雜湊表）
- 雜湊演算法也被用來加密存在資料庫中的密碼（password）字串，由於雜湊演算法所計算出來的雜湊值（Hash Value）具有不可逆（無法逆向演算回原本的數值）的性質，因此可有效的保護密碼
- 雜湊（Hashing）不需密鑰，無法逆向解出原始輸入，也就是說雜湊具有不可逆的性質
- 但假如多個輸入會產生同個輸出值的話，稱為碰撞 （Collision），這就代表說這個雜湊值已經不安全，不再是獨一無二的了，需要更改雜湊函數
- 雜湊常見的應用有 Git commit 產生的 ID 和區塊鏈上資料的難以竄改的特性

> **PHP 內有內建的 hash 函數，其形式為**

```
$password = password_hash($_POST['password'], PASSWORD_DEFAULT)
```

- 在使用時，需要搭配第二個參數，推薦直接使用 PASSWORD_DEFAULT
- 每次處理時，都會在背後產生隨機的 SALT
- 也可以手動指定要使用哪一個 SALT，但最好不要，就交給 PASSWORD_DEFAULT 來隨機處理，會更加安全
- 另外也可以搭配第三個 cost 參數 (默認為 10)，當值調整越大，所需耗費的計算時間就會越多，可以自行測試

> **要破解雜湊只有暴力破解 （brute-force），彩虹表（ rainbow table ），和字典法 / 雜湊表（Dictionary Attacke）三種方式，前兩種都可以歸類到暴力破解，以下介紹雜湊表：**

### 1. 雜湊表 (Hash table)

- Hash Table 是一種儲存 {key: value} 的資料結構
- key 值會被丟到 Hash Function 中計算，而計算結果 Hash Value 為 {key: value} 在 Hash table 中的 index 值
- 有了 key 就像擁有浪漫因子，馬上就能找到專屬於你的 value
- 但存取資料的速度快是利用空間換來的，也因此 Hash Table 較占用記憶體空間
- 但演算法運用 Hash table 有些情況能夠大幅降低空間複雜度
- Hash Table 的一個簡單應用就是「搜尋引擎」。

### 2. 加料式雜湊法 (Salted Hash)

- 如果駭客有了足夠完備的雜湊表，是有可能透過查表的方式破解原始的明碼的
- 因此為了提升安全性，又發明了加料式雜湊法，替用戶的明碼添加亂定數 (salty)，再使用雜湊函數進行運作
- 為了安全性，亂定數（salty）的取值就變得非常重要，例如值不得太短，且值必須不是常見的值（盡量是亂數或近乎亂數產生的值）

> **最後，補充雜湊比較廣為人知的實作如下**

- MD5（全名為 Message-Digest Algorithm），此演算法被定義在 RFC 1321
- SHA（全名為 Secure Hash Algorithm），由 NSA（美國國家安全局） 所設計

因 MD5 已被認定為不安全的演算法，而不安全主要是因為它無法防止碰撞（collision）。

- 相較 SHA 是比較安全的演算法，只是舊版的也有被攻破的記錄兩個演算法的歷史如下:
  - MD4：1990 年發布，但 1991 年馬上發現弱點，而在 2004 年證明會發生碰撞
  - MD5：1992 年發布，目的是取代 MD4，但 1996 年發現弱點，一樣也是 2004 年證明會發生碰撞
  - SHA-0：1993 年發布，但 NSA 馬上又撤回
  - SHA-1：1995 年發布，並被廣泛應用到許多需要安全雜湊的協定上，如 TLS。但 2005 年發現了有效的攻擊方法，2017 年 Google 即宣布成功的 SHA-1 碰撞攻擊
  - SHA-2：2001 年發布，底下又分為 6 種不同的演算法，如 SHA-256。目前還沒有有效的攻擊方法
  - SHA-3：2015 年發布，雖然 SHA-2 還沒找到弱點，但還是得準備一個更安全可替換的雜湊方法，以防 SHA-2 又被攻破

> 參考資料

1. [破解區塊與鏈堅不可摧的秘密](https://medium.com/@yijyun10100926/%E8%8F%9C%E9%B3%A5vc-%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-01-blockchain-f19c10f8aec)
2. [Best practicing for password protection](https://plainpass.com/2012/06/)
3. [白話的 Hash Table 簡介](https://blog.techbridge.cc/2017/01/21/simple-hash-table-intro/)
4. [簡介雜湊](https://mileschou.me/ironman/11th/authentication/day09/)
5. [認識 Hash Table](https://medium.com/@yl21/%E8%AA%8D%E8%AD%98-hash-table-e51b5b2f01fc)

---

## 二、什麼是加密（Encryption）？

> **加密（encryption）指的是將純文字（plain text）透過金鑰（key）和加密演算法（encryption algorithm）轉換成加密後的密文（ciphertext, encrypted text）**

- 加密後的內容可以透過相同的金鑰和演算法解密回原本的文字內容（original text）
- 簡單來說密碼會以 Key 值為基礎作一處理後偏移，加密的演算法可簡單可複雜
- 但基本上由於密碼與加密後的密碼是一對一的關係，還是有機會被回推

### 1. 對稱性加密

> **密碼學裡面有兩種加解密方式，一種是對稱性加密，一種是非對稱性加密**

對稱性加密，意思就是我加密跟解密用的是同一個鑰匙，所以只要 A 跟 B 都知道這把鑰匙，A 要傳給 B 的時候，就用這個鑰匙加密。B 拿到了之後再用同一個鑰匙解密，你們兩就可以互通有無，暗通款曲。即使有中間人攔截了你們的傳輸，只要他沒有你們的鑰匙，他就不知道你們在幹麻。

但 A 總要告訴 B 一次這個鑰匙是什麼吧，若是途中被人攔截下來，之後駭客就能透過這把鑰匙去解密你們互相傳輸加密過的訊息，所以這仍舊有風險，能夠再透過非對稱加密去提高資訊安全程度。

### 2. 不對稱性加密

> 公鑰加密私鑰解密，也可以私鑰加密公鑰解密！

非對稱式加密，就是每個鑰匙 pair 有兩個鑰匙，一個公鑰一個私鑰。

> 中心思想就是利用公鑰可以用來加密的特性，如果你是用公鑰加密你必須用私鑰解密。

所以根本不怕公鑰流出（公開給所有人也沒差），只要我私鑰保存好就好，我私鑰根本就沒傳過，也不可能被攔截。

實際運作上 A 有他自己的私鑰跟 B 的公鑰，B 有他自己的私鑰跟 A 的公鑰。A 要傳東西給 B 就用 B 的公鑰加密，然後 B 拿到之後用 B 自己的私鑰解密，即使在中途被攔截，只要 B 的私鑰沒有流出就完全不會有事。因為第一次的傳輸也只互傳公鑰，所以即使公鑰被攔截，之後中間人拿到加密過的訊息也不能怎麼樣，因為那個要對方的私鑰才能解。

> 但假設既然大家都有 B 的 public key ，那任何人都可以用 B 的公鑰加密傳訊息給 B，那 B 怎麼知道哪個是 A 寫的哪個是別人偽造的呢？

這裡就要引進數位簽章的概念。數位簽章就是 A 在傳送訊息前，用 A 的私鑰加密傳給 B，B 再用 A 的公鑰來看是不是真的是 A 簽名的（事實上是對內容的 Hash 簽名，不過為了講解方便，就先當直接對內容簽）。

> 簡單來說，上述程序為：A 要傳給 B 之前，把要傳的內容，先用 B 的公鑰加密再用 A 的私鑰簽，然後 B 用 A 的公鑰確認簽章再用 B 的私鑰解密內容。

---

## 三、雜湊、加密與編碼的差別？

加密（Encrypt）和雜湊（Hashing）都是一種處理密碼的方式，而編碼（Encoding）也是一種類似加密的方式，但由於其安全性過低通常只用在非重要資訊的資料傳輸上。

- 雜湊（Hashing）常用在平台的密碼驗證，因為平台方實際上也不需要知道使用者真正輸入的密碼，因此透過雜湊不可逆推的機制，能夠最好的保護使用者的明碼
- 而區塊鏈則會用到不對稱加密（Encrypt）的機制，讓一個訊息能夠透過私鑰及公鑰被加密傳輸和保護
- 而編碼（Encoding）則像是摩斯密碼基本上只是換個方式表達資料，別人只要懂這套轉換規則，就有辦法翻譯回來，所以編碼基本上完全沒有安全性可言

基本上編碼、加密跟雜湊三者各有優缺，適用的情境也不太一樣，所以實務上常常各取所長把他們混在一起用，譬如說壓縮檔加密就同時用到編碼跟加密、JWT（JSON Web Token）用到編碼跟雜湊、HTTPS 的實現則是用到加密跟雜湊。

### 1. 雜湊

- 資料是否可逆 : 不可
- 運算後資料長度 : 一樣
- 安全性 : 好
- 輸入與輸出 : 多對一

### 2. 加密

- 資料是否可逆 : 可
- 運算後資料長度 : 不一樣
- 安全性 : 好
- 輸入與輸出 : 一對一

### 3. 編碼

- 資料是否可逆 : 可
- 運算後資料長度 : 不一樣
- 安全性 : 無
- 輸入與輸出 : 一對一

> 參考資料

1. [網路安全(1) - 基礎密碼學](https://blog.techbridge.cc/2017/04/16/simple-cryptography/)
2. [一次搞懂密碼學中的三兄弟 — Encode、Encrypt 跟 Hash](https://medium.com/starbugs/what-are-encoding-encrypt-and-hashing-4b03d40e7b0c)
3. [加密和雜湊有什麼不一樣？](https://ithelp.ithome.com.tw/articles/10193762)

---

## 四、什麼是 SQL Injection ？又要如何防範？

在資訊安全中有一項針對資料庫的漏洞攻擊稱作 SQL Injection，不僅僅是網頁程式，只要是有和資料庫進行連結的任何程式都可能產生此項漏洞。

此漏洞的成因很簡單，就是允許使用者輸入的字串在代入 SQL 查詢語句時，沒有過濾非法字元，導致字串成為查詢語句的一部分，達到讓攻擊者能執行任意 SQL 語句。

### 1. 常見的 SQL INjection 攻擊手法

#### Authorization Bypass（略過權限檢查）

> "SELECT _ FROM customers WHERE name =' -name- ' AND password = ' -password-'_

假設今天有一段 Query statement 要求使用者輸入帳號及密碼，statment 中有兩個 input 值 name 與 password，會有兩個可供使用者輸入值的方塊，但是有心的攻擊者當然不會乖乖的輸入帳號及密碼囉！！透過在' -name- '所對應的方塊內輸入：'OR 1=1 --，會使 Query statement 變為

#### Injecting SQL Sub-Statements into SQL Queries（注入 SQL 子語法）

> http://www.mydomain.com/products/products.asp?productid=123; DROP TABLE Products\*

攻擊者可以在注入惡意的 SQL 的語法去改變資料庫，如加入一段 malicious commands 如上，這個 sub command 會命令 SQL server 將 Products 這個 Table 刪除掉。

> http://www.mydomain.com/products/products.asp?productid=123 UNION SELECT Username, Password FROM USERS

其中 UNION 能將兩個 SELECT 的結果用一個結果集呈現出來，而第二個 SELECT 是將 USERS 這個 Table 的 Username 與 Password 呈現出來，以竊取資料庫中存放的所有使用者的帳號密碼！

#### Exploiting Stored Procedures（利用預存程序）

> SomeAsp.asp?city=pune';EXEC master.dbo.xp_cmdshell' cmd.exe dir c:

Stored Procedures（預存程序）是將又臭又長又常用的 SQL 語法寫成一組程序並儲存起來，以供後續呼叫相同程序時不必再將完整個 SQL 語法重打一次，攻擊者亦可透過呼叫這些 Stored Procedures 進而對 DataBase 進行攻擊。

透過 EXEC 去執行 master.dbo.xp_cmdshell 這個預存程序，並帶一參數 cmd.exe dir c: 代表想讓預存程序執行的內容。

### 2. SQL INjection 防範手法

目前為止最推薦的防範方法是預處理 （Prepared Statement），Prepared Statement 會替 SQL 語句進行預處理，再利用它提供的 bindValue 或 bindParam 函式將欲查詢的參數的值或變數綁定上去，底層查詢時，其參數會保證作為數值傳遞，不可能成為 SQL 語句的一部分，也因此就不會產生 SQL Injection 的問題。

基本上有使用者可以操作的地方都需要加上預處理 （Prepared Statement），而如果都是自己可以控制的，照理來說不需要，但還是建議每個地方都用，好處除了統一標準外，也是為了將來可能需求變動，有可能改成使用者會 輸入的欄位做準備。

> **在 PHP 中需要用到 Prepared Statement 語法如下**

```
$sql = "insert into users(username) values(?)";
$stmt = conn−>prepare(sql);
$stmt->bind_param("s", $username);
$result = $stmt->execute();
$content = $stmt->get_result();
$row = $content->fetch_assoc();
```

> **其他 SQL INjection 防範方式也可視情況使用**

1.  使用 Regular expression 驗證過濾輸入值與參數中惡意代碼，將輸入值中的單引號置換為雙引號
2.  限制輸入字元格式並檢查輸入長度
3.  資料庫設定使用者帳號權限，限制某些管道使用者無法作資料庫存取......等

> 參考資料: [攻擊行為－SQL 資料隱碼攻擊 SQL injection](https://ithelp.ithome.com.tw/articles/10189201)

---

## 五、什麼是 XSS ？又要如何防範？

> **XSS（Cross-site scripting），也叫做 JavaScript Injection**

- 是現代網站最頻繁出現的問題之一，指的是網站被惡意使用者植入了其他程式碼
- 通常發生在網站將使用者輸入的內容直接放到網站內容時
- 例如論壇、討論區等可輸入任意文字的網站，惡意使用者如果寫入` <script>`，且前端、後端都沒有針對輸入內容做字元轉換、過濾處理，直接將使用者輸入的字串當成頁面內容的話，就有可能遭到 XSS
- XSS 攻擊最常見的就是盜取 Cookie 中的資料，更甚者會直接讓頁面當機無法正常呈現

> **可以分成「非持久型 XSS 攻擊」和「持久型 XSS 攻擊」兩種**

- 非持久型 XSS 攻擊顧名思義是一次性的，僅對當次的頁面訪問產生影響
  - 非持久型 XSS 攻擊要求使用者訪問一個被攻擊者篡改後的連結
  - 使用者訪問該連結時，被植入的攻擊指令碼被使用者遊覽器執行，從而達到攻擊目的
- 持久型 XSS，會把攻擊者的資料儲存在伺服器端，攻擊行為將伴隨著攻擊資料一直存在

> **而其更常見是分成以下三種攻擊手法**

### 1. 常見的 XSS 攻擊手法

#### 儲存型（Stored） XSS

> **將惡意程式寫入 DB，等資料被讀取出來時就會執行**

- 會被保存在伺服器資料庫（DB）中的 JavaScript 代碼引起的攻擊即為儲存型 XSS
- 最常見被攻擊的就是論壇文章、留言板等等
- 因為使用者可以輸入任意內容，若沒有確實檢查，那使用者輸入如 `<script>` 等關鍵字就會被當成正常的 HTML 執行，標籤的內容也會被正常的作為 JavaScript 代碼執行，會被存在 DB 裡，所以每個使用者打開都會看到被修改的內容，殺傷力很大！

#### 反射型（Reflected） XSS

> **使用者輸入的內容直接帶回頁面上**

- 反射型 XSS 是指不會被儲存在資料庫中，而是由網頁後端直接嵌入由前端使用者所傳送過來的內容造成的
- 最常見的就是以 GET 方法傳送資料給伺服器時，伺服器未檢查就將內容回應到網頁上所產生的漏洞，若是 html 元素就會被用 html 的方式顯現

#### DOM-based 型 XSS

> **網頁 JavaScript 在執行過程中，沒有詳細檢查資料使 js 產生網頁內容的時候被注入惡意字串**

- DOM 型 XSS 攻擊中，取出和執行惡意代碼由瀏覽器端完成，屬於前端 JavaScript 自身的安全漏洞
- 其他兩種 XSS 都屬於 Server 端的安全漏洞
- DOM-Based XSS 就是指網頁上的 JavaScript 在執行過程中，沒有詳細檢查資料使得操作 DOM 的過程代入了惡意指令
- DOM-based 型 XSS 通常需要搭配前兩個手法，先讓內容保存在伺服器資料庫中、或是以反射型的方式製造出內容再藉由 JavaScript 動態產生有效的 DOM 物件來運行惡意代碼
- 反射型 XSS 和 DOM-based 型 XSS 都需要在 url 加入 JavaScript 程式碼才能夠觸發

### 2. XSS 防範手法

> **儲存型 XSS 和 反射型 XSS 兩種類型必須由後端來防範，而 DOM-Based 則必須由前端來防範。**

但基本上還是跟前面的原則相同，防範方法簡單來說，就是對於所有不信任來源的 input 都要以 encode 後的方式呈現在瀏覽器上，最好都改成 .innerText() ，只會輸出純文字，對於 XSS 的攻擊來說，輸出的編碼更為重要。

因為輸入驗證因為 XSS 有太多漏洞可以鑽： HTML、JavaScript、CSS、XML、URL，要很完整對輸入做防範非常困難，例如刪除所有 `<script>`、 onerror 及其他可以執行 JavaScript 的字串，但設黑名單也不是一個理想的方式，因為有太多種變形可以換，白名單是比較推薦的作法，只是要寫的完整也是非常麻煩。

> **輸入輸出的防範方式其可分為兩種方式**

1. 驗證
   - 使用者輸入的內容後，才把資料存入資料庫，例如，過濾掉 `<script>` 這類 html 標籤
2. 消毒
   - 在將資料庫的內容呈現給使用者前，先對這些內容進行消毒（sanitize），例如將內容中的 HTML Body 和 attribute 內的 HTML Entities 都進行編碼

> **JavaScript 中一種好用的防範方式為使用內建的 htmlspecialchars 函數**

- htmlspecialchars() 函數把特殊字符轉換為 HTML 實體
- 這意味著 < 和 > 之類的 HTML 字符會被替換為 < 和 >
- 這樣可防止攻擊者通過在表單中注入 HTML 或 JavaScript 程式碼（XSS 攻擊）對程式碼進行利用
- 通常會在網頁渲染「輸出」時才轉換。
- htmlspecialchars 函數更多的時候要加上第二個參數：htmlspecialchars（$ string，ENT_QUOTES），否則預設是只轉化雙引號的
- 當然，如果需要不轉化任何引號，用 htmlspecialchars（$ string，ENT_NOQUOTES）
- 而第三個參數則可以設定是針對英文或者中文等語言

```
function escape($str) {
return htmlspecialchars($str, ENT_QUOTES);
}
```

---

## 六、為什麼儘管前端做了驗證，後端還是要再做一次驗證？

- 前端驗證是在 Client 端使用 Javascript 預先檢查表單的內容
  - 使用前端驗證除了能有效減少 Request 的數量（因為後端驗證就會發 Request），也會讓使用者體驗比較舒服一點（畫面不會閃一下）
- 後端驗證則在 Server 端作為資料保護最後一道防線
  - 但後端驗證也能透過 Ajax 達到畫面不閃的效果（遠端驗證 Remote Validation）
- 理論上資料驗證都是需要分開做兩次
- 我自己理解做兩次的理由是可以分散消耗的資源，在畫面渲染和延遲上取到一個平衡點，也確保沒驗證到疏漏以防萬一

> 參考資料

1. [[鐵人賽 Day15] - Model Validation(1) / 前端 vs. 後端驗證](https://ithelp.ithome.com.tw/articles/10206683?sc=iThelpR)
2. [前後端渲染以外，前後端驗證有什麼不同？](https://noob.tw/client-server-side-validation/)

---

## 七、什麼是 CSRF？如何防範？

> **CSRF 是一種 Web 上的攻擊手法，全稱是 Cross Site Request Forgery（跨站請求偽造）**

- 簡單來說當用户登錄原網站，瀏覽器會記錄 Cookies
- 如果用户未登出或 Cookies 並未過期（用户關閉瀏覽器不代表網站已登出或 Cookies 會立即過期），在這期間，如果用户造訪其他危險網站，點擊了攻擊者的連結，便會向原網站發出某個功能請求 （request），原網站的伺服器接收後會被誤會以為是用户合法操作
- 簡單來說，可能你在其他釣魚網站點了不知名的按鈕，卻被駭客利用你本人發送 request 去登陸/取得你在某平台上的帳戶資料

### CSRF 防範手法

> **防範方式從三個面向**

#### 1. 使用者端 （Clients）的防範方式

> 基本上使用者端能夠做的事情並不多

- 登錄網站後，使用期間不要去瀏覽不明的網站，使用完畢後記得馬上登出。
- 避免在瀏覽器自動儲存帳户名稱或密碼。

#### 2. 伺服器端 （Server）的防範方式

- 檢查 Referer
  - request 的 header 裡面會帶一個欄位叫做 referer，代表這個 request 是從哪個地方過來的，可以檢查這個欄位看是不是合法的 domain
- 加上圖形驗證碼、簡訊驗證碼
  - 基本上萬無一失的方法，常見於銀行帳戶等需要高度安全的領域，但若是每個網頁都要這樣，對於使用者很不方便
- 加上 CSRF token
  - 我們在 form 裡面加上一個 hidden 的欄位，叫做 CSRF token
  - CSRF token 裡面填的值由 Server 隨機產生的亂碼，並且存在 Server 的 session 中
  - 按下 Submit 之後，Server 比對表單中的 CSRF token 與自己 session 裡面存的是不是一樣的，是的話就代表這的確是由使用者本人發出的 request
  - 這個 CSRF token 由 Server 產生，並且每一段不同的 session 就應該要更換一次
  - 但是，攻擊者如果掌握了你底下任何一個 subdomain，就可以幫你來寫 cookie，並且順利攻擊了
- Double Submit Cookie
  - 這個解法的前半段與 CSRF token 的相似，由 Server 產生一組隨機的 token 並且加在 form 上面
  - 但不同的點在於，除了不用把這個值寫在 session 保存在 Server 當中以外，同時也讓 Client side 設定一個名叫 CSRF token 的 cookie，值也是同一組 token，利用「cookie 只會從相同 domain 帶上來」機制，使攻擊者無法從不同 domain 戴上此 cookie

#### 3. 瀏覽器端 （Broswer）的防範方式

最後補充下從 Browser 端本身也能進行防禦，而且在適當的條件下意外的簡單方便，Google 在 Chrome 51 版時加入此功能「SameSite cookie」。只要將妳原本設置的 Cookie 的 header 改幾行代碼即完成。原理簡單來說你的 cookie 只要不是從原本網頁來的就會被消除掉，由於目前只有 Chrome 支援這個新的特性，就不多贅述。

- 原先：Set-Cookie: session_id=ewfewjf23o1;
- 修正：Set-Cookie: session_id=ewfewjf23o1; SameSite

> 參考資料

1. [讓我們來談談 CSRF](https://blog.techbridge.cc/2017/02/25/csrf-introduction/)
2. [雜湊與加密 & 常見攻擊：SQL Injection、XSS](https://hackmd.io/@Heidi-Liu/note-be101-php-security)

---

## 八、結論

> 『我的資訊安全天衣無縫，卻只防不住妳的入侵 。』
