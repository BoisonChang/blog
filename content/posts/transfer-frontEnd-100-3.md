---
title: "百日轉職前端工程師：網頁的普通常識 HTML《DAY 3》"
slug:  Transfer-FrontEnd-100-3
date: 2020-06-12T00:07:21+08:00
draft: false
tags: ["FrontEnd", "transfer"]
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

大家好，這是百日轉職前端工程師的 Day3，也是 6/12（五），今天來聊聊網頁的普通常識。

因為前幾天還都陸續在上 Codeacdemy 的章節，昨天在自己電腦的主機端要寫 HTML 檔案（網頁其實就是由 HTML 文件檔案構成的）的時候，發現我還是對於裡面的一些「元素（Element）」和「標籤（tags）」分不太清楚，覺得種類有夠多而且很煩，根本不太知道何時要用什麼，所以嘗試一次寫篇文章稍微釐清一下關於 div 的部分，以免夜長夢多。如有寫錯，拜託指教！ 

---

## 一、div 標籤到底是什麼？何時用？

div 標籤基本上就是用來包裹「文章區塊」，我一度很懷疑都已經有 h1,h2,h3 之類幹嘛還要有 div，後來發現他的一大功用是可能用來插入 css 語法去美化部分區塊，因為你如果在 h1 加上 div 就會一次更動到整篇文章，但如果你只是在你想改的段落插入<div>你想改的段落</div> ，然後你再用 <div class="bold"> 或者用 <div id="bold"> 就可以單獨只改這塊，所以我理解上他就是一個隨時可以包裹原本段落來做一些部分微調的「文章區塊標籤」。

---

## 二、語意化 HTML (Semantic HTML)

基本上就是改版到 HTML5 以後，就會將過去的 div 標籤改成更能了解文章區塊有語意的標籤，像是 header、article、footer，雖然他們本質上功能是一樣的可是對於觀看者在語意判讀上比較好懂，甚至對於 Google 在爬取網頁時的辨識也有幫助。

啊過去只有 div 的時候如果想要把各區塊加上風格，還要先命名 class 或者 id 像是這樣很麻煩：<div class="menu">、<div class="nav">、<div class="navication"> 

> **所以 div 跟 section, article, figure 等標籤到底有啥差**

就如前述，div 是無意義，而 HTML5 以後加入基本功能差不多（用來包裹文章區塊）的標籤，但是可以表達語意的標籤 section, article, figure 去取代千篇一律無意義的 div。我又想到一個問題，能否完全不用這些東西，單純就只用 h1, h2, h3......我思考以後好像在兩種情況下可以：
1. 若你不需要部分去調整部分文章區塊的「風格（style）」
2. 你的文章沒有分區塊，就是一整坨一頁到底。

---

## 三、Semantic HTML 中各大標籤

### `<header>`

> **標題元素：預設為區塊元素，為文章或區塊定義標題。**
- 一個頁面中可以有多個 header
- `<header>` 是一容器，但裡面裝的東西應該只有區塊的標題或者摘要
- 另外 w3.org 中有提到  `<header>` 不能放在 `<footer>`、`<address>` 或另一個  `<header>` 裡面


### `<nav>`
- 導覽列
- 裡面裝的東西應該只有主要的 navigation links，用來連結到網站其他頁面，或者連結到網站外的網頁
- 不要把各種 link 都丟到裡面
- `<nav>` 不可以放到 `<address>` 裡面

### `<main>`
- 網頁的主要內容，一頁 HTML 只能出現一次。

### `<section>`
- 文件中的一個群組或區塊
- 一般來說，section 裡面的應該是有意義且附帶標題的一段內容，裡面都會包含 title 或 heading
- 不要把 當作 div 來使用，他們的使用目的不同
- `<section>`不可以放到 `<address>`  裡面

### `<article>`
- article 是獨立的區塊包含完整的內容，也必須帶有 heading
- article 跟 section 的區分是，article 有更高的獨立性及完整性
- article 是可以獨立存在、具有完整內容、可獨立於網站其他部分的區塊；而section 雖然也具有獨立表達內容的能力，但是對外層有一定的相依性
- 例如：雜誌、部落格的文章、報紙文章等

### `<aside>`
- aside 元素定義了除了內容之外的其他內容，aside 的內容應該與周圍的內容有相關性
- 例如：側邊欄

### `<figure>`
- figure 是一個有完整內容的區塊，是主要內容的一部分，可以任意移動位置而不影響整體內容的表達
- 例如：圖片與標題可以組合在 figure 元素裡

### `<footer>`
- footer 代表一個區塊的結尾訊息。footer內通常會包含作者、版權等資訊

> 資料來源: [小事之 HTML 語意化標籤 上篇](https://ithelp.ithome.com.tw/articles/10195356)
---

## 四、總結：HTML 就由這些標籤和元素構成

我個人感覺基本上 HTML 就由這些 「元素（Element）」和「元素（tags）」組成，一開始看覺得超複雜的，整理完梳理一遍好像簡單了一點，仔細想想其實這些東西也沒那麼複雜，用習慣以後好像也沒什麼變化了，基本上就跟 Word 文件差異不大吧。