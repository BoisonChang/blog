---
title: "百日轉職前端工程師：第十二週前後端整合復盤 《DAY 18》"
slug: Transfer-FrontEnd-100-18
date: 2020-11-13T21:47:21+08:00
draft: false
tags: ["FrontEnd", "Transfer"]
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

大家好，這是百日轉職前端工程師的 Day18，也是 11/13（五），這週的主題會講的該要如何將前後端（分開處理後再）整合，如果有做好這個步驟，那理論上就算後端 Server 掛掉頁面還是會有資料，只是某些部分是空白的而已，而將這個功能做到極致則會有點類似 SPA（Single Page Application）單頁應用一樣，除此之外本週也會介紹 Boostrap 和 jQuery 兩樣好用的工具。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

## 一、什麼是 SPA？

> **SPA（Single Page Application）單頁應用**

- 他能夠讓你雖然只有一個 index.html 檔案，用起來卻像一個 App
- 其能夠減少重新載入整個頁面的次數，改用 AJAX 局部渲染頁面內容
- 簡單來說原本資料是在後端就要做處理，對於瀏覽器來說這叫做 Server-side render，而讓前後端分開處理，則是前端只單純透過 API 向後端拿取資料再透過 JavaScript render 出來部分頁面，稱為 Client-side render
- 當你把這種東西發揮到極致稱為 SPA，會覺得很像在寫一個 App 一樣，因為用起來就跟你在用 Native App 差不多

### 1. Server-side 和 Client-Side Render 的兩種資料處理方式

#### PHP：連資料跟介面都已經處理好輸出

> 又稱之為 Server-side render（SSR）

1. 把資料拿出來
2. 把資料跟 HTML (UI) 介面結合在一起
3. 回傳 HTML

#### API：只是單純拿到資料（只有資料）

> 又稱之為 Client-side render（CSR）

1. 把資料拿出來
2. 變成某種格式（JSON）
3. 回傳純資料

---

## 二、怎麼樣用 PHP 自己寫出 API？

簡單來說在 PHP 的最後段，將你想要取得的資料存在陣列 $json 中，再用 PHP 內建的函示 json_encode() 將其轉為 JSON 字串，後加入到 $response 送出。

```
$json = array("comments" => $comments);
$response = json_encode($json);
```

> 測試 API 時可以使用兩個工具，更方便的即時檢視 echo 和回傳結果，以便確定 API 寫的無誤

1. Postman
2. Insomnia

---

## 三、如何在前端與自己開的 API 串接？

- 簡單來說在前端的 JavaScript 中用 new XMLHttpRequest 的方式去向 api_comment_api 發送一個非同步的請求（Request）
- 並且取得其回應 （response） 值
- 然後用內建的函式 JSON.parse 將 JSON 字串轉化為物件
- 並將整串非同步的請求放入一個 function getComments 中，隨時等待呼叫

```
function getComments(cb) {
  var request = new XMLHttpRequest();
  request.open('GET', 'api_comment.php', true);
  request.onload = function() {
    if(this.status >= 200 && this.status < 400) {
      var resp = this.response;
      var json = JSON.parse(resp)
      var comments = json.comments
     }
  };
  request.send();
}
```

> 參考資料

