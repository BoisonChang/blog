---
title: "百日轉職前端工程師：第十六週 JavaScript核心底層邏輯《DAY 25》"
slug: Transfer-FrontEnd-100-25
date: 2021-01-13T21:42:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "Closure", "Hoisting", "IIFE", "EventLoop"]
author: "Boison"

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


> **靜態作用域和動態作用域差別**
- 靜態作用域 (Static Scope)
    - 如果在 func1 的地方會印出 global ，這種程式語言採用的就是靜態作用域 (Static Scope)，例如：C / Java / JavaScript 
- 動態作用域 (Dynamic Scope)
    - 如果在 func1 的地方會印出 local的，這種程式語言採用的就是動態作用域 (Dynamic Scope)，例如：perl


> **靜態作用域的特性**
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

## 三、Hoisting（提升）是什麼？

> **先講 Hoisting 的定義**

在 JavaScript 中宣告的變數和函數，會被 JavaScript 直譯器 (interpreter) 先處理，換句話說，你在 global 或 function 任何位置宣告一個變數，就像在該 Scope 最開頭的位置宣告一樣，這種特性就叫做 Hoisting (提升)。

基本上 hoisting 最簡單，而且最多人會可能遇到的情境，就是發現你在宣告變數的上幾行竟然就已經能呼叫變數，雖然結果可能會是 undefined。

好，在我們深入瞭解 hoisting 之前，更重要的是到底為何要有 hoisting？

### 1. 為何要有 Hoisting？ 

> **因為方便呀！不然勒？**

好啦先不論還沒宣告就使用變數，這是一個壞習慣（雖然可能也方便）會造成易讀性的降低，但你有想過 function 的感受嗎？

就因為有 hoisting 你可以不用把所有 function 通通都寫在第一行就能用了誒！

這根本超讚的好嗎？而且**若是 function A 跟 function B 想要在內部呼相呼叫，啊他們程式碼的撰寫上就有先後順序，沒有 hoisting 就做不到啦。**


### 2. 直接講結論： Hoisting 的功能

我們最開始有小提了一下但還是沒講完 hoisting 所有功能，這邊先講完結論，只想知道怎麼用的小夥伴看完這邊就夠了，而想要理解背後邏輯的小夥伴，且慢別急。

- Hoisting 只會提升宣告
- 提升優先權： 函式（function） > 變數（var）
- 函式的提升可以覆蓋參數（Arguments）
- JS 有個東西叫 Variable Object，負責存放所有宣告
- 實作上，JS 是有編譯階段的，負責處理 VO 的前置作業
- let 與 const 也有 hoisting，但在賦值前被呼叫 a 會拋出 ReferenceError: a is not defined，又稱為處於 Temporal Dead Zone (TDZ) 。

## Hoisting 的原理為何？

要完整解釋 hoisting 其背後運作的機制，其實還要扯到 V8 引擎，何謂 V8 引擎？

V8 引擎是 Google 做出來讓 JS 跟瀏覽器溝通的的開源專案，V8 引擎的運作流程最重要就是以 JavaScript 原始碼將一個一個關鍵字解析成為抽象語法樹 AST，交給直譯器後編譯並執行。

