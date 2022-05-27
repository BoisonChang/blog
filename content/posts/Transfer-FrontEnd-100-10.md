---
title: "百日轉職前端工程師：第三週 JS 程式基礎《DAY 10》"
slug: Transfer-FrontEnd-100-10
date: 2020-07-20T20:51:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "JavaScript", "ES6"]
keywords: ["FrontEnd", "Transfer", "JavaScript", "ES6"]
author: "Boison"

---

大家好，這是百日轉職前端工程師的 Day10，也是 7/20（一）。今天開始要來復盤之前學過練習過程式基礎下，這塊是介紹 JavaScript 裡面一些比較進階的功能，主要會介紹常用函式、模組、測試還有 ES6 的一些新增的好用功能。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、常用內建函式如何使用

以下整理陣列和字串的內建函式，詳細功能介紹請點擊 百日轉職前端工程師：常用內建函式《DAY 9》

> **Array 常用內建函式 function**

1. Array.map
2. Array.filter
3. Array.indexOf
4. Array.reverse()
5. Array.fill()
6. Array.join()

> **String 常用內建函式 function**

1. String.trim
2. String.toLowerCase
3. String.endsWith
4. String.padEnd
5. String.slice
6. String.repeat

---

## 二、使用模組 (Module)

一個 Javascript 檔案，就是一個模組/模塊，當寫好一個功能將它做妥善的處理後，就成為一個模組，你可以自己用也可以讓別人來一起用，我們在 Node.js 的環境中可以使用很多別人寫好的模組，非常方便！

Node.js 本身也提供了一些模組給我們使用，可以到 Node.js v14.5.0 Documentation 查詢。在使用上需要先宣告一個變數引入「模組」如下：

```
var 變數名稱 = require('模組名稱')
```

> **以下介紹幾個常用的重要模組**

- http：它包含可以用來建立 http server 的一些類別，方法及事件。
- url：它包含可以解析 url 的一些方法。
- querystring：它包含可以處理由 Client 端傳來 querystring 的一些方法。
- path：它包含可以處理一些檔案或資料夾路徑的方法。
- fs：它包含檔案的存取／操作的一些類別，方法及事件。
- util：它包含一些可供程序者使用的效能函式。

