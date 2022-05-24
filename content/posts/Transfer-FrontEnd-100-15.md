---
title: "百日轉職前端工程師：第八週前端整合串 API 《DAY 15》"
slug: Transfer-FrontEnd-100-15
date: 2020-09-04T20:11:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "API"]
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

大家好，這是百日轉職前端工程師的 Day15，也是 9/04（五），這週開始整合之前所有學會，前端切版（HTML + CSS），以及 JS 並且去串接網路上的 API 做出一個動態更新的頁面，老實說一開始真得覺得很難，真的實際做出 Twitch 的 Top 排行榜時，內心超級感動呀！但要讀懂 API 文件對我來說還是一件有點難度的事，尤其很多大公司的 API 文件是密密麻麻的英文，根本像是大海撈針，但開始有慢慢抓到訣竅。


復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、什麼是 API？

> **API 全名 Application Programming Interface （應用程式介面）**
- 白話講，API 就是取用資料的一套「溝通介面」
- API 需要有 API 網址和 API 規範
- 通常前端工程師通過 API 去取用資料庫中的資料，同時也必須遵守 API 自有的一套規範
- 這套規範通常是由撰寫 API 的後端工程師所規範，撰寫在 API 文件中
- 之所以要有 API 是為了讓取用資料庫資料的流程可以透過標準化所簡化
- 也是為了讓外部人士要取用資料庫資料的時候，能夠保障資料庫中原始資料的資訊安全

> **在 Web Application 的開發情境下的 API 被稱為 Web API**
- 在 Web API 作用時，客戶端和伺服器端會透過 HTTP 通訊協定來進行請求與回應
- 使用 Web API 的一方是客戶端，可能是瀏覽器、手機或者穿戴式設備等等
- 客戶端會向伺服器端發出請求，要求執行某個 CRUD （Create, Read, Update, Delete）動作
- 伺服器端會依照客戶端的請求內容，經過內部的處理程序後，將結果回應給客戶端

---
## 二、什麼是 AJAX？

- 非同步 JavaScript 及 XML（Asynchronous JavaScript and XML，AJAX） 並不能稱做是種「技術」
- 而是 2005 年時由 Jesse James Garrett 所發明的術語，描述一種使用數個既有技術的「新」方法
- 這些技術包括 HTML 或 XHTML、層疊樣式表、JavaScript、文件物件模型、XML、XSLT 以及最重要的 XMLHttpRequest 物件
- 當這些技術被結合在 Ajax 模型中，Web 應用程式便能快速、即時更動介面及內容，不需要重新讀取整個網頁，讓程式更快回應使用者的操作

AJAX 是「Asynchronous JavaScript and XML」（非同步的 JavaScript 與 XML 技術）的縮寫，簡單說就是網頁不用重新整理，就能即時地透過瀏覽器去跟伺服器溝通，瀏覽器會直接撈出資料 render 出結果。（render / 渲染 / 生成 HTML）。

### 1. 非同步請求 (Asynchronous request)

> **在非同步請求 (Asynchronous request) 還沒被開發之前**
- 如果我們要瀏覽一則留言，就必須向 Server 重新 request 一個完整的頁面
- 也就是所謂的「用表單送出資料」
- 等待頁面切換的這段時間畫面往往會卡住不動，直到接收 response，才會重新渲染 (render) 畫面

> **而 Ajax 技術的出現讓瀏覽器可以向 Server 請求資料而不需費時等待**
- 當瀏覽器接收到 response 之後，新的內容就會即時地添入原本網頁
- 這也是為什麼當我們瀏覽 Facebook、Twitter 內容時，不會看見整個網頁一直重新整理


伺服器對 AJAX 資料請求回應通常是以三種資料格式其中之一（HTML、XML、JSON），最常與 Javascript 做搭配就是 JSON。

> **三種資料格式應用情境**
1. 向伺服器請求 HTML 片段，然後客戶端瀏覽器上的 JavaScript 再替換掉頁面上的元素
2. 向伺服器請求 JavaScript 程式腳本，然後客戶端瀏覽器執行它
3. 向伺服器請求 JSON 或 XML 資料格式，然後客戶端瀏覽器的 JavaScript 解析後再動作


### 2. 如何發起一個 AJAX 請求？

- XHR 的全稱是 XMLHttpRequest
- 這是由微軟首先引入的一個特性，其他瀏覽器提供商後來都提供了相同的實現
- 這跟以前的技術最大的不同點在於「頁面無需刷新」，僅此而已

> **XHR 的使用方式**

1. 創建一個 XHR 對象：const request = new XMLHttpRequest();
2. 監聽請求成功後的狀態變化：if (request.status >= 200 && request.status <= 400) {console.log(request.responseText);}
3. 也可以做失敗處理：request.onerror = () => {alert(errorMessage);}
4. 設置請求參數：request.open ('GET', baseUrl, true);
5. 發送請求：request.send();

> **如果要在送出到 API 的 Request 的  header 上帶上參數也很簡單如下（以 Twitch API token 為例子）**
- request.setRequestHeader('Accept', 'application/vnd.twitchtv.v5+json');
- request.setRequestHeader('Client-ID', '秘密～');

---
## 三、從網頁前端呼叫 API 與在自己電腦上寫程式呼叫的差異

- 要從網頁前端「透過瀏覽器」將 request 呼叫 API 傳送到 server 端時
- 瀏覽器基於安全性的考量會有某些限制與規範（同源政策、CORS等）
- 為了安全性考量瀏覽器會預測阻止你做某些事或加某些東西（例如：增加瀏覽器的版本）
- 而在自己的電腦上則完全沒有這些限制，能夠自由的傳送你想要傳送的東西並得到回應


