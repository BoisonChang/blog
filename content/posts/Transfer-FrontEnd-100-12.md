---
title: "百日轉職前端工程師：第六週前端基礎 HTML《DAY 12》"
slug: Transfer-FrontEnd-100-12
date: 2020-09-02T20:32:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "HTML"]
author: "Boison"

---

大家好，這是百日轉職前端工程師的 Day12，也是 9/02（三）。今天開始要來復盤之前學過練習過前端基礎下，這塊是介紹 HTML 和 CSS 處理網頁最基礎的切版問題，但第一次接觸也帶給了我不小的挫折，並且發現其中真的要玩的話水可是很深的呢！但熟悉之後這塊也帶給了我不小的成就感，未來有機會挺希望繼續深入這塊的。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## ㄧ、HTML 是在做什麼的？

- HTML 全名為 Hypertext Markup Language（超文本標記語言）是一種用於建立網頁的標準標記語言
- 基本上就是一份文件，但是這份文件卻是瀏覽器認識的文件，可以將他讀取為我們平常看到五彩繽紛有大有小有圖片的網頁頁面

> **HTML  內的元素 (element)  有兩個重要的基本觀念**

- 巢狀結構
  - 文件內的格式也就是所謂的元素 (element) ／區塊，都是一層一層的「標籤」包覆的
  - 比方說「主文區塊」中包覆「標題區塊」，標題區塊中又包覆「標題圖片區塊」
  - 基本上打開右鍵，選取檢視原始碼會出現 `<body><div><a/></div></body>` ，也就是「標籤」
  - 而其中間可以寫入「文字」，整體稱之為一個「區塊」，而且排版方式會是巢狀的
- 屬性 (attribute)
  - 運用 class 等為各個區塊加上屬性 `<div class="title"><div>`
  - 在 CSS 的介面中撰寫這個區塊的屬性內的視覺呈現 .title { background: black }
  - 所有的區塊都能夠加上 id 和 class 的屬性
  - 但一個  element  可以有無數個 class 可是只能有一個 id

---

## 二、如何使用有語意的（semantic）標籤？

改版到 HTML5 以後，就會將過去的 div 標籤改成更能了解文章區塊有語意的標籤，像是 header、article、footer，雖然他們本質上功能是一樣的可是對於觀看者在語意判讀上比較好懂，甚至對於 Google 在爬取網頁時的辨識也有幫助。

過去只有 div 的時候如果想要把各區塊加上風格，還要先命名 class 或者 id 像是這樣很麻煩：

```
 <div class="menu"></div>
 <div class="nav"></div>
 <div class="navication"></div>
```

### 所以 div 跟 section, article, figure 等標籤到底有啥差？

如前述 div 是無意義，而 HTML5 以後加入基本功能差不多（用來包裹文章區塊）的標籤，但是可以表達語意的標籤 section, article, figure 去取代千篇一律無意義的 div。

> **語意標籤 (semantic) 使用時機**

- `<header>`：網頁的標頭，通常放置網站標題
- `<nav>`：網頁的選單或導覽列
- `<main>`：網頁的主要內容
- `<aside>`：網頁的側欄、附加內容
- `<article>`：一篇文章內容
- `<section>`：自訂的區塊
- `<footer>`：網頁的頁尾
- `<mark>`：強調一小塊內容
- `<time>`：顯示日期時間

---

## 三、基本 SEO 的概念

> **關於 SEO 是什麼？又該要怎麼做最好？**

我從內容行銷/數位行銷的角度已寫過幾大非常詳細的文章如：「數據導向的 SEO 網頁排名優化策略」和 「SEO 「關鍵字行銷」的三大隱藏技巧」此處會以前端工程師的角度來談談 SEO 這件事。

> **SEO 全名為 Search Engine Optimization（搜尋引擎最佳化）**

簡單來說搜尋引擎諸如 Google、Yahoo、Bing......等(主要就是 Google 啦，其他來打醬油的)，搜尋引擎會根據讀取到的網頁內容去將其與使用者的搜尋連結，因此你網頁寫得越清楚，搜尋引擎讀的越容易越清楚，那合理來說你的內容就更容易被需要的使用者搜尋到。

