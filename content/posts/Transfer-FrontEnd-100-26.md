---
title: "百日轉職前端工程師：第十六週 JavaScript物件導向《DAY 26》"
slug: Transfer-FrontEnd-100-26
date: 2021-01-20T00:29:21+08:00
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

大家好，這是百日轉職前端工程師的 Day26，也是 2021/02/19（二），這週的主題會 JavaScript 最難但也是最核心，也是面試最常考的部分之一，物件導向。但這也是我覺得程式語言最有趣的部分，很多的程式語言都有物件導向的設計，因此我們可以這麼假設，物件導向，可能幫我們解決了很多大麻煩，導致它如此的無可或缺。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、 Prototype 在 JavaScript 裡是什麼？

> **要談到 Prototype，不可避免的我們就至少要簡單聊到物件導向的概念**

- 因為所謂的 Prototype 就是「原型」
- 在 JavaScript 中，只要是函式上面都有預設有一個公開的 Prototype 屬性
- 當我們將一個 Function 當作建構式使用的時候，所有被此建構式新建出來的物件都可以透過  Prototype  參考連結到這個原型物件上來存取其上的屬性


而因為因為每個同子型別物件都能存取其原型物件的屬性，就能讓這些物件看起來擁有相同的功能，也就可以建立出類似「物件導向」概念的程式了。這也是因為 JavaScript 並不像 Java、C++ 這些知名的物件導向語言具有「類別」（class）來區分概念與實體（instance）或天生具有繼承的能力，而只有「物件」，因此只能利用 Prototype 等設計模式來模擬這些功能。


> **Prototype 能讓我們寫出能夠讓多個「物件」共用的函式**

但在繼續說下去前，我們應該先解釋什麼是「物件」跟物件導向（OOP）對吧？

### 1. 淺談一下物件導向

> **在程式中，我們常以物件表達「真實世界的概念」，物件本身的組成是由一個 「屬性」(property) 和「值」(value) 組成**

- 「物件導向」程式設計中的每一個物件都應該能夠接受資料、處理資料並將資料傳達給其它物件
- 因此它們都可以被看作一個小型的「機器」，或者說是負有責任的角色
- 但 「物件」的概念卻往往很難被界定與定義
    - 具體的東西，如電腦、小狗、汽車、杯子、Xbox 360 …等，都是物件
    - 但抽象的概念，如訂房、會議、訂購、保險、行動電話的簡訊 …等，也都可以是物件
- 舉個例子說明，物件（Object）是類別（Class）的實例（instance）
    1. 類別: 「狗」這個類別列舉狗的特點，從而使這個類別定義了世界上所有的狗。
    2. 物件: 而萊絲這個物件則是一條具體的狗，它的屬性也是具體的，狗有皮毛顏色，而萊絲的皮毛顏色是棕白色的
    3. 實例: 因此，萊絲就是狗這個類別（Class）的一個實例（instance）


> **物件導向設計的優缺點**
- 使用物件導向設計的「優點」
    - 便於程式碼「重複使用」
    - 把程式細節隱藏在物件內
    - 讓主程式能變短，且簡化主程式邏輯
- 使用物件導向設計的「缺點」
    - 「基礎建設」較繁雜龐大
    - 寫個簡單程式需要比傳統寫法，還要更多行


## 2. JavaScript 是原型基礎的物件導向程式語言

- JavaScript 是屬於原型基礎 (Prototype-based) 的物件導向程式語言
- 其不強調「類別」與「實例」之間的差異，「類別」事實上就是「物件」
- 在 JavaScript 裡使用物件並不需事先設定類別即可直接建立
- 甚至到 ES6 標準制定後仍沒變動過，在物件的章節中所介紹的類別定義方式，只是原型物件導向語法的語法糖，骨子裡還是原型，並不是真正的以類型為基礎的物件導向設計
- 各同樣子型別的物件都會以 Prototype 連結到同一個「原型」物件
- 也就是說，只要我們為這個原型物件增加屬性，則所有該子型別的物件都可以取用到這個函式，這就是所謂的共用函式
- 呼應前面說的，Prototype 能讓我們寫出能夠讓多個「物件」共用的函式

那就讓我們看點程式碼吧：


```
function Dog(name) {
this.name = name;
}

Dog.prototype.speak = function() {
console.log('Bark');
};

Dog.prototype.move = function() {
console.log('walk');
};

var dog1 = new Dog('Blacky'),
dog2 = new Dog('Whity');

dog1.speak(); // "Bark"
dog2.speak(); // "Bark"
Dog.prototype.speak = function() {
console.log('Bow-wow');
};
dog1.speak(); // "Bow-wow"
dog2.speak(); // "Bow-wow"
```

是什麼讓 dog1.speak() 知道要去上面的 Prototype 找呢？答案如下。

### 3. 透過 Prototype 和 proto 實現原型鏈（Prototype Chain）

> **於是這邊我們要介紹一個跟 Prototype 一組的重要語法「proto」**

- .prototype
    - 用來實現基於「原型」的繼承與屬性的共享
    - ex: 用來指定屬性或 function
- .proto
    - 構成「原型鏈」，同樣用於實現基於「原型」的繼承
    - ex: 繼承資料

> **如果你在 dog1 身上找不到 speak 的話，你應該去哪裡找？答案就是透過 dog1.proto**
- 就去 dog1.proto 這裡找
    - 而其實其實 dog1.proto 就是 Dog.prototype
    - 若是還是找不到就會往再上一層找，直到找到 Object.prototype 為止還是沒有，那輸出就會是 null。
