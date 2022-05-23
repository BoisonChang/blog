---
title: "百日轉職前端工程師：第七週前端魔王 JavaScript《DAY 14》"
slug: Transfer-FrontEnd-100-14
date: 2020-09-04T14:43:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "JavaScript"]
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

大家好，這是百日轉職前端工程師的 Day14，也是 9/04（五）。今天開始要來復盤之前學過練習過前端基礎，也就是真正踏入前端工程師的真正門檻 JS，千萬不要搞錯 Java 和 JavaScript 歐，不然會被笑的！

所以這篇文章會解釋嗎？不會。（自己查，對我就是這麼壞心～ ）......總之， JS 算是我還滿喜歡的部份，在前三週程式基礎就玩過了，但這週開始真正用 JS 來操控網頁意外的有成就感！

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、JavaScript 跑在網頁上跟跑在 Node.js 上差在哪裡？

老實說這個問題可以很簡單的回答，執行環境（runtime）不同，能使用的 function 和程式運行的特性也會不同。

> JavaScript 是一個程式語言，會有程式語言本身所規範可以用的東西，例如說用 var 宣告變數，用 if else 進行判斷，或者是使用 function 宣告函式，這些東西都是 JavaScript 這個程式語言本身就有的部分。既然我上面說了「程式語言本身就有的部分」，就代表也有一些東西其實是「不屬於 JavaScript 這個程式語言的」。Node.js 其實是 JavaScript 的一個執行環境（runtime），JavaScript 最常見的兩個執行環境（runtime）就是 Node.js 和 瀏覽器。

by Huli

比如 JavaScript 在網頁中執行時就能用到 document 、拿來計時的 setTimeout 與 setInterval，以及拿來做 ajax 的 XMLHttpRequest 與 fetch，這些都是瀏覽器這個執行環境所提供的東西。

> 在早期 JavaScript 是為了在瀏覽器裡面而開發的。

不過後來 JavaScript 被應用在很多平臺，現在大多數的人使用 Node.js 這個執行環境（runtime）來執行後端程式。

所以，JS 在前端、後端都可以使用。那 Node.js 也提供了一些工具， 如用來控制檔案的介面 file system (fs)，讓 JavaScript 能讀寫電腦裡的檔案！還提供了 http 這個模組，可以用 JavaScript 來寫 server。

> **程式運行的底層邏輯，程式語言多有以下三個特性：**

1. 語法解析器 Syntax Parser
   當我們在寫 JavaScript 時，你的程式碼並不會神奇的直接告訴電腦該做什麼，而是有人寫了一個程式，這個程式會一個字一個字的讀你的程式碼，並且判斷語法是否有效，最後轉換你的 JavaScript 成為電腦看得懂的東西，這些程式稱為編譯器 (Compilers)。你的電腦真正執行的是你所寫出來的程式碼被「轉譯」過的語言。

2. 詞彙環境 Lexical Environment
   詞彙環境存在於一些程式語言中，這些程式語言認為程式碼寫在哪裡是很重要的。它必須以正確的格式和位置，才能被 語法解析器 ( Syntax Parser ) 讀懂並轉譯，特定的程式寫在哪裡是很重要的，例如：當我們撰寫出一段宣告變數的 JS 語法，它被寫在哪呢?它在一個函數中嗎?它的周圍的環境是什麼?它被包在物件、陣列中嗎? 詳細圖解可參考 JavaScript 基礎二三事系列 Day02 - 幾個名詞小觀念。

3. 執行環境 Execution Context
   我們有許多的「詞彙環境 （Lexical Environment）」，程式碼實際上所在的位置，但哪個才是正在執行的？就是被執行環境（Execution Context）所管理。

> **我們說當 JS 開始執行的時候，程式碼必須被執行在下面三種環境之一**

1. 全域 Global：預設當您程式開始執行時的環境
2. 函式：當我們進入一個函式 function 時的環境，也就是開始跑函式內部程式碼的時候
3. Eval：把一串字串，當作指令來執行時的環境

> 參考資料：

1. [JavaScript 中的同步與非同步（上）：先成為 callback 大師吧！](https://blog.techbridge.cc/2019/10/05/javascript-async-sync-and-callback/)
2. [語法解析器、執行環境、詞彙環境](https://medium.com/icguanyu/%E8%AA%9E%E6%B3%95%E8%A7%A3%E6%9E%90%E5%99%A8-%E5%9F%B7%E8%A1%8C%E7%92%B0%E5%A2%83-%E8%A9%9E%E5%BD%99%E7%92%B0%E5%A2%83-9ab4137965cc)
3. [Node.js 是什么？Node.js 与 JavaScript 的区别是什么？](https://www.jianshu.com/p/21d3130ca059)

---

## 二、什麼是 DOM？

DOM 全名為 Document Object Model，DOM 是一種介面的定義，根據 W3C DOM 規範，DOM 是一種與瀏覽器，平台，語言的接口。簡單來說它能把一份 HTML 文件內的各個標籤，包括文字、圖片等等都定義成「物件」，而這些物件最終會形成一個樹狀結構，提供瀏覽器跟 JS 溝通的橋樑，在 JS 上可以運用各種 DOM API 去操作 DOM 實現網頁功能。DOM API 定義了讓 JavaScript 可以存取、改變 HTML/XML 架構、樣式和內容的方法，甚至是對節點綁定的事件。

DOM 最常被用以與 JavaScript 溝通，也就是說雖然程式以 JavaScript 寫成，但使用 DOM 來存取頁面及其元素。而 DOM 其實可以與任何程式語言共同運作。一個網頁的所有元素組織在一起，就構成了一棵「DOM 樹」。

在 DOM 中，每個 元素 (element) 、 文字 (text) 等等都是一個節點。

> **而節點通常分成以下四種（可參考上篇 前端基礎 HTML 做知識的連結）**

1. Document: Document 就是指這份文件，也就是這份 HTML 檔的開端，所有的一切都會從 Document 開始往下進行
2. Element: Element 就是指文件內的各個標籤
3. Text: Text 就是指被各個標籤包起來的文字
4. Attribute: Attribute 就是指各個標籤內的相關屬性
