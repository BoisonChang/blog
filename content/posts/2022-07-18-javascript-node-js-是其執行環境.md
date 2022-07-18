---
title: "JavaScript: Node.js 是其執行環境"
date: 2022-07-18T08:21:00.150Z
author: Boison
slug: Javascript-NodeJS
tags:
  - JavaScript
  - NodeJS
  - Runtime
draft: false
---
> **V8 是 JavaScript 的編譯器**

JavaScript 是直譯式程式語言，需要存在「直譯器」 interpret 程式碼後再執行，因此 Node.js 使用 V8 虛擬機進行直譯（一個由 Google 開發的開源 JavaScript 引擎），Chrome 和 Node.js 都是用 V8 在執行 JavaScript。

> **Node.js 是 JavaScript 的 Runtime 執行環境，讓其能脫離瀏覽器的環境運行程式碼**

而定義上來說 Node.js 是 JavaScript 的 Runtime 執行環境，要在電腦上執行 JavaScript 就需要有 Node.js。

> **JavaScript 同時具備直譯語言和編譯語言的特性**

編譯語言的好處是可以在執行前的編譯階段，審視所有的程式碼將可以做的優化都完成，但直譯語言就無法做到這一點，因為執行時才開始解釋的關係，執行上就相對較慢，也沒辦法在一開始做優化。

為了處理這個狀況 JIT 出現了，中文名稱是即時編譯，這也是 V8 引擎所採用在執行時編譯 JavaScript 的方式。

JIT 的方式結合解釋和編譯兩者，讓執行 JavaScript 的時候，能夠分析程式碼執行過程的情報，並在取得足夠情報時，將相關的程式碼再編譯成效能更快的機器碼出現了。

---

> **參考資料:** [執行JavaScript 的V8 引擎做了什麼？](https://medium.com/starbugs/%E5%9F%B7%E8%A1%8C-javascript-%E7%9A%84-v8-%E5%BC%95%E6%93%8E%E5%81%9A%E4%BA%86%E4%BB%80%E9%BA%BC-f97e5b4b3fbe)