- 透過 Prototype 這樣的方式，將他底下的東西可以用 .proto 連起來，讓他們可以共同享有同一個 Function，這又被稱為「原型串鏈」（Prototype Chain）。
- 至此，我們已經學會了如何使用 JavaScript 的 Prototype 寫出類似「物件導向」的程式
- ES6 的語法糖
    - 雖然如此，但是 Prototype 的繼承寫法會讓人無法一目了然
    - 所以在 ES6 中允許我們以「語法糖」的概念去用更貼近物件導向的語法來寫
    - 雖然基底依然還是 Prototype，但可以有效增加開發速度以及易讀性




> 參考資料
1. [JavaScript 中的 function constructor 和關鍵字 new](https://pjchender.blogspot.com/2016/06/javascriptfunction-constructornew.html)
2. [前端中階：JS令人搞不懂的地方-物件導向](https://hugh-program-learning-diary-js.medium.com/%E5%89%8D%E7%AB%AF%E4%B8%AD%E9%9A%8E-js%E4%BB%A4%E4%BA%BA%E6%90%9E%E4%B8%8D%E6%87%82%E7%9A%84%E5%9C%B0%E6%96%B9-%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91-cdea0e3266ee)
3. [Javascript 物件導向設計](https://github.com/nodejs-tw/nodejs-wiki-book/blob/master/zh-tw/js-OO-pattern.rst#javascript%E9%A1%9E%E5%88%A5%E5%AF%A6%E4%BD%9C)
4. [Javascript 物件導向設計](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/prototype.html)
5. [JavaScript 物件導向白話文筆記——全端開發者內功 I](https://ithelp.ithome.com.tw/articles/10250999)
6. [你懂 JavaScript 嗎？#19 原型（Prototype）](https://ithelp.ithome.com.tw/articles/10205313)
7. [JavaScript 入門指南/建構式的原型：Constrctor.prototype](https://javascript.alphacamp.co/constrctor-prototype.html)
8. [JavaScript 物件導向 (4) - With ES6](https://ithelp.ithome.com.tw/articles/10196867)
9. [JavaScript ES6 Map and WeakMap Object 物件](https://www.fooish.com/javascript/ES6/Map-and-WeakMap.html)

---

## 二、大部分情況下 this 的值是什麼？

要談 this，也要從「物件導向」開始談，然而弔詭的是 this 真的如此複雜讓很多人混淆的關鍵就是因為在物件以外的地方也可以用 this，但其本質上是沒有太大意義的，就只是可以用而已，這在後續會陸續說明。

> **在物件導向中的 this，代表的就是在物件導向裡面，那個實例 (instance) 本身**

舉個例子：

```
class Car {
setName(name) {
this.name = name
}
getName() {
return this.name
}
}
const myCar = new Car()
myCar.setName('hello')
console.log(myCar.getName()) // hello
```

> **物件導向的世界裡面，this 的作用**
- 在上面我們宣告了一個 class Car，寫了 setName 跟 getName 兩個方法
- 在裡面用 this.name 來存取這個 instance 的屬性
- 為什麼要這樣寫？因為這是唯一的方法，不然你要把 name 這個屬性存在哪裡？沒有其他地方讓你存了
- 所以 this 的作用在這裡是顯而易見的，所指到的對象就是那個 instance 本身
- 以上面的範例來說，myCar.setName('hello')，所以 this 就會是myCar
- 在物件導向的世界裡面，this 的作用就是這麼單純

> **但在 JavaScript 中 this 之所以那麼難懂，就是因為在 JavaScript 裡面，你在任何地方都可以存取到 this**

所以在 JavaScript 裡的 this 跟其他程式語言慣用的那個 this 有了差異，脫離了物件 this 的值就沒什麼意義，但他還是會呈現以下特徵
1. 嚴格模式底下就都是 undefined
2. 非嚴格模式，瀏覽器底下是 window
3. 非嚴格模式，node.js 底下是 global

### 1. 如何去更改 this 的值？

> **僅管 this 可能有預設的值，但我們可以透過一些方法來改它**
這改的方法也很簡單，一共有三種:
1. call
2. apply
3. bind

前兩種超級類似，叫做 call 跟 apply，這兩種都是能夠呼叫 fucntion 的函式，舉一個例子比較好懂

```
'use strict';
function hello(a, b){
console.log(this, a, b)
}
hello(1, 2) // undefined 1 2
hello.call('yo', 1, 2) // yo 1 2
hello.apply('hihihi', [1, 2]) // hihihi 1 2
```


- 上面三兩種呼叫 function 的方式是等價的，一模一樣
- call 跟 apply 傳進的第一個參數的值就是 this
- 而 apply 的差別只在於他後面要傳進去的參數是一個 array
- 除了直接呼叫 function 以外，你也可以用 call 或是 apply 去呼叫，差別在於傳參數的方式不同


除了以上兩種以外，還有最後一種可以改變 this 的方法：bind

```
'use strict';
function hello() {
console.log(this)
}
const myHello = hello.bind('my')
myHello() // my
```

你可能會好奇如果我們把 call 跟 bind 同時用會怎樣？答案是不會改變，一但 bind 了以後值就不會改變了，這邊還要特別提醒的一點是在非嚴格模式底下，無論是用 call、apply 還是 bind，你傳進去的如果是 primitive 都會被轉成 object。



### 2. 物件中的 this

> **最前面我們示範了在物件導向 class 裡面的 this，但在 JavaScript 裡面還有另外一種方式也是物件**













> 參考資料
1. [[JavaScript]使用建構器創造實體物件](https://ithelp.ithome.com.tw/articles/10198141)
2. [從ES6開始的JavaScript學習生活 /物件](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part3/object.html?q=)
