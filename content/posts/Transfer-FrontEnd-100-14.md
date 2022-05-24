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
- 當我們在寫 JavaScript 時，你的程式碼並不會神奇的直接告訴電腦該做什麼，而是有人寫了一個程式，這個程式會一個字一個字的讀你的程式碼，並且判斷語法是否有效，最後轉換你的 JavaScript 成為電腦看得懂的東西，這些程式稱為編譯器 (Compilers)。你的電腦真正執行的是你所寫出來的程式碼被「轉譯」過的語言。

2. 詞彙環境 Lexical Environment
-  詞彙環境存在於一些程式語言中，這些程式語言認為程式碼寫在哪裡是很重要的。它必須以正確的格式和位置，才能被 語法解析器 ( Syntax Parser ) 讀懂並轉譯，特定的程式寫在哪裡是很重要的，例如：當我們撰寫出一段宣告變數的 JS 語法，它被寫在哪呢?它在一個函數中嗎?它的周圍的環境是什麼?它被包在物件、陣列中嗎? 詳細圖解可參考 JavaScript 基礎二三事系列 Day02 - 幾個名詞小觀念。

3. 執行環境 Execution Context
- 我們有許多的「詞彙環境 （Lexical Environment）」，程式碼實際上所在的位置，但哪個才是正在執行的？就是被執行環境（Execution Context）所管理。

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

- DOM 全名為 Document Object Model，是一種介面的定義
- 根據 W3C DOM 規範，DOM 是一種與瀏覽器，平台，語言的接口
- 簡單來說它能把一份 HTML 文件內的各個標籤，包括文字、圖片等等都定義成「物件」
- 而這些物件最終會形成一個樹狀結構，提供瀏覽器跟 JS 溝通的橋樑
- 在 JS 上可以運用各種 DOM API 去操作 DOM 實現網頁功能
- DOM API 定義了讓 JavaScript 可以存取、改變 HTML/XML 架構、樣式和內容的方法，甚至是對節點綁定的事件
- DOM 最常被用以與 JavaScript 溝通，也就是說雖然程式以 JavaScript 寫成，但使用 DOM 來存取頁面及其元素
- DOM 其實可以與任何程式語言共同運作。一個網頁的所有元素組織在一起，就構成了一棵「DOM 樹」。

在 DOM 中，每個 元素 (element) 、 文字 (text) 等等都是一個節點。

> **而節點通常分成以下四種（可參考上篇 前端基礎 HTML 做知識的連結）**

1. Document: Document 就是指這份文件，也就是這份 HTML 檔的開端，所有的一切都會從 Document 開始往下進行
2. Element: Element 就是指文件內的各個標籤
3. Text: Text 就是指被各個標籤包起來的文字
4. Attribute: Attribute 就是指各個標籤內的相關屬性

---
## 三、 用 JavaScript 操控 DOM 物件：DOM API

> **JavaScript 透過 DOM 提供的 API 來 對 HTML 做存取與操作，常用的有以下**

- 選取標籤的元素
    - getElementsByTagName()
    - document.getElementsByTagName('div')
    - document.getElementsByTagName('div')[0]
- 選取 class 的元素
    - getElementsByClassName()
    - document.getElementsByClassName('first')
    - document.getElementsByClassName('greet')[0]
    - document.getElementsByClassName('greet english')
- 選取 id 的元素
    - getElementById()
- 選取屬性（CSS 形式）的元素
    - querySelector() (document.querySelector('.greet'))
    - querySelectorAll()
- 改變 style
    - HTMLElement.style['padding-top'] = '20px';
    - HTMLElement.style.paddingTop = '20px';
    - HTMLElement.style.cssText = "color: blue; border: 1px solid black";
- 改變 Class
    - 加上：classList.add()
    - 移除：classList.remove()
    - 切換：classList.toggle()
    - 判斷是否包含：classList.contains()
- 改變屬性（attribute）
    - setAttribute (ex: e.setAttribute('href','http://www.yahoo.com.tw'))
    - getAttribute (ex: e.getAttribute('href'))
- 更動 Element
    - 改變內元素內容：innerText (ex: HTMLElement.innerText = 'buzz')
    - 改變內元素 HTML：innerHTML (ex: HTMLElement.innerHTML = '<h2>buzz</h2>')
    - 重新改變元素 HTML：outerHTML (ex: HTMLElement.outerHTML = '<h2>buzz</h2>')
    - 刪除元素：removechild() (HTMLElement.removechild(document.querySelector('a')))
    - 增加元素：appendChild()