※ 想要詳細瞭解 V8 引擎的請見 [JS 原力覺醒 Day02 - JavaScript V8 引擎](https://www.muji.dev/2020/01/02/iron30-2/) 這篇文章。

> **V8 引擎，在全域執行環境產生之後會做四件事情**
1. 產生全域物件 window
2. 產生 this 物件
3. 會為所宣告的變數保留記憶體空間，但還不會指派程式碼寫入的值，只會給初始值 undefined
4. 也會為一般的函式宣告（使用 function 關鍵字宣告的具名函式）保留記憶體空間
    - 且會將整個函式內容存入「記憶體空間」，也就是被保存到叫作全域記憶體  (Global Memory) ，或稱記憶體堆積 (Heap) 的地方


在這個階段，在執行環境裡面用到的變數跟函數已被存放好，但所有宣告的變數都只有做保留記憶體空間的動作，還沒有被賦值，因此也被稱為「創造階段」（Creation Phase）。創造階段結束後，就會進入「執行階段」（Execution Phase），直到這個時候前面宣告的變數才會被賦值，程式碼才會真的被執行。

> **V8 引擎運作流程就是以 JavaScript 原始碼的關鍵字解析成抽象語法樹，交給直譯器後編譯並執行，可分為三個階段**
1. 解析階段
2. 直譯 & 執行階段
3. 優化階段

其中 2 的直譯 & 執行階段會轉化成轉換成特殊的機器代碼稱為 ByteCode，是能夠被執行的機器碼，可以很快的被編譯成更底層機器碼，此處不多提。我們往上走一層，用「執行環境」去簡單的理解 Hoisting 運作原理，但首先我們先解釋一下「執行環境」 (Execution Context) 在幹嘛。


### 1. 執行環境 (Execution Context)

> **V8 引擎在啟動時，所產生給 JavaScript 專屬的特定環境稱為「執行環境」**
- 在前面提到直譯語言必須依賴環境才能被執行
- 在 JavaScript 裡面，提供這個環境的工作就是由 JavaScript 引擎來擔任
- 所以當我們說「瀏覽器執行 / 讀了你的 JavaScript 程式碼之後出現了錯誤」，其實並不真的是瀏覽器去讀你的程式碼，而是身為瀏覽器一部分的 JavaScript 引擎在做這件事
- 上面提到，能夠讓程式碼被執行的環境也被稱為「執行環境」（ Execution Context ）
- 執行環境是一個抽象的概念，概括地來說，任何你 JS 程式碼被執行、讀取的地方，像是 function 裡、甚至全域 ，都可以是「執行環境」

> **執行環境可以分為以下幾種**
1. 全域執行環境（Global Execution Context）
2. 函式執行環境（Functional Execution Context）
3. eval 函式內的執行環境


> **接著聊到執行堆疊 (Execution Stack)**

- 上面我們提到，每當函式被呼叫時，就會產生對應的「執行環境」
- 而當函式裡有另一個函式被呼叫時，執行環境是按照什麼順序被產生的？ 
    - JavaScript 使用後進先出的「堆疊」結構，依序來儲存隨著函式宣告所產生的執行環境。
    - 「堆疊」本身其實是一種資料結構，在堆疊裡面，某個元素之上如果還有有其他元素就無法被取出，因此有了「先進後出」的特性
    - 如果你有吃過罐裝的品客應該可以很輕易知道我在說什麼，想想看，你沒辦法直接吃最底部的洋芋片對吧！

一句話解釋完「執行環境 」(Execution Context)：每當你進入一個 function 的時候，就會產生一個 EC，裡面儲存跟這個 function 有關的一些資訊，並且把這個 EC 放到 stack 裡面，當 function 執行完以後，就會把 EC 給 pop 出來。


好，那我們到此打住，詳細 EC 跟 stack 的互動，可以參考上面第一點寫過的「 Event Loop 是怎麼運作的？」，我們要開始聊正事 (Hoisting) 了。


### 2. Hoisting 在執行環境 (Execution Context) 怎麼運作的？

Hoisting 簡單來理解就像是把程式碼往上「提升」到更前面執行了。

> **咦？ JavaScript 作為一個直譯的語言，不是應該一行一行跑嗎？**

可是如果真的一行行跑，那怎麼可能達成 hoisting 這個功能？你在執行第 n 行的時候根本不知道 n + 1 行是什麼，想提升是不可能的。

- 事實上，語言一般只會定義抽象語義，不會強制用某種方式實現，像是 C 我們會說它是編譯型語言，可是 C 也有直譯器
- 所以當我們在說某種程式語言是直譯或編譯型的時候，其實是在指涉「大多數」而不是全部
- 而換言之，我們說 JavaScript 是直譯型語言，不代表 JavaScript 不能有編譯器
- 其實主流 JavaScript 引擎多有「編譯」這個步驟

因此要理解 hoisting，其實必須要從 EC 上來理解這件事，程式碼的順序並沒有被更改，而是由於很多參數在「編譯階段」就處理完了，**接下來才是我們直觀理解的一行一行程式碼往下跑的「執行接端」。**


> **當我們在進入一個 EC 的時候（你可以把它想成就是在執行 function 後，但還沒開始跑 function 內部的程式碼以前），會按照順序做以下三件事**
1. 把參數放到 VO 裡面並設定好值，傳什麼進來就是什麼，沒有值的設成 undefined
2. 把 function 宣告放到 VO 裡，如果已經有同名的就覆蓋掉
3. 把變數宣告放到 VO 裡，如果已經有同名的則忽略

好，先備知識總算都充足了，讓我們最後看一小段程式碼引入最後一個概念來理解 hoisting

```
var a = 10          // 第一行
console.log(a)   // 第二行
```

上面這兩行有個差異，第一行的時候我們只需要知道「a 的記憶體位置在哪裡」就好，我們不關心它的值是什麼。而第二行則是「我們只關心它的值是什麼，把值給我就好」，所以儘管兩行裡面都有 a，但你可以看出來他們所要做的事情是不一樣的。

第一行的 a 我們叫它 LHS（Left hand side）引用，第二行叫它 RHS（Right hand side）引用。

> **這邊的 left 跟 right 指的是相對於等號的左右邊，但用這種方式理解的話其實不夠精確，因此像下面這樣記就好**
- LHS：請幫我去查這個**變數的位置在哪裡**，因為我要對它「賦值」
- RHS：請幫我查詢這個**變數的值是什麼**，因為我要用這個值


> **如果有了 LHS 和 RHS 的概念，你可以簡單地想像程式碼是一行一行跑的，但每一行程式碼都有兩位先生在對話**
- 「JS 引擎」會對「執行結果」說：「我在這一行程式碼，看到一個 LHS 或者 RHS，你有看過它嗎？」
- 「執行結果」聽到「JS 引擎」的問題後，則會去作用域 (Scope) 內找
    - 一層一層往上找若沒結果，最後會找到 Global Level Scope 那層，並且根據找的內容結果，來執行那行程式碼


> 參考資料
1. [我知道你懂 hoisting，可是你了解到多深？](https://blog.huli.tw/2018/11/10/javascript-hoisting-and-tdz/)
2. [JS 原力覺醒 Day06 - 提升 Hoisting](https://ithelp.ithome.com.tw/articles/10218457)
3. [JS 原力覺醒 Day03 - 執行環境與執行堆疊](https://ithelp.ithome.com.tw/articles/10216450)
4. [你懂 JavaScript 嗎？#13 拉升（Hoisting）](https://cythilya.github.io/2018/10/20/hoisting/)
5. [提升到哪去？詳解 Hoisting](https://medium.com/%E7%8B%97%E5%A5%B4%E5%B7%A5%E7%A8%8B%E5%B8%AB/%E6%8F%90%E5%8D%87%E5%88%B0%E5%93%AA%E5%8E%BB-%E8%A9%B3%E8%A7%A3hoisting-javascript%E9%8D%9B%E9%8D%8A%E5%AF%A6%E7%B4%80-4d85644f0622)

---

## 五、Closure（閉包）是什麼？

- 閉包是一種資料結構
- 由於閉包 (Closure) 這個字詞有多層意義，你可以說它是一種技術，或是一種資料結構，或是這種有記憶環境值的函式
- 基本上在 JavaScript 中每當函式被建立時，一個閉包就會被產生，閉包是一個函式建立時的就有的自然特性
- 閉包 (Closure) 是由兩個主體構成的一個組合，分別是：
    1. 函式 (function)
    2. 函式被宣告時所在的語彙環境 (Lexical Environment)


在常見的應用上也就是當內部 (inner) 函式被回傳後，除了自己本身的程式碼外，也可以穿越取得了內部函式「當時環境」的變數值，記住了執行當時的環境，這就是「閉包」(Closure)

> **簡單理解閉包 (Closure)**
- 一句話解釋 closure 就是：閉包 (Closure) 是一種函式，它能夠存取被宣告當下的環境中的變數。
- 或者更白話的說，實際應用上就是利用 return 回傳值，並且做一個作用域環境封裝。


> **總之我們先講閉包 (Closure) 實際應用上幫我們解決了什麼問題**
- 你可以在函數執行完，EC 的內容被 pop up 清除後，仍舊把 EC 內存的變數保留下來
- 並在後續去呼叫使用，這個聽起來滿棒的，不是嗎？
- 除此之外，「閉包」(Closure) 也能降低不必要的細節暴露，並提高安全控管的程度


再進一步瞭解閉包的運作原理前，讓我們先更新一些執行環境 (Execution Context) 上的名詞，雖然概念基本上跟前面提過的作用域鍊 (scopeChain) 相同，但在 ES5，ES6 之後較不用 VO / AO 去描述執行環境 (Execution Context) 內的運作，而**改用詞法作用域 (Lexical Environment)、環境記錄 (Environment Record)  等抽象概念名詞去描述其執行環境 (Execution Context) 內部運作的機制。**

> **所以讓我們先來了解一下這些新的名詞中的一些概念吧**

### 1. Lexical Environment（詞法作用域）

簡單來講 Lexical environment（詞法作用域）提供了識別子 (identifier) 與變數 (variable) 的關聯

> **而 Lexical environment （詞法作用域）包含兩個部分**
1. Environment Record（環境記錄）
- 記錄了在該 Lexical Environment（詞法作用域）中被創建的「識別子」(identifier) 與「變數」 (variable) 間的關聯
- 你可以簡單想像此處取代了上述舊版 ECMAScript 中 scopeChain 的功能，讓作用域之間可以彼此相連
2. Reference to an outer Lexical Environment（外部環境參考）
- Lexical Environment（詞法作用域），對外部 lexical environment 的引用構成了多層結構，表示一個作用域指向的外層詞法作用域
- 在查找變量時，如果在當前的詞法作用域裡面沒有找到變量，那就通過 outer 找到外層的詞法作用域，然後再在外層的詞法作用域裡面查找變量
- 如果還沒有找到，則會繼續往外層找，一直找到全局作用域為止



> **其實基本上整體概念跟上述的作用域鍊 (scopeChain) 中是一樣的，只是又分得更細了**
- 事實上，Lexical Environment（詞法作用域）可以分成三種類型，而 Environment Record（環境記錄）則可以分成五種類型。
- 而根據處理的變數類型，作用域其實可以分成 Lexical Environment（詞法作用域）和 Variable Environment（變數作用域）兩種。

但以上這些說明已經足夠讓我們去簡單理解閉包 (Closure) 的作用原理了，因此此處就不再贅述龐大的內容造成混淆，有興趣可以進一步詳讀 [結合 JavaScript 規範來談談 Execution Contexts 與 Lexical Environments](https://www.twblogs.net/a/5e5021e8bd9eee101e86c2de)  和 [ECMAScript 中是如何講解 environment record 和 executing context 的](https://www.bruceyj.com/front-end-interview-summary/front-end/JavaScript/6-environment-record.html#%E7%AE%80%E4%BB%8B) 文章。


### 2. Closure（閉包）的作用原理

- 靜態作用域其實更學術的名詞叫做 lexical scope，我有看過有人翻成語彙範疇，有人則是翻成詞法作用域
- 要理解什麼是 lexical，你必須先知道一點 compiler 的運作原理
- 在編譯的時候有幾個步驟是用程式去 parse 你的程式碼並且解析
    - 其中一個步驟就叫做 Lexical Analysis（詞法分析或語彙分析），其實就是去正確分析出程式碼裡面的每一個詞
- 之所以會叫做 lexical scope 的原因就是在編譯的時候其實就能決定作用域是什麼

> **Closure（閉包） 的發生場景**
- 若是內層函數使用了一個未宣告的變數，在編譯時就會透過 scopeChain 向外層一層一層尋找
    - 在某些程式語言中，如果函式回傳了，定義在其內部的區域變數就會消失，但在 JavaScript 並非如此
    - 在 JavaScript 中，即使在外層區塊已經回傳的狀況下，只要內層區塊還保留著一份「參考」，那麽外層區塊的環境不會隨著回傳而消失，我們依然可以存取外層環境中的變數
- 即使函式已經回傳了，只要還保留一個「參考」，那函式內的變數就不會消失，仍舊可以被存取
    - 閉包還有一個特性，每次函式被呼叫時，都會創造一組新的語彙環境 (Lexical Environment)。
    
直接來看一組程式碼比較清楚﹔

```
function makeAdder(x) {
function add(y) {
return x + y;
}
return add;
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

add5(2) //  7
add10(2) //  12
```
每次函式被呼叫時如 makeAdder(5) 和 makeAdder(10)，因為 closure 的特性，add5 和 add10 能夠記住宣告當下的語彙環境 (Lexical Environment)，包括變數 x，分別為 5 和 10。

### 3. Closure（閉包）的特性

- 在電腦科學中，閉包（英語：Closure）又稱詞法閉包（Lexical Closure）或函式閉包（function closures），是參照了自由變數的函式
- 這個被參照的自由變數將和這個函式一同存在，即使已經離開了創造它的環境也不例外
- 所以，有另一種說法認為「閉包」是由「函式」和「與其相關的參照環境」組合而成的實體

> **如果你認同閉包的定義是：「由函式和與其相關的參照環境組合而成的實體」，那就代表在 JavaScript 裡面所有的函式都是閉包**

因為這就是 JavaScript 的運行機制，你每個宣告的 function 都會儲存著一個 scopeChain 的資訊，而這個資訊裡面就是參照的環境。

> **而閉包有以下三點特性**
1. 若是內層函數使用了一個未宣告的變數，在編譯時就會透過 scopeChain 向外層一層一層尋找
2. 即使函式已經回傳了，只要還保留一個「參考」，那函式內的變數就不會消失，仍舊可以被存取
3. 每次函式被呼叫時，都會創造一組新的「語彙環境」 (Lexical Environment)

> 參考資料
1. [所有的函式都是閉包：談 JS 中的作用域與 Closure](https://blog.techbridge.cc/2018/12/08/javascript-closure/)
2. [Execution Context 和 Scope 概念有什麼區別？](https://www.zhihu.com/question/51336888?sort=created)
3. [來談談 Execution Contexts 與 Lexical Environments](https://blog.csdn.net/qq_35368183/article/details/103888311)
4. [重新認識 JavaScript: Day 19 閉包 Closure](https://ithelp.ithome.com.tw/articles/10193009)
5. [Day17_短短的 Scope2](https://ithelp.ithome.com.tw/articles/10196038)
6. [ECMAScript 中是如何講解 environment record 和 executing context 的](https://www.bruceyj.com/front-end-interview-summary/front-end/JavaScript/6-environment-record.html#%E7%AE%80%E4%BB%8B)
7. [JS-閉包 (Closure) 觀念整理](https://medium.com/chloelo925/js-%E9%96%89%E5%8C%85-closure-%E8%A7%80%E5%BF%B5%E6%95%B4%E7%90%86-346c32be3e30)
8. [JavaScript Closure (閉包)、函式與語彙環境](https://shubo.io/javascript-closure/)
9. [Day 28：閉包 (Closures)](https://something-about-js-book.onejar99.com/day28)
10. [JS 中 Scope Chain 和 outer environment 的概念](https://pjchender.blogspot.com/2015/12/javascriptscope-chainouter-environment.html)

---

## 六、舉出一個運用 Closure 的例子

直接先秀兩段程式碼，讓我們快速看看Closure（閉包）的實際應用例子


程式碼 1
```
function test() {
var a = 10;
function inner() {
a++;
console.log(a);
}
return inner // 我們把 inner 這個 func 給 return 回去。
}
test();
var func = test() // 所以這邊就要有個東西來接這個 func，所以這個 func 執行以後它會回傳一個 func
func() // 所以你有了那個 func 以後，你這邊就可以再呼叫一個 func
func() // 再呼叫（call）一次 變 12
func() // 再呼叫（call）一次 變 13
```
程式碼 2 
```
var i = 0;
function complex(num) {
console.log('我現在 calculate 了',i,'次')
i = i+1;
return num * num * num //假設他乘了一百次好了，是很複雜的運算。
}
console.log(complex(20))
console.log(complex(20))
console.log(complex(20)) //我每 Call 一次就要做複雜的運算。可是輸入都跟輸出一樣嘛，不如我們寫個 function 改一下！
```
程式碼 2 又可以優化如下
```
var i = 0;
function complex(num) {
console.log('我現在 calculate 了',i,'次')
i = i+1;
return num * num * num //假設他乘了一百次好了，是很複雜的運算。
}
function cache(func) {
var ans = {};
return function(num) {
if (ans[num]) {
return ans[num]; //如果已經有答案，就 return 這個東西
}
ans[num] = func(num); //如果沒有的話，
return ans[num]; //最後再把這個東西給傳回去
}
}
const cachedComplex = cache(complex) //你想要把這個 function 給傳進去，那這個 funct 就會變成一個新的 funct
console.log(cachedComplex(20)) // 計算
console.log(cachedComplex(20)) // 呼叫第二次時，這邊就會直接輸出結果，因為你前面已經計算過了
console.log(cachedComplex(20))
// 他會幫我把輸入的值給記錄起來，如果我已經有輸入過這個值了，我就可以直接輸出結果，而不是重新算一遍。
```

以上的例子主要是利用閉包能夠把變數的值持續保存的特性，將重複的複雜計算，簡單化，如果一旦計算量大的話，有做這個處理一來一往之間就能節省很大量的運算時間和記憶體空間的浪費。


### 1. IIFE（立即呼叫函式表達式）

IFE本身就是一種運用閉包與匿名函式立即執行的樣式。

> **它的常見基本語法（實際有很多種寫法）有下面這兩種**
```
(function(){ ... }())
```

```
(function(){ ... })()
```

IIFE 是一種會在建立時就會立即執行的匿名函式，經常用於封閉住一個作用域，避免與全域作用域污染。

一個簡單的 IIFE 範例如下，counter 是一個會鎖住函式裡面的變數值的閉包，這個樣式通常會用來模擬靜態變數。

```
const counter = (function() {
let i = 1
return function() {
console.log(i++)
}}())
counter() //1
counter() //2
```

> 參考資料: [Closure 閉包](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/closure.html)

---

## 七、總結

> 『那些最浪漫的話語，都藏在我心最底等你一層一層來剝開。』