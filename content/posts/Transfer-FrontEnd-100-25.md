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

先介紹他們在資料結構的程式世界最原始的定義，然後我們再開始介紹，他們是如何協力完成 Event Loop 的運作的。

- Stack
    - 中文翻譯為堆疊，是資料結構的一種
    - 它就像是疊盤子一樣，特性為後進先出 (LIFO = Last In, First Out)
- Queue
    - 中文翻譯為佇列，是資料結構的一種
    - 它就像排隊一樣，特性為先進先出 (FIFO = First In, First Out)
- Web APIs
    - Web API 是瀏覽器提供的方法，它並不是 JavaScript 引擎的一部分，且運作於瀏覽器端，也就是說他們可以同時運行
    - 常見的 Web API 有 setTimeout、XMLHttpRequest 等等，詳細可以看 MDN/Web APIs。


### 4. Event Loop 的運作機制

#### 1. Call Stack

這個區塊的運作原理使用 Stack 的方式，程式碼會先到這個區塊執行以下的操作：

- 若同步的動作則直接執行
- 非同步的動作則丟到  Web API 做處理
- 若 function 內有另一個 function 則向上堆疊  (Stack)
- 執行完成後則移出 Call Stack

#### 2. Web API

- 到這邊的「非同步」動作會在瀏覽器背景執行
- 這裡並沒有所謂 Stack 或是 Queue 的概念，而是先執行完成會先丟到 Callback Queue 去做等待

#### 3. Callback Queue

- 進到這邊的函式會等待 Call Stack 清空後才依序將其放回 Call Stack 執行
- 看名字就知道這邊是一個 Queue
- 也就是 Web Api 執行完成後，先進入這個 Queue 的函式會先被放入 Call Stack

講了這麼多，總算可以回歸到結論，其實 Event Loop 指的就是這一整個循環。當 Call Stack 被清空則會檢視 Callback Queue，並將其放入 Call Stack，就這樣不斷的循環，達成一個不阻塞的機制。不過講這麼多不如看兩張圖，勝過千言萬語。

```
console.log('Hi')
setTimeout(function cb1() {
console.log('cb1')
}, 5000)
console.log('Bye')
```