> 恩，有點像是國中時交的女朋友的爸爸呢......。

---

## 四、什麼是同源政策（Same-origin policy）

只要瀏覽器發的 request 位置 跟 Server 端的 位置是不同源（不同網域 ）， request 就會被瀏覽器擋掉。相同 domain 就是同源，但同網域的 http & https 也是不同源喔。詳細定義請看 MDN 的說明。

值得注意的一點是：「 Request 還是有發出去，而且瀏覽器也確實有收到 Response 」，重點是因為 瀏覽器 才會有同源政策，主要是因為安全性的考量，不把 Response 傳回給你的 JavaScript。

基本上只要你是接別人的 API，都會是不同源的！那該怎麼辦呢？這時候就要輪到我們的 CORS 出場了。

> 參考資料：[交換資料 - 同源政策、CORS、JSONP](https://yakimhsu.com/project/project_w8_CORS_JSONP.html)

---

## 五、如何存取跨網域的資源（CORS）？

如果你想開啟跨來源 HTTP 請求的話，Server 必須在 Response 的 Header 裡面加上 Access-Control-Allow-Origin 如下：

> Access-Control-Allow-Origin: *; （代表允許任何網域存取資源）

但像是 Twitch API 當然不可能允許任何網域都去存取它的數據，因此必須要在 Request 的 Header 中附帶可以認證你身份的資料，此時 Twitch 的 Server 必須在 Response 的 Header 裡面加上 Access-Control-Allow-Origin

1. 提供可認證身份的憑據信息( cookie、HTTP 認證及客戶端 SSL 證明等)
2. 設定 XMLHttpRequest.withCredentials = false （預設為 true 不同域名下取不回 response ）
3. 設定可以存取的域名：res.setHeader(‘Access-Control-Allow-Origin’, 'http://172.19.0.215:3333');

至於 CORS 把請求（Request）分兩種為「簡單請求（simple requests）」和「預檢請求（Preflight Request）」等，此處就先不贅述。

> 參考資料
1. [輕鬆理解 Ajax 與跨來源請求](https://blog.techbridge.cc/2017/05/20/api-ajax-cors-and-jsonp/)
2. [跨來源資源共用 CORS](https://medium.com/neils-blog/cors-%E8%B7%A8%E4%BE%86%E6%BA%90%E8%B3%87%E6%BA%90%E5%85%B1%E7%94%A8cors-191d4bfc4735)
3. [和 CORS 跟 cookie 打交道](https://medium.com/d-d-mag/%E5%92%8C-cors-%E8%B7%9F-cookie-%E6%89%93%E4%BA%A4%E9%81%93-dd420ccc7399)

---

## 六、什麼是 XML, JSON？

> **現在各大網站最常使用的兩大資料格式就是 XML 和 JSON**
- XML 全名為 Extensible Markup Language（可擴展標記語言），其比較近似於 HTML 的「標記語言」，是非常早期就開始被使用的資料格式
- JSON 則是類似於物件的格式，其不但資料結構體積小，傳遞快速方便，且解析上比 XML 方便許多，不會眼花瞭亂

> **JSON 全名為 JavaScript Object Notation（JavaScript 物件表示法）**
- 是基於 JavaScript 的物件產生出來的資料格式
- 也是為為現代最普遍、常用的資料格式
- 其廣泛應用在各程式語言，由於前後端分離的興起需要用 JavaScript 處理的後端傳來的資料
- 而最常使用的交換技術「AJAX」就是使用 JSON 作為交換資料的格式。

> **JSON 常用函式**
- JSON.prase()
    - 將 JSON 格式字串轉成物件
- JSON.stringify()
    - 將物件轉成 JSON 格式字串

> 參考資料
1. [基本資料格式: XML 和 JSON](https://ithelp.ithome.com.tw/articles/10203632)
2. [資料格式: JSON、API Method 風格: RESTful](https://miahsuwork.medium.com/%E7%AC%AC%E5%85%AD%E9%80%B1-api-%E5%9F%BA%E7%A4%8E-json-restful-curl-%E6%8C%87%E4%BB%A4-28670813764e)

---

## 七、什麼是 JSONP 及其原理

> **JSONP（JSON with Padding）是資料格式 JSON 的一種「使用模式」，可以讓網頁從別的網域要資料**
- 透過 JSONP （JSON with Padding）傳輸的，將 JSON 資料填入 Padding （Padding就是要呼叫的函式）
- 利用 src 不受同源限制的特性，透過 script 標籤裡的 src 元素繞過同源政策去取得引用外部網站的資源（例如 jQuery）
- 實務上在操作 JSONP 的時候，Server 通常會提供一個 callback 的參數讓 client 端帶過去
- 但 JSONP 的缺點就是你要帶的那些參數「 永遠都只能用附加在網址上的方式（GET）帶過去，沒辦法用 POST 」
- 很重要的一點是，要使用 JSONP 傳送資料，也要 Server 端有提供 JSONP 的方法（ 意指用 callback function 包起來 ）才行，不然回傳的 Response 就只是字串而已，沒有辦法取得資料


像是在 html 中有些標籤像是 `<img>`、`<script>`......等，在 src 的部分可以從 https:// …. 載入圖片或是 js 檔案，這不就是運用 JSONP 跨來源取得資料，目前已經很少人用，因為比較麻煩，要帶的那些參數永遠都只能用附加在網址上的方式（GET）帶過去，沒辦法用 POST。

不過有個有趣的小運用，就是在追蹤電子信開信率時會在信中塞入一張透明的圖片，一旦使用者點擊圖片就發出一個 Request 傳到 Server 端你就能統計有多少人打開信件了。

---
## 八、結論

> 比起串接全世界的 API，我更想串接的是妳的心。