然而，搜尋引擎看到的網頁並非我們使用者看到的網頁，它所看到的是一串密密麻麻的 HTML 純文字原始碼。因此你要想辦法用 HTML 的方式，讓搜尋引擎更容易讀懂你的網頁內容，其實這部份的水很深，而且規則也時常更改，所以最好的方式是去看 Google SEO 方面的官方文件。

> **但以下就略為整理我的認知，要如何提升 SEO 的分數呢？**

### 1. 盡可能使用上述提到的語意化標籤（semantic）

除了上述提到基本的區分區塊的語意化標籤，還有各種進階的標籤（像是 Title、meta、img-alt......等）和屬性的設置來幫助瀏覽器更精準讀取內容，也能設定好不同語系國家/瀏覽器使用者看到的頁面。以下列舉一些供參考：

- 網頁標題：`<title>`
- meta：存放在 `<head>` 之中
  - 網站描述：`<meta name="descriptions" content="" />`
  - 不要檢索 (noindex): `<meta name="robots" content="noindex, follow" />`
  - 只檢索該頁，不檢索該頁裡的鏈結 (nofollow): `<meta name="robots" content="index, nofollow" />`
  - 作者：`<meta name="author" content="">`
  - 網頁編碼：`<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />`
  - 網頁路徑：`<meta name="URL" content="https://www.tripxbook.com">`
  - 網頁相容模式：`<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7" />`
  - 宣告自然語言（中文，英文…）
- 針對 FB 的 tag
- 針對 Twitch 的 tag
- 圖片

### 2. 提升網頁速度（包含兼顧提升手機版 RWD 的載入速度）

### 3. 在網頁的 HTML 中加入結構化資料（schema）

結構化資料（schema）是一種協助搜尋引擎理解網站內容的系統，以標準化的格式，針對網站所提供的訊息、內容特性進行分類，實際增加結構話資料的方式有 JSON-LD、微資料，和 RDFa。

#### 3-1. JSON-LD

全名為 JavaScript Object Notation for Linked Data，即是 JSON 格式的結構化資料，跟 og 是類似的東西，以固定的格式去說明網頁內容，可以幫助機器去理解網頁內容，就能針對不同類型的網站作出對應的搜尋結果的呈現。

我自己覺得最酷的就是你可以用 JSON-LD 操控 Google 搜尋顯示出的頁面，還能夠自己畫出使用者評價五顆星，看你想要幾顆星都可以！（海濤法師：假的！我眼睛業障重。）

#### 3-2. 微資料

就是在 HTML 上直接加入結構化屬性在標籤中，如:itemscope、itemtype、itemprop

#### 3-3. RDFa

### 4. 透過 robots.txt 告訴網站上的哪些頁面需要檢索，哪些特定頁面禁止爬取。(通常放在根目錄。)

### 5. 提交 Sitemap 增加網站被完整收錄的可能

Sitemap 就像是網站的目錄，將站內所有頁面依分類呈現，而 SEO 需要用到的 Sitemap 常見的檔案格式為 sitemap.xml 檔。可以使用 Google 的  Search Console  提交 XML Sitemap，詳細教學可見 sitemap.xml 網站地圖提交教學。

### 6. 網站上方的麵包屑導覽列 (Breadcrumb)

做好網頁分層設計的導覽，從首頁開始依「大分類 > 中分類 > 小分類」這樣階層式的順序記載下來最為重要。若是網站 (ex: 電商) 結構很複雜更要分層明確，此需跟結構化資料（schema）搭配，麵包屑導覽列為表面，結構化資料（schema）則為 HTML 文件內容。

> 參考資料：

1. [[第七週]基礎 SEO 標籤 — meta、og、JSON-LD](https://miahsuwork.medium.com/%E7%AC%AC%E4%B8%83%E9%80%B1-%E5%9F%BA%E7%A4%8E-seo-%E6%A8%99%E7%B1%A4-meta-og-json-ld-a0a86d2a39dc)
2. [前端網站優化-SEO 篇](https://zx2515296964.medium.com/seo-%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%B8%AB%E8%99%95%E7%90%86seo%E5%BF%83%E5%BE%97-107ac28f4aa5)

---

## 四、總結

> 『切版水很深，同志待努力。』