1. [XMLHttpRequest — JavaScript 發送 HTTP 請求](https://notfalse.net/29/xmlhttprequest)
2. [如何在 PHP 中將陣列（array）轉成 JSON 格式](https://pjchender.blogspot.com/2016/04/phparrayjson-jsonencodeutf8.html)

---

## 四、Server 與 Client render 的差別？

- 在實際應用上，前後端拆成兩個專案也有一些好處，像是利於團隊分工
- 在這個思維下，後端用來實作商業邏輯及橋接資料庫，前端用來傳接資料及輸出頁面
- 前端的程式設計師不會直接動到後端的專案，反之亦然。
- 將專案拆開後，前端只要用靜態網頁主機就可以架起來，不需和後端共用同一個伺服器
- 另外一個好處是將資料和輸出解耦，同樣的資料可以搭配不同的媒介，像是同一個後端，可用來橋接網頁、行動裝置、桌機等不同環境。

> **但看似美好的 SPA 也不是沒有任何缺陷**

- 由於需要仰賴前端下載大量 JavaScript 檔案，隨後才計算、渲染畫面，造成第一畫面需要較長的反應時間，在性能較差的行動裝置上更是如此
- 且如果使用者操作太快，前端就不能保證每次都能得到正確的 Server 資料（點進電影 A → 快速按上一頁 → 快速點進電影 B → 畫面顯示電影 A 資料）
- 除此之外 SEO（Search Engine Optimization） 效果也會變差
  - 因為是由前端的 JavaScript 動態產生內容，因此如果你對 SPA 的網站按下右鍵 -> 檢視原始碼，只會看到空蕩蕩的一片，搜尋引擎無法讀取內容做判斷
- 視情況去使用，其比較廣泛運用於 Gmail、Twitch，和 Spotify......等場景，讓使用者能夠在同一個頁面部分刷新，但又不中止整個網頁的運作，也就不會在讀取新資料時出現惱人的一片白，在特定場景上大幅度優化了整體的使用者體驗。

> **SSR+CSR：解決 SPA 在 SEO 上效果不佳的問題**

- 既然問題出在「第一次渲染」，那我們只要在第一次渲染的時候把該輸出的資料都輸出就好啦
- 對使用者來說還是一個 SPA，差別在於使用者接收到 HTML 的時候，就已經有完整的資料了
- 而後續的操作還是由 JavaScript 來處理，依舊能保持 SPA 的優點
- 簡單來說就是第一個頁面是由 SSR（Server-side render），之後的頁面還是保持 CSR （Client-side render）

> 參考資料

1. [跟著小明一起搞懂技術名詞：MVC、SPA 與 SSR](https://hulitw.medium.com/introduction-mvc-spa-and-ssr-545c941669e9)
2. [前後端分離與 SPA](https://blog.techbridge.cc/2017/09/16/frontend-backend-mvc/)
3. [為什麼網站要做成 SPA？SSR 的優點是什麼？](https://ithelp.ithome.com.tw/articles/10224772)

---

## 五、jQuery 是做什麼的？

> **jQuery 是一個 JavaScript 函式庫（library），其初衷是用來簡化 HTML 與 JavaScript 之間的溝通與操作**

1. 將 JS 檔案變小（ex：省空格）
2. 壓縮檔案
3. 跨瀏覽器！

其中又以第三點是最為關鍵，在早期瀏覽器百家爭鳴的年代，你可能要寫五種寫法才能在五個不同的瀏覽器上面能運行，但 jQuery 跨瀏覽器的特性讓工程師只要寫一種寫法，就能在不同的瀏覽器上運行，且其可跟 JavaScript 混用。

引入方式:

```
<scrpit src="https://code.jquery.com/jquery-3.5.1.js"></script>
```

若是要讓其能夠不會由於程式碼順序產生 bug，需要將所有程式碼包覆在 `$(document).ready(function() { })` 中，其作用為讓網頁下載完成後立即執行包覆的程式碼，也就是說當頁面內的 DOM 都準備好開始讀取就會執行 callback 內的 function。

其中的 `$()` 符號就是 jQuery 常用的選擇器（$ = jQuery），善用選擇器可以快速找到網頁當中的物件。

> **以下介紹幾種常見的用法，其他用法可至 You might not need jQuery 有非常詳盡的對照資料可查詢**

### 1. 選取 '$(selector)' 元素，並指派它進行 '.action()' 事件

`$( "button.continue" ).html( "Next Step..." )`

### 2. 修改一個表單元素的 value 值

`$("Element").val(value)`

### 3. class 的新增

`$("#test").addClass("aaa")`

### 4. click 事件點選 btn-finished 元素

`$('.list-group-body').on('click', '.btn-finished', e => {}`

### 5. 切換: toggle

`$('#mydiv').toggle(function(){}, function(){})`

### 6. ajax

`$.ajax({'type': 'POST', 'url': '', 'data': { todolists }}).done(function(data) { ... }`

### 7. 取得 HTML

`$("#htmDoc").load("test.html")`

### 8. 淡入淡出效果

`$('.box').fadeOut(500)`

### 9. 新增元素到最下面/最上面

- `append()`
- `prepend()`

> 參考資料

1. [jQuery 概述](https://www.notion.so/jQuery-4ff9dbbd5de74daab687318f15ac2ab2)
2. [前後端整合 - 學習 jQuery 與 Bootstrap](https://hackmd.io/KB2wVAOuS9mVz8Xlx5DT2A?view#jQuery-%E6%98%AF%E5%81%9A%E4%BB%80%E9%BA%BC%E7%9A%84--%E8%88%87-vanilla-js-%E7%9A%84%E5%B7%AE%E5%88%A5-)
3. [jQuery 是什麼，它跟 JavaScript 有什麼關係？它又有什麼能耐呢？](https://progressbar.tw/posts/6)

---

## 六、jQuery 與 vanilla js 的差別？

Vanilla JS 是世界上最輕量的 JavaScript 框架（沒有一個）這句話讀三遍。如果還是不信的話，再把下面這句話讀三遍：「無論過去，現在還是未來，Vanilla JS 都是世界上最輕量的 JavaScript 框架（沒有一個）！」它輕量到甚至沒有任何東西，什麼？你說中文看不懂？那請看看原文可能比較精準，或自行上網查詢，在網路上有很多關於 vanilla js 的精彩討論串。

> VanillaJS is a term for library/framework free javascript. Its sometimes ironically referred to as a library, as a joke for people who could be seen as mindlessly using different frameworks, especially jQuery. Some people have gone so far to release this library, usually with an empty or comment-only js file.

---

## 七、什麼是 Bootstrap？ 原理及應用？

簡單來說 Bootstrap 就是由一堆 HTML、CSS，和 JavaScript 所組成的網站樣板（templates）/ 框架，核心的設計目標是達成 RWD 響應式與行動優先，也就是讓你的網站排版可以自動適應螢幕大小。

基本上你想的到的網站元件舉凡字體排印、表單、按鈕、導航......都有，是前端工程師神助攻的小幫手。網路上有很多公開資源可以下載，可參考官網以及 Bootswatch（提供各種經過調整顏色的 Bootstrap 版本，可以套用在已有的 Bootstrap 網站上）

> **應用 grid 網格系統在 Boostrap 上 **

可以直接在 class 裡面給予不同的 .col-，的來宣告其在不同版位下的佔據網格的比例如：

```
<div class="col-12 col-md-4"></div>
```

---

> 參考資料

1. [Bootstrap 介紹 + 套版](https://tw.coderbridge.com/series/4e63287d623d41fb93507f5959accab4/posts/272300398e144b43ba350bd4dc46444b)
2. [Bootstrap 初學介紹 #靜態篇](https://weilihmen.medium.com/bootstrap-%E5%88%9D%E5%AD%B8%E4%BB%8B%E7%B4%B9-%E9%9D%9C%E6%85%8B%E7%AF%87-f20500235b33)

## 八、結論

> 『分離是為了更好的相遇（整合）。』