![](https://miro.medium.com/max/1778/0*0EpvJV7Wc6TLNtF6.gif)
圖片來源: [How JavaScript works](https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5)

總而言之，Event Loop 這個漂亮的機制設計，讓我們能夠有滑順如斯的網頁瀏覽體驗，它把麻煩的東西（非同步）分開處理，可說是簡單卻又設計精妙。


> 參考資料
1. [【筆記】到底 Event Loop 關我啥事？](https://medium.com/infinitegamer/why-event-loop-exist-e8ac9d287044)
2. [JavaScript - Event Loop](https://ithelp.ithome.com.tw/articles/10230871)
3. [理解 Javascript 裡的 event loop 運作](https://medium.com/@kind_grullo_fish_990/%E7%90%86%E8%A7%A3-javascript-%E8%A3%A1%E7%9A%84-event-loop-%E9%81%8B%E4%BD%9C-32aca70b1b0b)
4. [理解 JavaScript 中的事件循環、堆疊、佇列和併發模式](https://pjchender.blogspot.com/2017/08/javascript-learn-event-loop-stack-queue.html)


---

## 二、什麼是作用域（Scope）？

> **Javascript 的 scope 是什麼？**
- 其可稱為作用域 / 範疇 / 生存範圍 / 存在範圍
- 意思是變數可以被取用 (accessibility)，或者可以被看到 (visibility) 的有效範圍

### 1. JavaScript 的作用域

JavaScript 的作用域可以分為以下三個層級：

#### I. Global Level Scope

- JavaScript 代碼在編譯階段的最初，會產生一個全域執行環境 (Global Execution Context)
- 在全域的執行環境中，會存在一個全域的變數物件 (Global Variable Object)
- 在全域環境中被宣告的變數（或者函式），就會被存放在這個 Global Variable Object，可在整個程式的任何地方被存取
-  Global Variable Object 的作用域是全域 Global Level Scope，也就是所謂的全域變數 (Global Variable)

#### II. Function Level Scope

- 變數如果是在函式內 (函式的 {} 內) 被宣告。他的影響就只限於這個函式最外層的 {} 中
- 這種變數出了 {} 就完全沒用了 (記憶體會被回收)
- 想要使用它會出現 ReferenceError。而我們會把這種變數稱之為區域變數 (Local Variable)

#### III. Block Level Scope (ES6)

- 塊級作用域 (Block Level Scope) 是一種更小的作用域。只存在於 {} 中
- 最常出現在 Function Scope 中的 {} 中（像是 if、for 等語法）
- 在 ES6 之前，我們宣告變數只能使用 var 來宣告
- 使用 var 宣告變數會有不少缺點，也無法形成 Block Scope (只有 Function Level Scope 和 Global Level Scope)
- 以至於 ES6 推出了另外兩個宣告變數的方式：const 和 let

是不是也可以將 Function Scope 看成是 Block Scope 的一種？是的，雖然有點不同，但要這麼想也不是不行。更何況，在 ES6 之前，是沒有塊級作用域這種概念的。


### 2. Static Scope / Lexical Scope

要對於作用域 (Scope) 應該會有更深刻的理解，可以稍微來聊聊 **Static Scope 靜態作用域 （也可以稱為 Lexical Scope 語彙作用域）**。

讓我們用以下範例來理解:

```
var thisIs = 'global';
let func1 = () => {
console.log(thisIs)
}
let func2 = () => {
var thisIs = 'local';
func1();
}

func1() // 'global'
func2() // 'global'

```


func2() 印出 global 很合理，但 func1 竟然也印出 global 耶！其實只要記得以下簡單的說明，區分這個程式語言是靜態作用域 (Static Scope)，還是動態作用域 (Dynamic Scope)，就能明白印出結果的差異。


>** 靜態作用域和動態作用域差別**
- 靜態作用域 (Static Scope)
    - 如果在 func1 的地方會印出 global ，這種程式語言採用的就是靜態作用域 (Static Scope)，例如：C / Java / JavaScript 
- 動態作用域 (Dynamic Scope)
    - 如果在 func1 的地方會印出 local的，這種程式語言採用的就是動態作用域 (Dynamic Scope)，例如：perl


>** 靜態作用域的特性**
- JavaScript 中的「閉包」 (Closure) 是函式以及其「語彙環境」 (Lexical Environment) 的組合，所有的函式都能夠記住被創造的當下的環境以及變數
- 「靜態作用域」（lexical scope / 語彙範疇 / 詞法作用域）跟「動態作用域」最大的區別就是，靜態作用域函式內的變數是在這個函式被宣告時 (代碼被編譯時) 就已經設定好的，也就是 early binding
- 對於 Javascript，只要暸解它是採用靜態作用域 (Static Scope) 或是所謂的語彙作用域 (Lexical Scope) 就可以了

所以不管這個函式在哪被呼叫，它內部的變數位置早就被決定了，並不會因為被呼叫的位置而改變。而動態作用域，則是取決於函式被執行時代碼的狀態，來決定函式內的變數。也就是 late binding。


### 3. 作用域鍊 (Scope Chain)

> **Scope Chain 是系統用來尋找變數數值的路徑**

假設 function test 中使用了 a 這個變數，但系統在這個 function  test 裡面找不到 a 的數值是多少，所以就到 global 去找，而這個尋找數值的路徑，就是所謂的 作用域鍊 (Scope Chain)。

要搞懂 JavaScript 尋找變數數值的路徑的原理之前，要先了解兩個 ` [[Scope]]` 和 scopeChain 兩個東西，先大略說明定義，後面看程式碼會更清楚：

#### I.` [[Scope]]`

- 每一個 function 只要被「宣告」的時候（我理解宣告是在編譯階段），就會產生一個 `[[Scope]]`
- 若是 function 是多層次包覆的，在全域環境的最外層的那個 function  A 被宣告時，就會產生一個 `A.[[Scope]] `
    - 而內容就是全域環境 scopeChain，也就是讓 function A 內使用的變數若是在 Function Level Scope 內找不到值，可以循著這條 scopeChain 去全域環境找值。（別急，待我解釋 scopeChain）


#### II. scopeChain

- 當 function 被「呼叫」的時候（我理解宣告是在編譯階段），就會產生一個 scopeChain
- 而若是 function 是多層次包覆的，在全域環境的最外層的那個 function A 被宣告時，其實全域變數的 Execution Context (EC) 也已經被呼叫了
    - 同時也創建了一個 scopeChain 是全域變數的 Variable Object (VO)，裡面儲存了全域環境中的全域變數的值


> **到底 `[[Scope]]` 和 scopeChain 怎麼合作運作起來，發揮「尋找變數數值的路徑」 的功能？**

- 當 function A 被「呼叫」
    - 其內容中同時「宣告」了 function B，此時 `B.[[Scope]]` 產生
    - 而當 function  A 被「呼叫」時，其也同時產生了一個 scopeChain
    - scopeChain 裡面有 A 的 Activation Object (AO) ，儲存了 function A 中變數的值，同時還有一個 `A.[[Scope]]`
    - 此處的 `A.[[Scope]]` 就是全域環境 scopeChain，也就是讓 function A 內使用的變數若是在 Function Level Scope 內找不到值，可以循著這條 scopeChain 去全域環境找值

> **接著讓我們看程式碼就一目瞭然了，上述所提及的全域環境可以簡化理解為 globalEC，而 function A 則是 function test**

```
var a = 1;
function test() {
var b = 2;
function inner() {
var c = 3;
console.log(b);
console.log(a);
}
inner()
}
test()
```

> **一開始在 global EC 建立時，會先初始化 global EC 的 VO 及 global EC 的 scopeChain**

```
globalEC: {
VO: {
a: undefined,
test: function
},
scopeChain: [globalEC.VO]
}
// 因為有宣告 test 所以會建立 test 的 [[scope]] 屬性
test.[[scope]] = globalEC.scopeChain
```


> **接著開始執行程式，test() 被呼叫時，testEC 被建立，初始化 testEC.AO 及 testEC 的 scopeChain**
1. a = 1 ，將 globalEC.VO.a 更改為 1
2. 呼叫 test()

```
testEC: {
AO: {
b: undefined,
inner: function
},
scopeChain: [testEC.AO, test.[[scope]]]
}
// 因為有宣告 inner 所以會建立 inner 的 [[scope]] 屬性
inner.[[scope]] = testEC.scopeChain
globalEC: {
VO: {
a: 1,
test: function
},
scopeChain: [globalEC.VO]
}
test.[[scope]] = globalEC.scopeChain
```

> **接著開始執行 test() 函式，inner() 被呼叫時 innerEC 被建立，初始化 innerEC.AO 及 innerEC 的 scopeChain**
1. b = 2 ，將 testEC.AO.b 更改為 2
2. 呼叫 inner()


```
innerEC: {
AO: {
c: undefined
},
scopeChain: [innerEC.AO, inner.[[scope]]]
}
testEC: {
AO: {
b: undefined,
inner: function
},
scopeChain: [testEC.AO, test.[[scope]]]
}
inner.[[scope]] = testEC.scopeChain
globalEC: {
VO: {
a: 1,
test: function
},
scopeChain: [globalEC.VO]
}
test.[[scope]] = globalEC.scopeChain
```

以上，大功告成，其實 innerEC.scopeChain 展開後就等於 innerEC.scopeChain = [innerEC.AO, testEC.AO, globalEC.VO] 歐，是不是很像一條層層網上的關係鏈呀，function 之間就是透過這樣的機制，找到彼此作用域內的變數的值呦。


> 參考資料
1. [JavaScript 進階 05 - Execution Context - Scope Chain](https://mtr04-note.coderbridge.io/2020/09/26/--%E7%AD%86%E8%A8%98--javascript-%E9%80%B2%E9%9A%8E-execution-context-scope-chain/?utm_source=coderbridge-com&utm_medium=indie_related_post_title&utm_campaign=Day%200%20-%20%E5%89%8D%E8%A8%80_%20[%20%E7%AD%86%E8%A8%98%20]%20JavaScript%20%E9%80%B2%E9%9A%8E%2005%20-%20Execution%20Context%20-%20Scope%20Chain_@krebikshaw) 
2. [Javascript 的作用域 (Scope) 與範圍鏈 (Scope Chain)](https://medium.com/itsems-frontend/javascript-scope-and-scope-chain-ca17a1068c96) 
3. [[JS] Scope 作用域](https://medium.com/take-a-day-off/js-scope-%E4%BD%9C%E7%94%A8%E5%9F%9F-ee536640963b) 
4. [Day 9：圖解變數作用域(Scope) / 裡面的圖片特別讚！](https://something-about-js-book.onejar99.com/day09) 
5. [JavaScript 進階 03 - Execution Context - Variable Object](https://mtr04-note.coderbridge.io/2020/09/25/about-variable-object/) 

---

