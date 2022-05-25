---
title: "百日轉職前端工程師：第十六週 JavaScript核心底層邏輯《DAY 25》"
slug: Transfer-FrontEnd-100-25
date: 2021-01-13T21:42:21+08:00
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

大家好，這是百日轉職前端工程師的 Day25，也是 2021/01/13（三），這週的主題會 JavaScript 最難但也是最核心，也是面試最常考的部分，我沒有把握百分之百掌握，但每一篇我都會嘔心瀝血用平易近人的方式書寫，讓任何有點 JS 基礎的人都能輕鬆讀懂，務必以讓看的人能夠看一篇抵三篇為目標， JavaScript 的底層邏輯到底怎麼運作的，就讓我們開始吧（抖

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、Event Loop 的運作方式

### 1. Event Loop 是什麼？


> **首先要問的當然是，Event Loop 是什麼？**

- 可以想像 Event Loop 是一種瀏覽器內建幾乎無時無刻、每一毫秒都在執行的程式
    - 負責檢查現在主執行環境堆疊是否是空的
    - 如果是空的，再去檢查 Queue 
    - 若 Queue 有函式待執行，則將這些函式從 Queue 依序移出，並執行
- 由於 Event Loop 是為瀏覽器而生的，因此它並未出現在 ECMAScript 的標準中
- 反而是在 HTML Living standard of Event loops 中被定義

> **誒等等，那......Queue 又是什麼？**

讓我們先簡單敷衍你一下，Event Loop 這個程式下面有很多的小夥伴，Queue 是其中之一，但在解釋 Queue 是什麼之前，讓我們先來問另外一個更重要的問題。

### 2. 為什麼要有 Event Loop？

我們已經知道了 Event Loop 可以大概想成是另外一個幾乎無時無刻、每一毫秒都在執行的程式，然後反覆檢查、移動，和執行函式。


> **但.......為什麼要有 Event Loop？**

凡存在必有原因，Event Loop 幫我們解決 / 優化了兩個惱人的問題：

1. Event Loop 能協助非同步請求的實現，並讓網頁產生連貫的畫面呈現（influent UI）不會卡卡的
2. Event Loop 將「費時較久」或「須等待事件才能啟動」的任務往後安排
    - 因而能打造流暢的使用者體驗 (Outstanding UX)，這是它內建的一個機制，其實也就是第一點的換句話說

> **你有想過若沒有 Event Loop 的機制，為何網頁就會卡卡的嗎？**

且讓我們用簡單幾句話考古帶過好嗎？

- 網頁基本上可以想像成一個傳遞紙條的遊戲
    - 你（A）傳給了別人（B）一個請求，他則回傳給你一個回應
    - 可是網頁上幾乎每個動作都會觸發這個機制，你應該不想在傳出請求後，等了又等，終於收到需求，馬上又觸發一個請求，然後又在等......（暴躁），無限循環的等待，根本是地獄。
- 後來 JavaScript 發明了
    - 發現很多使用者的在網頁上的操作，可以直接透過 JavaScript 這個程式語言在網頁前端的介面，簡單又快速的處理，不用老是一個動作一個請求一個回應。
- 不過呢～JavaScript 設計的初衷是而為了讓開發者可以專注在程式開發上
    - JavaScript 被設計為「單線程」(single threaded runtime)
    - 也就是一次只單一執行一小段程式碼
    - 也因此不必煩惱「並發性議題」(concurrency issues) 的問題

好啦，考古學講完了，所以你知道 JavaScript 如果一次只能執行一件事情，那遇到要處理特別久的事情，網頁就會卡卡的，為了不要卡卡的，Event Loop 就完美的幫我們解決了這個問題**，要弄很久的動作就先放到一邊，先讓整個網頁跑完**，再來把這些很花時間的動作，好好處理處理，不干擾使用者滑網頁的整體體驗。

### 3.  Event Loop 是怎麼運作的？

OK，可以回歸正題了，前面已經提到 Event Loop 可以大概想成是另外一個幾乎無時無刻、每一毫秒都在執行的程式，然後反覆檢查、移動，和執行函式。(要講幾次)

但 Event Loop 底下其實有很多小夥伴支援，共同完成這個循環 (Loop)。