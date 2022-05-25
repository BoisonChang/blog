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