> 資料來源：[自建模組(Local Modules )與如何使用](https://ithelp.ithome.com.tw/articles/10185008)

---

## 三、為什麼我們需要 unit test

1. 確保你寫好的程式能夠在各種邊界條件下，都能夠運行無阻，並且跑出你設想中正確的結果
2. 隨著時間過去當你要修改程式時，能夠在當初設想的邊界條件下也能夠運行
3. 當別人要修改你的程式碼時，別人對於你程式碼的掌握程度未必有原撰寫者那麼高，unit test 則可以確保別人在對你當初寫好的程式做修改時，能夠在各種情況下跑出你當初預期中正確的結果

---

## 四、什麼是 unit test

unit test（單元測試）用來確認一個方法在接收特定的輸入後是否會得到預期的輸出。

程式中最小的結構單位就是以 function 為劃分，像這樣測試每一個 function 的結果是否正確就是所謂的單元測試 ( Unit Test )，相對於單元測試，整合測試 (Integration Test) 是指檢查數個元件之間的互動。

unit test 也就是你寫好這個測試的「條件」，去跑跑看你的程式在這個條件下輸出的結果是否符合你原本預期的！你可以寫很多奇奇怪怪的邊界條件，看看你的程式是不是在各種情形下都能跑出正確的預期中的結果。

> **TDD (Test-driven Development)**

Test-driven Development，中文為測試驅動開發，就是把一般測試的流程倒過來，先寫好測試再開發程式。這樣有個好處是，當寫完程式時就已經完成測試了，而且一開始也會需要把邊界條件等想得更清楚。

> 延伸閱讀

1. [JUnit Test Infected: Programmers Love Writing](http://junit.sourceforge.net/doc/testinfected/testing.htm)
2. [JUnit A Cook's Tour](http://junit.sourceforge.net/doc/cookstour/cookstour.htm)

---

## 五、如何寫 unit test

此處運用 Jest 這個框架來寫 unit test。

Jest 是前端測試框架（Test runner）的一種，會使用是因為它與 React 的整合度較佳（同為 Facebook 開發的）。其他框架例如：Jasmine（茉莉花）、 Mocha（摩卡咖啡）、和 AVA 等。

## 六、如何測試一個 function

1. 安裝 jest 模組：安裝 jest 模組
2. 寫 test 程式碼
   - 先從原檔案把要測試的 function 輸出 export：module.exports = Join
   - 新建立一個檔案 index.test.js (原始檔案為 index.js)
   - 先宣告一個變數用 require 從原檔案引入 function：var test = require('./index')
   - 寫一個說明的 describe：describe('測試說明', function(){ test… })
   - 寫一個單元測試的 test：test('測試說明', function( ){ expect(測試的函式).toBe(預期要回傳的東西)})
3. 設定執行指令 test

- 打開 package.json 看到 devDependencies 已經增加了 jest 代表已經安裝成功
- 找到 scripts 中的 test 將其對應的預設值直接改成 jest，可使用 npm run test
- 會去幫你找所有副檔名是 .test.js 的檔案

> **只想 test 單一檔案的方法**

1. 若檔案名稱為 index.test.js
2. 打開 package.json 會看到 devDependencies 已經增加了 jest 代表已經安裝成功
3. 找到 scripts 中的 test 將其對應的預設值直接改成 jest index.test.js / 若 npm 版本夠新，可直接在終端機輸入 npx jest index.test.js

> 資料來源：[JavaScript — 測試框架 Jest](https://miahsuwork.medium.com/%E7%AC%AC%E4%B8%89%E9%80%B1-javascript-%E6%B8%AC%E8%A9%A6%E6%A1%86%E6%9E%B6-jest-eccf0ff2cea2)

---

## ES6 的強大功能

ECMAScript (ES)是 JavaScript 的規格標準，而 ES6 就代表 ECMAScript 第六版本的意思。這版本做了很多顯著的更新，多了很多強大的功能如下：

### 1. let 與 const

- let 就是只在這個 function 中生效的變數宣告
- const 則是一旦宣告即視為常數不再可更改的變數

### 2. Template Literals 模板字串符

字串中想要加值，直接用用兩個反引號 (back-tick) 包覆字串，並在其中要加入值得位置使用 ${}，如 a + b = ${sum(a, b)}

### 3. Destructuring 解構

可以直接同時變數命名，並且把陣列中按照順序的值取出放到變數中

```
arry1 = ['1', '6', '8']
[first, second, three] = arry1
console.log(first, second, three) // '1', '6', '8'
```

```
arr1 = arry1 = ['1', '6', '8', '10', '15']
[first, ...others] = arry1
console.log(first, others) // '1', ['6', '8', '10', '15']
```

> 資料來源：[JavaScript ES6 語法糖 - 解構賦值介紹](https://w3c.hexschool.com/blog/1e2092e9)

### 4. Spread Operator 展開運算子

上述解構中遇到情境時用 ... 稱之為其餘運算符 (Rest Operator)，而若在某些情境下 ... 稱之為展開運算符 (Spread Operator)，可將陣列中所有元素各別取出。

```
const params = [ "hello", true, 7 ]
const other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

```

### 5. Arrow Function 箭頭函式

當你想要宣告無名函式時特別好用，可以用比較簡短的方式去寫出 function。

> 範例 1

- request.get( 'url', function (error, response, body) {}
- request.get( 'url', (error, response, body) => {}

> 範例 2

- function(value){}
- value => {}

### 6. 加上預設值：Default Parameters

- 宣告函式 (function )時可以直接為其參數 (parameters) 加上預設值
- function repeat(str = 'hello', times = 3){}

### 7. Import & Export

> **Export 三種方式**

- export function add(){}，使用 import {add} 引入
- export { add, ... }，與上面那種一樣
- export default function add()，使用 import add 引入，不需要加大括號

> **Import 四種方式**

- import {add , ... } from './fileName'
- import add, {PI , ... } from './fileName'. // export 時就以 add 為 default
- import {default as add, PI , ... } from './fileName' // export 時就有 default
- import \* as utils from './fileName'

> **as**

- 在 Import/ Export 都可以 as 和一代稱加在 function 後面作代稱
- export { add as addFunction }
- import \* as utils from 'utils'

### 8. Babel

> **是一種 JavaScript 轉譯器，可將 ES6 轉換 ES5**

比如當你想要用上述 7.Import & Export 屬於 ES6 新增的語法的話就需要用到 Babel，此時你就不用擔心執行環境版本問題，Babel 基本上都會幫你搞定。（實際在做網頁時也不用擔心瀏覽器的支援度的問題）

1. 安裝必要套件：npm install --save-dev @babel/core @babel/node @babel/preset-env
2. 新增 .babelrc：touch .babelrc
   - 內容寫入以下：{"presets": ["@babel/preset-env"]}
3. 輸入指令：npx babel-node index.j

---

## 八、總結

藉著整理把很多不熟的地方補起來，像是單元測試，還有這陣子陸續加深瞭解程度的 ES6 語法如解構等，東西還滿多的之後如果有用的可以再分別延伸成一篇去做一個完整的整理，好像不知不覺其實學了不少東西，之後要盡量早點整理可能記憶猶新時會在學習上更有效率更節能。