- 監聽事件
    - addEventListener()：(ex: element.addEventListener('type', function, useCapture))
    - 事件類型 (type)：click 、 keydown、keypress......等（詳見 MDN）


    > 參考資料: [DOM— 操作 DOM 介面](https://miahsuwork.medium.com/%E7%AC%AC%E5%85%AB%E9%80%B1-dom-%E6%93%8D%E4%BD%9C-dom-%E4%BB%8B%E9%9D%A2-523c135b4fc1)

    ---

    ## 四、如何幫一個按鈕加上 event listener

    假設按鈕本身是 `<input>` 標籤中的 submit 以此為舉例，可以先為其加上 btn 屬性，整個元件長成 `<input type="submit" class="btn">` 因此為其加上事件監聽器 (event listener) 的程式碼如下

    ```
    document.querySelector(.btn).addEventListener('submit', (e) -> {事件觸發內容的程式碼} )
    ```

    如果是想要直接在點到整個網頁中的按鈕就觸發可以用 
    ```
    window.addEventListener('submit', (e) -> {事件觸發內容的程式碼} )

    ```
    ### 1. 事件資訊 Event(e)

    addEventListener 其中的會內建一個「事件物件」，內含有你執行的動作的事件資訊，例如：click 事件就會包含點擊的區塊、位置….等。你可以自由為其命名，例如上述即命名為「e」。


    ### 2. 常見的事件資訊

- click 事件：點擊 DOM 時觸發
    - e.target：點擊到的元素。
    - e.screenX：滑鼠離視窗左邊的距離。
    - e.screenY：滑鼠離視窗右邊的距離。
- keydown 事件：按下鍵盤時觸發
    - e.key：按鍵號碼。
- 阻止事件
    - e.preventDefault()：阻止預設行為。
    - e.stopPropagation()：阻止事件傳遞。
    - e.stopImmediatePropagation()：有多個事件監聽器情況下，同一層級的監聽都不要被執行。
- 其他
    - e.eventPhase：提供觸發事件的階段資訊，分為捕捉、目標、冒泡。


---

## 五、捕獲與冒泡是什麼？

- JavaScript 是一個事件驅動 (Event-driven) 的程式語言
- 當瀏覽器載入網頁開始讀取後，雖然馬上會讀取 JavaScript 事件相關的程式碼
- 但是必須等到 「事件」被觸發(如使用者點擊、按下鍵盤等)後 ，才會觸發執行的動作執行對應的程式
- 然而假使當你「點擊」 DOM 的內層時，實際上你連包覆內層的外層也點到了
- 你的動作實際上可以視為也是點擊外層的一事件
- 而這個事件為根據「事件傳遞機制」的概念一層一層往內傳到你實際上點擊的內層
- 這個過程中的概念有 DOM 事件傳遞機制中的「捕獲」與「冒泡」


> **事件傳遞機制總共分為三大階段**

1. 捕獲階段 (Capture Phase)
    - 在捕獲階段，DOM 的事件會從祖先層 (window) 開始往下尋找目標 (target)，這個過稱稱為捕獲階段 (CAPTURING_PHASE)。
2. 目標階段 (Target Phase)
    - 這時候在目標 (target) 身上所加的事件監聽器 (eventListenr) 會是 AT_TARGET 這一個階段 (Phase)。（當事件傳到 target 本身，沒有分捕獲跟冒泡）
3. 冒泡階段 (Bubbling Phase)
    -  循著原路回去的過程，就是冒泡階段 (BUBBLING_PHASE)。


至於孰先孰後，有句名言是這麼說的：

> “ 先捕獲，再冒泡 ”

- eventListenr 的第三個參數 (Parameter) 設 true 則將 eventListenr 設置在捕獲階段 (Capture Phase)
- eventListenr 的第三個參數 (Parameter) 設 false 則將 eventListenr 設置在冒泡階段 (Bubbling Phase)
- eventListenr 的第三個參數 (Parameter) 預設為 false

---

## 六、事件代理（delegation）

- 情境 1：假設有 100 個元素就要綁定 100 個事件，裝一百個事件監聽器？
- 情境 2：後續動態幫網頁新增的元素就無法事先綁定事件，怎辦？

別擔心！以上問題都可以透過事件代理解決，事件代理就像是底下一百個子弟兵的法定代理人，可以把相似任務統一綁在它「父元素」身上。

事件代理 (event delagation) 是運用事件傳遞機制的特性，將事件監聽器 (eventListenr) 加在目標元件的上層元素，若是你同時有多個元件都希望裝上同樣的事件監聽器 (eventListenr) ，也就是功能類似的話，透過事件代理可以大幅將程式碼簡化，有點像是統一把一樣的任務委託給上層的一個元素執行的概念。

---

## 七、 preventDefault 與 stopPropagation 的差異

> **e.preventDefault 與 e.stopPropagation 的差別**
- e.preventDefault 就只是取消預設行為，跟事件傳遞沒有任何關係
- e.stopPropagation  則是讓事件不再往下傳遞

### 1. event.preventDefault

有些元素會有屬於他的預設行為，透過 event.preventDefault 可以取消元素的預設行為，比如說點擊超連結會自動帶你到另一個網頁，若加上 event.preventDefault 可以取消此預設行為。（並不會阻止事件向上傳遞「事件冒泡」行為）

### 2. event.stopPropagation


DOM 在進行捕獲及冒泡行為時，若在某個元素監聽事件加上 event.stopPropagation 後，會停止捕獲 /冒泡行為，若你希望不要觸發點擊內層之外的外層 eventListener，用在只想單純針對單一元素監聽，不想去觸發到其他元素的情境。（加在哪邊，事件的傳遞就斷在那裡，不會繼續往下傳遞）。

---

## 八、總結

> 勇者呀！拿到 JS 之劍，你才終於第一步真正踏入了挑戰魔王的殿堂。