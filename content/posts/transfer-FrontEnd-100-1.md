---
title: "百日轉職前端工程師：前端工程師知識地圖 《Day 1》"
slug: Transfer FE 100 1
date: 2021-06-08 T11:36:00+08:00
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


開始先弄懂一下前端工程師可能要懂的知識點，之後有看到什麼查什麼，昨日阿斌密我說他也有參加過胡立的計畫，問了阿斌請他幫我講解一下六角學院的這張圖，以下自己補充完紀錄之，以下內容為個人用麻瓜的語言理解後的翻譯，可能有誤歡迎留言指點。


※ 如果你想要學習如何在百日內「高效學習」上路實戰，非常推薦閱讀《超速學習》一書。

---
![](https://firebasestorage.googleapis.com/v0/b/hexschool-api.appspot.com/o/blog%2F2018%2F04%2F%E6%8A%80%E8%83%BD%E6%A8%B9_web-1.png?alt=media&token=f2e4391d-ff1a-4d67-82b2-33e8851cff64)

## 一、必備技能

### 1. Git
類似 RPG 的存檔功能，可以隨時回到上個存檔點。

> **比較**
- Git:  是一種版本控制軟體，用於控制版本並與團隊共同協作同一專案。
- GitHub: 是一個商業網站，GitHub 的本體是一個 Git 伺服器。
    - 但這個網站上的應用程式讓大家可以透過 Web 介面來操作一些原本需要複雜的 Git 指令才能做到的事。
    - 但事實上類似這樣的商業網站還有很多，其中另一最常被使用的則是 GitLab，功能基本大致上與 GitHub 相同，但其各有其主打的功能。
    - GitLab 除了 倉庫管理 和版本控制，它還提供 Wiki 託管和錯誤跟踪系統。 

### 2. Terminal
不透過圖形化介面，直接用這個工具寫程式碼向電腦下指令的工具。
在 OS 系統它就叫做 Terminal，而在 Windows 系統中可以透過命令提示字元 (cmd.exe) 開啟 Terminal。

> **在不同的作業系統下大致上可以照以下方式叫出 Terminal**
- Windows: 開始 → 所有程式 → 附屬應用程式 → 命令提示字元 → 輸入 powershell → 終端機 (Terminal)
- Mac OS X: 應用程式 → 工具程式 → 終端機 (Terminal)
- Linux:  應用程式 → 附屬應用程式 → 終端機 (Terminal) 

---

## 二、CSS 預處理

目前為止，在眾多優秀的 CSS 預處理器語言中就屬 Sass/Scss、LESS 和 Stylus 最優秀使用的人也最多。

基本上就是把 CSS 的語法在比較方便的環境中編撰（預先處理，可能覺得原始的程式碼編寫會有諸多不方便或者不直覺），但是這些程式碼不能直接執行，打包之前還要做一些處理才能跑。

Sass 是一種基於 CSS 所產生的高階語言，他將一般程式語言所擁有邏輯概念和變數帶入 CSS 樣式表中，讓我們在撰寫網站的樣式時，可以用更快速、更方便的方式撰寫，而這是網路上可以直接試著寫 Sass 轉成 CSS 以及 Scss 轉成 CSS 的線上編譯器。

> **Sass 和 Scss 的差別**
- 基本上是同樣的東西，差別只差在撰寫的方式不一樣
- Sass 的撰寫方式是用 Ruby 的撰寫方式，沒有分號、括號那些東西，都是用分段的方式來表示
- Scss 而是在 CSS3 推出，撰寫方式與 CSS 相同，比起 Sass，對於寫習慣的 CSS 的人，Scss 相對會比較好上手

> **HTML、CSS、JS  的預處理**
- HTML、CSS、JS 都可以做模組化，而「預處理器」常是模組化的手段之一
- 例如 pug 是 HTML 的預處理、Sass/Scss 是 CSS 的預處理、Babel 是 JS 的預處理

> 資料來源: [一起來用 Sass+Compass 吧 ](https://www.hellosanta.com.tw/knowledge/category-13/post-82)

---
## 三、CSS 模組化概念

### 1. SMACSS 
- SMACSS (Scalable and Modular Architecture for CSS) 偏向整個專案架構的分類及模組化你的 CSS
- 顧名思義是可模組化且具結構分類有命名規則的方式，其中的精神就是容易擴展及模組化
- 基本上的結構分為 Base、Layout、Module、State、Theme

### 2. OOCSS 
- OOCSS (Object Oriented CSS) 是使用物件導向方式管理 CSS 的 Style
- 其有兩項基本的原則：(1). 結構與樣式分離，(2). 容器與內容分離
- 結構與樣式分離要避免 .content > div > span{ ... ) 的 CSS 寫法
- 容器與內容分離要避免 .left-bar .list-group li span { ... } 的 CSS 寫法

### 3. BEM 
- BEM (Block, Element and Modifier) 是關於 Class 的命名方式
- 強調完全使用 Class Selector 並且以功能導向命名
- 強調每個功能區塊 Block 裡面的 Element 分成一塊，命名上配合此功能去延伸命名
- 每一個 Block 都會有自己的資料夾路徑，並且裡面有這個 Block 的專屬 CSS 及 JS
- 裡面還會有這個 Block 下方 Element 的資料夾，並且開頭為 __ 

> 資料來源 
1. [談談CSS模組化](https://medium.com/@zehung860486/css-notes-css%E6%A8%A1%E7%B5%84%E5%8C%96-3bb03e72523e)
2. [Scalable CSS - 介紹OOCSS/SMACSS/BEM](http://sj82516-blog.logdown.com/posts/1077348/finish-css-intro-oocss-smacss-bem)

---
## 四、CSS 技巧  & CSS 專精

不需要使用 CSS 的 FrameWork，依然可以幫助快速排好版型，那就是——FlexBox。最近這一年多以來相當盛行的一個排版神器。可以透過幾行簡單的語法，就可以將我們想要呈現的內容，存放在我們想要的區塊內。

> **好用的工具網站**
- Bootstrap（最常被使用，更新也能勤的 Component-First CSS framework）
- Material UI （Component-First CSS framework）
- antdesign （Component-First CSS framework）
- Tailwind（近年崛起，更能客製化切版需求的 Utility-First CSS  framework）

> **Component-First CSS  和  Utility-First CSS 比較**
- Component-First CSS 
    - 已有現成的功能組件像是 Button，Form 等幫你寫好
    - 由於已經寫好了整套若是想要根據需求調整，會比較麻煩而且容易遇到衝突
- Utility-First CSS 
    - 只針對 CSS 樣式去做處理
    - 讓你保留原生 CSS 切版的彈性，又能夠直接使用定義好的 class 渲染出樣式


--- 
## 五、基礎 DOM 操作

### 1. DOM

> 檔案物件模型（Document Object Model，簡稱 DOM），是 W3C 組織推薦的處理可擴充套件置標語言的標準程式介面。

根據 W3C DOM規範（http://www.w3.org/DOM/），DOM 是一種與瀏覽器，平台，語言的接口，使得你可以訪問頁面其他的標準組件。簡單理解， DOM 解決了 Netscaped 的 Javascript 和 Microsoft 的 Jscript 之間的衝突，給予 web 設計師和開發者一個標準的方法，讓他們來訪問他們站點中的數據、腳本和表現層對像。

> 文件物件模型（Document Object Model，DOM）是給 HTML 與 XML 文件使用的一組 API。

它提供了文件的結構表述（representation），讓你可以更動其中的內容及可見物，其本質是建立網頁與 Script 或程式語言溝通的橋樑。

所有網頁設計師可操作及建立文件的屬性、方法及事件都以物件來展現（例如，document 就代表「文件本身」這個物件，table 物件則代表 HTML 的表格物件等等）。這些物件可以由當今大多數的瀏覽器以 Script 來取用。

DOM 最常被用以與 JavaScript 溝通，也就是說雖然程式以 JavaScript 寫成，但使用 DOM 來存取頁面及其元素。無論如何，DOM 本身是設計為一種獨立的程式語言，以一致的 API 存取文件的結構表述；是以雖然焦點放在 JavaScript 上，但 DOM 其實可以與任何程式語言共同運作。

簡單來說，在DOM的標準下，一份文件中所有的標籤定義，包括文字，都是一個物件。

> 這些物件以文件定義的結構，形成了一個樹狀結構。例如：

```
<html>
    <head>
        <title>首頁</title>
    </head>
    <body>
        <h1>Hello!World!</h1>
        <a href="practice/index.html">學習筆記</a>
    </body>
</html>
```

> 資料來源
1. [甚麼是 DOM](https://chenmike.pixnet.net/blog/post/25948952)
2. [W3C DOM 簡介](https://openhome.cc/Gossip/JavaScript/W3CDOM.html)

### 2. jQuery

jQuery 是一個快速又簡潔的 JavaScript 程式庫 (library)，簡化了在 HTML 文件裡面尋找 DOM 物件、處理事件、製作動畫、和處理 Ajax 互動的過程。

與框架並行使用，早期框架的工作是 jQuery 在做的，如果同個事件綁定多個處理器，甚至連執行先後順序也不一樣。而透過 jQuery 處理事件只需要一個 $(…).bind() 或 $(…).on() 就足夠，除了解決跨瀏覽器的問題之外，也能解決對 DOM 控制的問題。

---

## 六、其他

其它是 UI 要懂的，但有空也可以懂。

---

## 七、JavaScript 技巧

ES 就是 JavaScript 的版本，基本上 ES6, ES7 是比較大改變，所以要熟悉！最新已經出到 E11，但是不是所有公司的環境都更新到這，不要只會用 E11 不然去到不能用的公司等於武功廢一大半， ES11 很多好用功能都不能直接用。

---
![](https://i.pinimg.com/736x/61/85/50/618550199389f841a1b51392be0339bf.jpg)

## 八、JavaScript 預處理

JavaScript 預處理，就是為了更好寫程式之類開發的一個環境，但不能直接實際執行，打包之前要做一些轉換。

因為 JavaScript 沒有型別，有些人想要變成強型別比較精準好用，所以開發了 TypeScript，TypeScript 在還沒執行前若程式碼有錯就會跳 Error 讓你知道。（TypeScript 是微軟開發的）。

至於 Babel 則是可以把新的語法 (ES11, ES10) 變成舊的語法，讓有些使用者即使是舊的 Chrome 也可以跑網頁。

## 九、JavaScript 框架

> JavaScript 框架一組能輕鬆生成跨瀏覽器兼容的 JavaScript 代碼的工具和函數。

早期框架的工作是 jQuery 在做的，如果同個事件綁定多個處理器，甚至連執行先後順序也不一樣。而透過 jQuery 處理事件只需要一個 $(…).bind() 或 $(…).on() 就足夠，除了解決跨瀏覽器的問題之外，也能解決對 DOM 控制的問題。

但是 jQuery 還是自由度太高，使用上不嚴謹，所以當專案越來越大時團隊運作很不方便，於是框架誕生了。

> **主流前端框架**
1. React（Facebook 開發的框架）
2. Angular（Google 開發的框架）
3. Vue （中國開發的框架，但很多人在用好用）

> **React 和 Web Component 是為了解決不同的問題所建立的**
- Web Component 為了可重複使用的 component 提供了強大的封裝
- React 提供了一個宣告式函式庫 (library)，使 DOM 與你的資料保持同步。
- 這兩個目標是相輔相成的，你可以自由地在 Web Component 中使用 React，或在 React 中使用 Web Component，或兩者都是。

React 以一種維持狀態 (state) 的方式，保持資料的同步。

實際上前端三大框架 Angular、React 和 Vue 的設計理念在這塊都有些像，基本上已經跳脫過去 JS 直接去操控網頁的 DOM 的範圍，所以跟 jQuery/JavaScript 的使用方式基本上已經不太能去比較，其他的由於理解不深，會再花完整篇幅闡述。

---
## 十、任務管理工具

將一些處理「動作」寫好，以後就能自動化去跑完。例如像是 gulp ，可以寫好執行腳本（第一步執行 Babel，第二步壓縮圖片.....）

---
## 十一、套件管理工具

我的理解任務管理工具，是能裝一些套件。就像是 Window10 不是裝任何軟體都要到內建的一個區域，那邊可以搜尋到很多套件，統一在那邊下載：
- NPM （Google開發的，比較老的套件管理工具）
- YARN （Facebook 開發的）

---

## 十二、API
基本上後端工程師從資料庫取出資料以後，前端工程師跟後端工程師會約定好一套 API，以後前端工程師就透過這組 API 去取得相對應的資料。

---

## 十三、測試

就是開始跑之前要看看成果長怎樣，有沒有 Bug，就透過測試環境。

---

## 十四、其他
### 1. Firebase - 前端要自幹的工具

Firebase 是一個同時支援 Android、iOS 及網頁的 app 雲端開發平台，協助 app 開發者在雲端快速建置後端服務，提供即時資料庫，有效縮短 app開發時間，並幫助開發者更專注在前端的優化。 

### 2. Node.js 框架

後端用的框架。node.js 對於前端來說嚴格來說是一個開發環境，讓電腦環境也能跑 JavaScript 寫好的程式碼，也因此能用來與後端資料庫做串接。

---

## 十五、總結：一招練一萬次勝過一萬招

還有推薦我一個網站 Github 上的 Roadmap 有空再研究，其實雖然以上功能真的很多，但 Junior 可能一開始都不用碰到，因為每間公司有自己的習慣工具，就大概懂概念進去後馬上用得到學起來就好，不過如果能夠事先寫過幾行代碼有個印象了解運作邏輯，應該在面試時更好表現自己的優秀，總而言之呢，不要害怕工具推陳出新，李小龍說過一句話，這在工程師的世界應該也是適用的。