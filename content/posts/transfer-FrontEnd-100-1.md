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


註：如果你想要學習如何在百日內「高效學習」上路實戰，非常推薦閱讀《超速學習》一書。

---
## 一、必備技能

### 1. Git
類似 RPG 的存檔功能，可以隨時回到上個存檔點。

> 比較
- Git:  是一種版本控制軟體，用於控制版本並與團隊共同協作同一專案。
- GitHub: 是一個商業網站，GitHub 的本體是一個 Git 伺服器。
    - 但這個網站上的應用程式讓大家可以透過 Web 介面來操作一些原本需要複雜的 Git 指令才能做到的事。
    - 但事實上類似這樣的商業網站還有很多，其中另一最常被使用的則是 GitLab，功能基本大致上與 GitHub 相同，但其各有其主打的功能。
    - GitLab 除了 倉庫管理 和版本控制，它還提供 Wiki 託管和錯誤跟踪系統。 

### 2. Terminal
不透過圖形化介面，直接用這個工具寫程式碼向電腦下指令的工具。

在 OS 系統它就叫做 Terminal，而在 Windows 系統中可以透過命令提示字元 (cmd.exe) 開啟 Terminal。

> 在不同的作業系統下大致上可以照以下方式叫出 Terminal ：
- Windows: 開始 → 所有程式 → 附屬應用程式 → 命令提示字元 → 輸入 powershell → 終端機 (Terminal)
- Mac OS X: 應用程式 → 工具程式 → 終端機 (Terminal)
- Linux:  應用程式 → 附屬應用程式 → 終端機 (Terminal) 

---

## 二、CSS 預處理

目前為止，在眾多優秀的 CSS 預處理器語言中就屬 Sass/Scss、LESS 和 Stylus 最優秀使用的人也最多。

基本上就是把 CSS 的語法在比較方便的環境中編撰（預先處理，可能覺得原始的程式碼編寫會有諸多不方便或者不直覺），但是這些程式碼不能直接執行，打包之前還要做一些處理才能跑。

Sass 是一種基於 CSS 所產生的高階語言，他將一般程式語言所擁有邏輯概念和變數帶入 CSS 樣式表中，讓我們在撰寫網站的樣式時，可以用更快速、更方便的方式撰寫，而這是網路上可以直接試著寫 Sass 轉成 CSS 以及 Scss 轉成 CSS 的線上編譯器。

>  Sass 和 Scss 的差別？
- 基本上是同樣的東西，差別只差在撰寫的方式不一樣
- Sass 的撰寫方式是用 Ruby 的撰寫方式，沒有分號、括號那些東西，都是用分段的方式來表示
- Scss 而是在 CSS3 推出，撰寫方式與 CSS 相同，比起 Sass，對於寫習慣的 CSS 的人，Scss 相對會比較好上手

> HTML、CSS、JS  的預處理
- HTML、CSS、JS 都可以做模組化，而「預處理器」常是模組化的手段之一
- 例如 pug 是 HTML 的預處理、Sass/Scss 是 CSS 的預處理、Babel 是 JS 的預處理

`資料來源: [一起來用 Sass+Compass 吧 ](https://blog.hellosanta.com.tw/%E7%B6%B2%E7%AB%99%E8%A8%AD%E8%A8%88/%E5%89%8D%E7%AB%AF/%E4%B8%80%E8%B5%B7%E4%BE%86%E7%94%A8sasscompass%E5%90%A7%EF%BC%8C%E6%88%91%E5%80%91%E5%8F%AF%E6%98%AF%E4%B8%80%E7%A7%92%E5%B9%BE%E5%8D%83%E8%90%AC%E4%B8%8A%E4%B8%8B%E7%9A%84%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%B8%AB%EF%BC%8C%E6%80%8E%E9%BA%BC%E8%83%BD%E6%8A%8A%E6%99%82%E9%96%93%E6%B5%AA%E8%B2%BB%E5%9C%A8css%E8%BA%AB%E4%B8%8A%E5%91%A2%EF%BC%81)`

---
# 三、CSS 模組化概念

1. SMACSS 
- SMACSS  全名為 (Scalable and Modular Architecture for CSS) ，偏向整個專案架構的分類及模組化你的 CSS
- 顧名思義是可模組化且具結構分類有命名規則的方式，其中的精神就是容易擴展及模組化
- 基本上的結構分為 Base、Layout、Module、State、Theme

2. OOCSS 
- OOCSS (Object Oriented CSS) 是使用物件導向方式管理 CSS 的 Style
- 其有兩項基本的原則：(1). 結構與樣式分離，(2). 容器與內容分離
- 結構與樣式分離要避免 .content > div > span{ ... ) 的 CSS 寫法
- 容器與內容分離要避免 .left-bar .list-group li span { ... } 的 CSS 寫法

3. BEM 
- BEM (Block, Element and Modifier) 是關於 Class 的命名方式
- 強調完全使用 Class Selector 並且以功能導向命名
- 強調每個功能區塊 Block 裡面的 Element 分成一塊，命名上配合此功能去延伸命名
- 每一個 Block 都會有自己的資料夾路徑，並且裡面有這個 Block 的專屬 CSS 及 JS
- 裡面還會有這個 Block 下方 Element 的資料夾，並且開頭為 __ 







