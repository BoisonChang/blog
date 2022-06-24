---
title: 百日轉職前端工程師：第十六週 JavaScript物件導向《DAY 26》
date: 2021-02-20T00:29:21+08:00
slug: Transfer-FrontEnd-100-26
draft: false
tags:
  - FrontEnd
  - Transfer
keywords:
  - FrontEnd
  - Transfer
author: Boison
---
大家好，這是百日轉職前端工程師的 Day26，也是 2021/02/19（二），這週的主題會 JavaScript 最難但也是最核心，也是面試最常考的部分之一，物件導向。但這也是我覺得程式語言最有趣的部分，很多的程式語言都有物件導向的設計，因此我們可以這麼假設，物件導向，可能幫我們解決了很多大麻煩，導致它如此的無可或缺。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

- - -

## 一、 Prototype 在 JavaScript 裡是什麼？

> **要談到 Prototype，不可避免的我們就至少要簡單聊到物件導向的概念**

* 因為所謂的 Prototype 就是「原型」
* 在 JavaScript 中，只要是函式上面都有預設有一個公開的 Prototype 屬性
* 當我們將一個 Function 當作建構式使用的時候，所有被此建構式新建出來的物件都可以透過  Prototype  參考連結到這個原型物件上來存取其上的屬性

而因為因為每個同子型別物件都能存取其原型物件的屬性，就能讓這些物件看起來擁有相同的功能，也就可以建立出類似「物件導向」概念的程式了。這也是因為 JavaScript 並不像 Java、C++ 這些知名的物件導向語言具有「類別」（class）來區分概念與實體（instance）或天生具有繼承的能力，而只有「物件」，因此只能利用 Prototype 等設計模式來模擬這些功能。

> **Prototype 能讓我們寫出能夠讓多個「物件」共用的函式**

但在繼續說下去前，我們應該先解釋什麼是「物件」跟物件導向（OOP）對吧？

### 1. 淺談一下物件導向

> **在程式中，我們常以物件表達「真實世界的概念」，物件本身的組成是由一個 「屬性」(property) 和「值」(value) 組成**

* 「物件導向」程式設計中的每一個物件都應該能夠接受資料、處理資料並將資料傳達給其它物件
* 因此它們都可以被看作一個小型的「機器」，或者說是負有責任的角色
* 但 「物件」的概念卻往往很難被界定與定義

  * 具體的東西，如電腦、小狗、汽車、杯子、Xbox 360 …等，都是物件
  * 但抽象的概念，如訂房、會議、訂購、保險、行動電話的簡訊 …等，也都可以是物件
* 舉個例子說明，物件（Object）是類別（Class）的實例（instance）

  1. 類別 

     * 「狗」這個類別列舉狗的特點，從而使這個類別定義了世界上所有的狗。
  2. 物件

     * 而萊絲這個物件則是一條具體的狗，它的屬性也是具體的，狗有皮毛顏色，而萊絲的皮毛顏色是棕白色的
  3. 實例

     * 因此，萊絲就是狗這個類別（Class）的一個實例（instance）

> **物件導向設計的優缺點**

1. 使用物件導向設計的「優點」

* 便於程式碼「重複使用」
* 把程式細節隱藏在物件內
* 讓主程式能變短，且簡化主程式邏輯

2. 使用物件導向設計的「缺點」

* 「基礎建設」較繁雜龐大
* 寫個簡單程式需要比傳統寫法，還要更多行

## 2. JavaScript 是原型基礎的物件導向程式語言

> JavaScript 是屬於原型基礎 (Prototype-based) 的物件導向程式語言

* JavaScript 不強調「類別」與「實例」之間的差異，「類別」事實上就是「物件」
* 在 JavaScript 裡使用物件並不需事先設定類別即可直接建立

> JavaScript 骨子裡還是原型，並不是真正的以類型為基礎的物件導向設計

* 甚至到 ES6 標準制定後仍沒變動過，在物件的章節中所介紹的類別定義方式，只是原型物件導向語法的語法糖
* 骨子裡還是原型，並不是真正的以類型為基礎的物件導向設計

> JavaScript 中各同樣子型別的物件都會以 Prototype 連結到同一個「原型」物件

* 也就是說，只要我們為這個原型物件增加屬性，則所有該子型別的物件都可以取用到這個函式，這就是所謂的共用函式
* 呼應前面說的，Prototype 能讓我們寫出能夠讓多個「物件」共用的函式

那就讓我們看點程式碼吧：

```
function Dog(name) {
this.name = name
}

Dog.prototype.speak = function() {
console.log('Bark')
}

Dog.prototype.move = function() {
console.log('walk')
}

var dog1 = new Dog('Blacky'),
var dog2 = new Dog('Whity')

dog1.speak() // "Bark"
dog2.speak() // "Bark"
Dog.prototype.speak = function() {
console.log('Bow-wow')
};
dog1.speak() // "Bow-wow"
dog2.speak() // "Bow-wow"
```

是什麼讓 dog1.speak() 知道要去上面的 Prototype 找呢？答案如下。

### 3. 透過 Prototype 和 proto 實現原型鏈（Prototype Chain）

> **於是這邊我們要介紹一個跟 Prototype 一組的重要語法「proto」**

* .prototype

  * 用來實現基於「原型」的繼承與屬性的共享
  * ex: 用來指定屬性或 function
* .proto

  * 構成「原型鏈」，同樣用於實現基於「原型」的繼承
  * ex: 繼承資料

> **如果你在 dog1 身上找不到 speak 的話，你應該去哪裡找？答案就是透過 dog1.proto**

* 就去 dog1.proto 這裡找

  * 而其實其實 dog1.proto 就是 Dog.prototype
  * 若是還是找不到就會往再上一層找，直到找到 Object.prototype 為止還是沒有，那輸出就會是 null。
* 透過 Prototype 這樣的方式，將他底下的東西可以用 .proto 連起來

  * 透過 Prototype 這樣的方式，將他底下的東西可以用 .proto 連起來，讓他們可以共同享有同一個 Function，這又被稱為「原型串鏈」（Prototype Chain）。
  * 至此，我們已經學會了如何使用 JavaScript 的 Prototype 寫出類似「物件導向」的程式
* ES6 的語法糖

  * 雖然如此，但是 Prototype 的繼承寫法會讓人無法一目了然
  * 所以在 ES6 中允許我們以「語法糖」的概念去用更貼近物件導向的語法來寫
  * 雖然基底依然還是 Prototype，但可以有效增加開發速度以及易讀性

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

- - -

## 二、大部分情況下 this 的值是什麼？

要談 this，也要從「物件導向」開始談，然而弔詭的是 this 真的如此複雜讓很多人混淆的關鍵就是因為**在物件以外的地方也可以用 this**，但其本質上是沒有太大意義的，就只是可以用而已，這在後續會陸續說明。

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

* 在上面我們宣告了一個 class Car，寫了 setName 跟 getName 兩個方法
* 在裡面用 this.name 來存取這個 instance 的屬性
* 為什麼要這樣寫？因為這是唯一的方法，不然你要把 name 這個屬性存在哪裡？沒有其他地方讓你存了
* 所以 this 的作用在這裡是顯而易見的，所指到的對象就是那個 instance 本身
* 以上面的範例來說，myCar.setName('hello')，所以 this 就會是myCar
* 在物件導向的世界裡面，this 的作用就是這麼單純

> **JavaScript 中 this 之所以那麼難懂，正因為在 JavaScript 中任何地方都可以存取到 this**

所以在 JavaScript 裡的 this 跟其他程式語言慣用的那個 this 有了差異，脫離了物件 this 的值就沒什麼意義，但他還是會呈現以下特徵

1. 嚴格模式 : undefined
2. 非嚴格模式/瀏覽器 :  window
3. 非嚴格模式/node.js : global

### 1. 如何去更改 this 的值？

> **僅管 this 可能有預設的值，但我們可以透過一些方法來改它**

1. call
2. apply
3. bind

前兩種超級類似，叫做 call 跟 apply，這兩種都是能夠呼叫 fucntion 的函式，舉一個例子比較好懂

```
'use strict'
function hello(a, b){
console.log(this, a, b)
}
hello(1, 2) // undefined 1 2
hello.call('yo', 1, 2) // yo 1 2
hello.apply('hihihi', [1, 2]) // hihihi 1 2
```

* 上面兩種呼叫 function 的方式是等價的，一模一樣

  * call 跟 apply 傳進的第一個參數的值就是 this
  * 而 apply 的差別只在於他後面要傳進去的參數是一個 array

用 call 或是 apply 去呼叫，跟直接呼叫 function的差別，在於傳參數的方式不同。除了以上兩種以外，還有最後一種可以改變 this 的方法：bind

```
'use strict'
function hello() {
console.log(this)
}
const myHello = hello.bind('my')
myHello() // my
```

> 你可能會好奇如果我們把 call 跟 bind 同時用會怎樣？

答案是不會改變，一但 bind 了以後值就不會改變了，這邊還要特別提醒的一點是在非嚴格模式底下，無論是用 call、apply 還是 bind，你傳進去的如果是 primitive 都會被轉成 object。

### 2. 物件中的 this

> **最前面我們示範了在物件導向 class 裡面的 this，但在 JavaScript 裡面還有另外一種方式也是物件**

```
const obj = {
value: 1,
hello: function() {
console.log(this.value)
}
}
obj.hello() // 1
```

這就是單純的創建了一個物件，而不是使用物件導向的設計，先再複習一個重要的觀念「**this 的值跟作用域跟程式碼的位置在哪裡完全無關，只跟『你如何呼叫』有關**」。

```
const obj = {
value: 1,
hello: function() {
console.log(this.value)
}
}
obj.hello() // 1
const hey = obj.hello
hey() // undefined
```

> 同一個函式，怎麼第一次呼叫時 this.value 是 1，第二次呼叫時就變成 undefined 了？

記住：「要看 this，就看這個函式『怎麽』被呼叫」。

> **再引入一個新的觀念，過去我們習以為常的呼叫 function 其實是一種語法糖**

* 比方說上述的 obj.hello()，他真正的樣貌其實是等於 obj.hello.call(obj)
* 所以 hello 的 this 被改成 obj，會得到 console.log(obj.value) 為 1

> 那既然如此過去我們呼叫 function 時為何都沒碰到這個概念呢？

* 其實有的，只是我們預設傳入了一個 undefined 將 this 改成 undefined。

### 3. 箭頭函式中的 this

> **ES6 新增的箭頭函式中有不太一樣的運作方式，它本身並沒有 this**

* 所以「**在宣告它的地方的 this 是什麼，它的 this 就是什麼**」。
* 簡單來說，說箭頭函式的 this 不是自己決定的，而是取決於在宣告時那個地方的 this

> 參考資料
>
> 1. [JavaScript使用建構器創造實體物件](https://ithelp.ithome.com.tw/articles/10198141)
> 2. [從ES6開始的JavaScript學習生活 /物件](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part3/object.html?q=)

- - -

## 三、物件導向的基本概念（類別、實體、繼承、封裝）

最後，我們終於要來好好聊聊物件導向了！由於物件導向篇幅眾多，我會盡可能簡潔的說明，但是最大幅度的提高看倌對於物件導向的掌握程度。

首先在講物件導向之前，我們先講其在多數地方最主要的幾個概念，但在 JavaScript 中可能並不能完全套用。

> ***但這不妨礙我們先從更高的視野先去瞭解一下這個在程式領域重要的概念：「物件導向」(Object Oriented Programming)***

OOP（物件導向） 總共有四大支柱，分別是：

1. **Encapsulation（封裝）**

* Encapsulation 這個概念是在說，我們可以把許多屬性、方法包裝成一個物件使用把 OOP 與 Procedural programming
* Encapsulation 的優點在於參數很少或沒有，降低了程式的複雜性與提供了靈活性

2. **Abstraction（抽象）**

* Abstraction 指得是將此物件的某些屬性與方法隱藏（hide）起來
* 舉例來說，我們按下遙控器的按鈕就能轉台，其實這是靠遙控器內的小零件互相作用的，但我們不必知道這些
* 這些被隱藏在遙控器裡的零件與作用就像是被隱藏起來的屬性與方法
* Abstraction 的優點

  1. 使物件的介面更簡單
  2. 減少改變的影響

3. **Inheritance（繼承）**

* Inheritance 的概念就是繼承者擁有某些屬性或是方法，而這些是來自於被繼承者的
* 使繼承者透過 Inheritance 可以直接取用這些屬性與方法

4. **Polymorphism（多型）**

* Polymorphism 指的是使用相同名稱的方法，傳入不同的參數，會執行不同的指令
* 假如我們要讓好多個物件渲染（render）頁面，在 Procedural programming 就會必須用好多 switch 和 case
* 抽象講法解釋，就是使用單一介面操作多種型態的物件，繼承父類別
* 定義與父類別中相同的方法，但實作內容不同，稱為「覆寫 (override)」

  * 覆寫是指子類別可以覆寫父類別的方法內容，使該方法擁有不同於父類別的行為
* 多型 (Polymorphism) 是指父類別可透過子類別衍伸成多種型態

  * 而父類別為子類別的通用型態，再透過子類別可覆寫父類別的方法來達到多型的效果，也就是同樣的方法名稱會有多種行為

JS 是用原型繼承的方式實作物件導向繼承的抽象概念，JS 要怎麼實作呢，如以下以 prototype 實現此概念

```
function Role(name, blood){
this.name = name || ""
this.blood = blood || ""
}
function SwordMan(name, blood){
Role.call(this, name, blood)
this.fight = "揮劍攻擊"
}
function Magician(name, blood){
Role.call(this, name, blood)
this.fight = "火球術！"
this.cure = "治療！"
}
SwordMan.prototype = new Role()
Magician.prototype = new Role()
var sword = new SwordMan("劍士",200)
var magic = new SwordMan("魔法師",100)
```

可以看到說，雖然 sword 與 magic 都有 name 與 blood ，但會發現顯示出來的不一樣，這是因為我們繼承了Role 所以在覆寫時候才能顯示不一樣，而不是只會統一顯示。

前面有提過 JavaScript 是屬於原型基礎 (Prototype-based) 的物件導向程式語言，其不強調類別與實例之間的差異，「類別」事實上就是「物件」，在 JavaScript 裡使用物件並不需事先設定類別即可直接建立，ES6 有新增 Class 的語法，但也是算是語法糖。

### 1 . 物件導向的類別和實體（實例）

* 實際上，在 JavaScript 中，除了沒有類別外，其實也沒有建構子
* 一個 Function 是不是建構式並不是取決於它的宣告方式
* 如果是用 new 執行一個 Function 時，我們就稱做這種呼叫為「建構式呼叫」
* 當我們用建構式呼叫去執行一個 Function，這個 Function 就會被當作「建構式」
* 在 ES5 裡，我們把 function 當作建構子（constructor）使用

  * 我們可以透過 function 的方式來建立一個新的物件
  * 如果我們想要建立出同屬性名稱但不同屬性值的物件內容，我們可以把物件的屬性值變成參數，如此就能透過此 function constructor 建立出許多不同的物件

> **此外，我們會把根據建構子（constructor）所建立出來的物件稱作是實例（instance）**
> 使用 new 建構式呼叫的時候，實際上會有幾件事會被執行：

1. 首先會新建出一個「物件」
2. 新物件帶有 Prototype 連結，將物件的 .proto 指向建構子的 prototype，形成原型串鏈。
3. 建構式中的 this 會被繫結指向此新建的「物件」
4. 回傳新建的「物件」 ( 如果建構式本身沒有回傳東西的話 )

```
function Person(){
console.log(this)
}
var john = new Person()// console.log(john)
```

但若函式的最後 return 其他物件，則原新物件內容會被覆蓋。

```
function Person (){
this.firstName = 'John'
this.lastName = 'Doe'
return {"RETURN":"原本this的內容就不會被回傳"}
}
var john = new Person()
console.log(john) // Object {"RETURN":"原本this的內容就不會被回傳"}
```

但假若我們建立了一個狗的建構式，也在裡面寫入了行為的 function，實際執行上每叫一隻狗，就要叫一個 function，一萬隻狗一萬個 function 非常耗費記憶體，因此我們可以透過原型鍊將其寫在 Prototype 中，讓其共用一個 function 節省運算資源如下：

```
function Dog(name) { 
this.name = name
}
Dog.prototype.getName = function() {
return this.name
}
Dog.prototype.sayHello = function() {
console.log(this.name)
}
var d = new Dog('Peter')
var b = new Dog("Cow")
console.log(d.sayHello() === b.sayHello())
```

那 ES6 有 Class 的語法糖的程式碼的實例創建方法其實本質上也差不多，可見下範例：

```
class Dog {
setName(name) {
this.name = name
}
getName() {
return this.name
}
sayHello() { // 這邊在語法上不用加 function
console.log(this.name)
}
}
var d = new Dog() // 實例
d.setName('PAUL')
d.sayHello()
console.log(d.getName())
```

※ 這邊再額外提物件導向中兩個好用的工具，其概念也被運用在 Vue.Js 中，很推薦先暸解一下：

> **Getter & Setter**

* JavaScript 物件提供了 Getter 和 Setter 二種函式

  * 能讓我們用物件屬性方式呼叫方法，使得程式碼更簡潔，不必再另外建立函式
  * 如果有比較複雜的運用時，例如取不到值時不想回傳 undefined ，設定值小於零時設成將它以大於零來儲存時，這兩個函式就非常好用
* 在私有屬性或特殊值，就能用這兩種方法來作取得或設定

  * getter（取得方法）與 setter（設定方法）的呼叫語法，長得像一般的存取物件成員的語法，都是用句號 (.) 呼叫
  * 而且 setter（設定方法）是用指定值的語法，不是傳入參數的那種語法

**Getter: 取得特定值的方法**

```
var wallet = {
total: 100,
set save(price){
this.total = this.total + price / 2
}
}
wallet.save = 300;
console.log(wallet.total); // 250
```

**Setter: 存值的方法**

```
var wallet = {
total: 100,
set save(price){
this.total = this.total + price / 2
},
get save(){
return this.total / 2;
}
}
wallet.save = 300;
console.log(wallet.save); // 125
```

Object.defineProperty: 另外還有一種定義方式 Object.defineProperty，如果用 defineProperty 去定義 getter & setter，則 enumerable & configurable 預設為 false：

```
var wallet = {
total: 100,
}
Object.defineProperty(wallet,'save',{
configurable: true,
enumerable: true,
set : function(price){
this.total = this.total + price / 2;
},
get : function(){
return this.total / 2;
}
})
wallet.save = 300;
console.log(wallet);
```

```
var a = [1,2,3];
Object.defineProperty(Array.prototype,'latest',{
get: function(){
return this[this.length - 1]
}
})
console.log(a.latest);
```

### 2 . 物件導向的繼承（Inheritance）

* 繼承就是用在你需要用到一些共同的屬性時，不用所有東西都自己重新做
* 例如假設狗是 amimal 下面的分支，那所有 animal 的 function 你都可以用

> **但有一種常見的錯誤繼承方式，就是直接寫 Dog.prototype = Animal.prototype**

* 由於 Call by Sharing 的特性，兩個 function 的位置都被同步，簡單說就是覆蓋過去了
* 所以正確的做法是不應該直接修改到 Animal.prototype
* 而是應該要拷貝一份與 Animal.prototype 相同內容的物件後，指派給 Dog.prototype

```
Dog.prototype = Object.create(Animal.prototype)
```

這就是我們需要的，Object.create 會回傳與一個原型物件相同的新物件回來，因此就不會出現改到 Animal.prototype 的問題了。我們的完整範例會如下：

```
function Animal(name, gender, age) {
  this.name = name
  this.gender = gender
  this.age = age
}

Animal.prototype.speak = function() {
  console.log('some sounds')
}

Animal.prototype.move = function() {}

function Dog(name, gender, age) {
  this.name = name
  this.gender = gender
  this.age = age
}

Dog.prototype = Object.create(Animal.prototype)

Dog.prototype.speak = function() {
  console.log('Bow-wow')
}

Dog.prototype.move = function() {
  console.log('walk')
}

var dog1 = new Dog('Blacky', 'male', 3)
var dog2 = new Dog('Whity', 'female', 1)
var animal = new Animal('Browny', 'male', 5)
dog1.speak() // "Bow-wow"
dog2.speak() // "Bow-wow"
animal.speak() // "some sounds"
```

> **當你瞭解以上的邏輯， ES6 的繼承寫法又更簡單了**

* ES6 提供了定義（模擬）類別時的標準化方式
* 而在繼承這方面，可以使用 extends 來實現
* 而在在子類建構式中試圖使用 this 之前，也一定要先使用 super 呼叫父類建構式
* 就類別風格來說，可以想成父類建構初始化必須先完成，再執行子類別初始化
* 如果沒有子類沒有定義建構式，自動加入的建構式中會呼叫父類建構式

> **聊到這邊我想先定義幾個 ES6 Class 語法糖的特性**

* class 中只能放方法（**在class 中的 function 都叫做 method**）
* 子類別需要在 constructor() 中呼叫 super() 來使用父類別的建構函式
* super() 只能在 constructor() 中執行
* 子類別的 constructor() 呼叫 super 之前，this 是沒有指向的會跑出 Reference Error

#### I. ES6 繼承與 super

讓我們開始透過個例子來瞭解 ES6 Class 繼承和 super() 特性

```
class Person{
  constructor(age, weight){
    this.age = age
    this.weight = weight
  }
  call_this(){
    return this;
  }
  static SonCanNotUse(){
    console.log("內部專用")
  }
}

class SuperMan extends Person{
  constructor(age, weight, power){  // 如果在 super() 之前就呼叫 this 的話會 reference error
    super()
    this.power = power
  }
  hello(){
    console.log(`我是個有 ${this.power} 戰鬥力的 SuperMAN`)
  }
}
```

> **要注意的點是一定要在繼承的 class 的 constructor() 中使用 this 前呼叫 super()**

* 為什麼有這樣的寫法限制？因為一般沒有繼承的情況下，在 constructor 裡面會先建立一個物件，然後把 this 指向這個物件
* 相反地，**有繼承的情況下，在子類別的 constructor 裏就不會有建立物件的動作**
* 為什麼呢？因為建立物件的動作只需要做一次就好了
* 所以我們會預期，物件已經在母類別的 constructor 裏建立了，否則就會在子物件裡重複動作
* 所以，我們要在子類別呼叫 super()，this 才不會是空（不然會跳 undefined ）

> **在子類中，可以用 super 來呼叫父類別的原型，在子類中呼叫父類別的原型可以做什麼**

1. 可以增加屬性 / 修改屬性 
2. 可以呼叫父類別的方法 
3. 可以對原類別 prototype 中的屬性

如下：

```
class Person{
  constructor(age, weight){
    this.age = age
    this.weight = weight
    this.property = '會在父類別實例中產生的屬性'
  }
  call_this(){
    return this
  }
  showProperty(){
    console.log(父類別實例的: ${this.property})
  }
  static SonCanNotUse(){
    console.log("老子專用")
    console.log(父類別私有方法的: ${this.property})
  }
}

class SuperMan extends Person{
  constructor(age, weight, power){
  // 如果在 super() 之前就呼叫 this 的話，會 reference error
    super();
    this.property = "子類用 this 初始化的property"
    super.property = "子類用 super 初始化的property"
    this.power = power
    super.showProperty()
    console.log(super.property)
  }
  hello(){
    console.log(`子類實例中，被 super.property 改掉的 property ${this.property}`)
  }
  static sonPrivateMethod(){
    super.showProperty()
  }
}
```

裡面特別難理解的一點是，這兩行程式碼的運行邏輯會截然不同:

```
super.showProperty()
console.log(super.property)
```

* **用 super 賦值**

  * super.showProperty() 會指向父類別中的 showProperty() 
  * 但其印出的 this.property 卻是子類別在 super.property=" 子類用 super 初始化的 property 宣告的這行程式碼：「父類別實例的子類用 super 初始化的 property」
  * **也就是在賦值的時候 super 等同子類中的 this，指向子類實例**
* **當 super 取值**

  * 然而 console.log(super.property) 的時則會印出 undefined
  * 因為當我們試圖用 super 取值時，這邊的 super 指向父類的 prototype
  * **而父類中則沒有給予 this.property 一個值**

#### II. ES6 繼承與 static

* static 關鍵字用來定義靜態方法 (static method)
* static 表示「類別」的靜態方法，靜態方法不需要實體化它所屬類別的實例就可以被呼叫
* 被定義為 Static Method 可以直接以 Constructor 呼叫
* static 的變數和方法不會被實例 (instance) 繼承，而是可以直接透過父類別呼叫使用
* 靜態方法經常被用來建立給應用程式使用的工具函數，可以有效避免全域的污染

```
class Triple {
  static triple(n) {
    if (n === undefined) {
      n = 1
    }
    return n * 3
  }
}

class BiggerTriple extends Triple {
  static triple(n) {
    return super.triple(n) * super.triple(n)
  }
}

console.log(Triple.triple()) // 3
console.log(Triple.triple(6)) // 18
var tp = new Triple()
console.log(BiggerTriple.triple(3)) // 81，在在子類別中用 super 取出 static
console.log(tp.triple()) // TypeError: tp.triple is not a function
```

* 被定義為靜態方法的函式可以直接以 constructor function 呼叫
* 它也無法被已實體化（new過）的類別物件呼叫
* 一般來說實例（如下述例子中的 var tp = new Triple()）也無法直接取用
* 但父類別上的靜態方法也可以透過 super 來調用

舉個例子如下

```
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  static student(name, age) { //Static Method 不需實體化所需類別的實例就可以被呼叫
    console.log(`I'm ${name}. ${age} years old.`) // 不要加this
  }
}

Person.student('Teagan', '22') // I'm Teagan. 22 years old.
let person = new Person('Teagan', 22) // 被定義為靜態方法的函式，無法被已實體化（new 過）的類別物件呼叫
person.student // Uncaught TypeError: person.student is not a function
```

### 2 . 物件導向的封裝（Encapsulation）

* 要提到 JavaScript 的封裝（Encapsulation），就得提到 Closure（閉包）
* 在 JavaScript 中函式被建立時，一個閉包就會被產生，閉包是一個函式建立時的就有的自然特性
* 而封裝（Encapsulation）則是運用其特性的延伸

Closure （閉包）可以達到封裝（Encapsulation）的目的，其實閉包最大的目的之一是能夠用來來實現「私有變數」，為了不讓不必要的資訊出現，也讓外界無法對其隨意更改，這跟 module 有相同的概念。

> **可以運用 Closure （閉包）可以達到封裝（Encapsulation）的目的，去封裝「私有變數」（對外部隱藏的物件屬性）**

* 閉包內的函式不僅可以在閉包建立時可以訪問這些變數
* 而且可以在閉包函式執行時，更改這些變數的值
* 閉包不是在建立的那一時刻的快照，而是一個真實的狀態「封裝」
* 只要閉包存在，就可以對變數進行修改

> **在談如何封裝前，首先我們要談談在 ES5 中實現 Class 中的私有屬性**

* 要在 function（ 建構函式 ）內部宣告一個變數，之後再搭配使用 getter & setter 去存取私有變數（Private Property）
* 但到底「私有屬性」要怎麼寫？

  * 其實......和很多高級語言不同，JavaScript 中沒有 public、private、protected 這些訪問修飾符（access modifiers）
  * 而且 JS 長期以來也沒有「私有屬性」這個概念，對象的屬性/方法默認都是 public 的
  * 但換個角度來說，任何在函式中定義的變數，都可以認為是另一種解讀下的私有變數
  * 因為不能在函式的外部訪問這些變數 （變數的作用域）

> **先講個結論吧，在 ES6 嚴格來說要實現 private property 最方便的寫法是什麼？ 答案是沒有**

* 對，答案真的就是沒有。雖然隨著 ES7 的支援性越來越高，要實現 private property 之後應該會有更方便的 # 關鍵字可以用
* 但現在 ES6 還沒有看到通用的方法
* 最簡單的方法其實可以再 private property or method 的名稱加上 _ 前綴，同事們可以很簡單看出這是個私有屬性，沒事不要去動它！（怒）

以下開始，簡單介紹其中三個比較常見的封裝（Encapsulation）寫法：

#### I. ES5 - function 模擬 Class 版 ( real private )

```
function Wallet(init) {
  let money = init
  this.getMoney = function() {
    return money
  }
}

const wallet = new Wallet(100)
console.log(wallet.getMoney())
console.log(wallet.money) // => 存取不到，真正的 private
```

#### II. ES6 - constructor 版 ( real private )

需要存取 private property 的 method 都只能放在 constructor 裡面。

```
class Wallet {
  constructor(num) {
    var _money = num
    this.getMoney = () => _money
    this.setMoney = (newNum) => _money = newNum
  }
}

const wallet = new Wallet(100)
console.log(wallet.getMoney())
console.log(wallet._money) // => 存取不到，真正的 private
```

#### III. ES6 - Symbols 版 ( half private )

比較新的寫法，使用上較為方便。

```
var money = Symbol()
class Wallet {
  constructor(num) {
    this[money] = num
  }
  getMoney() {
    return this[money]
  }
}

const wallet = new Wallet(100)
console.log(wallet.getMoney())
console.log(wallet[money]) // => 還是存取得到，不是真正的 private
```

就我自己的理解，封裝（Encapsulation）只是一個概念，而不是 JavaScript 中內建的語法，可以透過很多種方式達成，而 JavaScript 也不斷在新版本中推出新的語法糖。

因此重點還是掌握其概念的本質，以及 Closure （閉包）的特性，在實際應用中去配合當下的情境運用此概念達到想要設計的功能，和程式碼結構。

> 參考資料

* **物件導向的類別和實體（實例）**

  1. [你懂 JavaScript 嗎？#18 （簡易版）物件導向概念](https://cythilya.github.io/2018/10/25/mixing-up-class-objects/)
  2. [你懂 JavaScript 嗎？#17 物件（Object）](https://cythilya.github.io/2018/10/24/object/)
  3. [JavaScript 進階：物件導向 (new、super、封裝)](https://yakimhsu.com/project/project_w17_advancedJS_07_OOP.html)
  4. [JavaScript 物件導向式程式設計](https://www.fooish.com/javascript/oop-object-oriented-programming.html)
  5. [前端工程研究：關於 JavaScript 的物件藍圖建立方法](https://blog.miniasp.com/post/2020/06/19/JavaScript-Object-in-depth)
  6. [\[教學] 深入淺出 JavaScript ES6 Class (類別)](<* 1. https://shubo.io/javascript-class/#%E7%94%A8-super-%E8%A6%86%E5%AF%AB-constructor>)
  7. [ES6 中最容易誤會的語法糖 Class - 基本用法](https://medium.com/enjoy-life-enjoy-coding/javascript-es6-%E4%B8%AD%E6%9C%80%E5%AE%B9%E6%98%93%E8%AA%A4%E6%9C%83%E7%9A%84%E8%AA%9E%E6%B3%95%E7%B3%96-class-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95-23e4a4a5e8ed)
  8. [Day 10: ES6篇 - Class(類別)](https://ithelp.ithome.com.tw/articles/10185583)
  9. [學 JS 的心路歷程 Day21-JS 支援物件導向?(二)](https://ithelp.ithome.com.tw/articles/10208093)
  10. [Object Oriented Programming（OOP）是什麼？](https://teagan-hsu.coderbridge.io/2021/01/05/javascript-oop-constructor-function-class/)
  11. [Javascript 多型的問題](https://social.msdn.microsoft.com/Forums/zh-TW/9292d8ae-cc40-47e2-89bd-1cb2bd320319/javascript-2281022411303402183938988?forum=236)
  12. [Getter and Setter 筆記](https://note.pcwu.net/2017/02/12/js-getter-setter/)
  13. [JavaScript 中 set 與 get 方法用法示例](https://www.itread01.com/article/1534304024.html)
  14. [JS Getter 與 Setter DAY71](https://ithelp.ithome.com.tw/articles/10253936)
  15. [JS Object.defineProperty(): value V.S. getter & setter](https://ithelp.ithome.com.tw/articles/10203297)
  16. [Day 19: getter & setter](https://ithelp.ithome.com.tw/articles/10218892)
* **物件導向的繼承（Inheritance）**

  1. [JavaScript new、Function Constructor (建構函式) 及 Object.create()](https://shubo.io/javascript-new/)
  2. [JS 的原型繼承(方法3)－ES6 Class](https://dylan237.github.io/es6-class.html)
  3. [ES6 的 Class 、super 的特例與繼承](https://ithelp.ithome.com.tw/articles/10200658)
  4. [JavaScript ES6 class 關鍵字](https://www.fooish.com/javascript/ES6/class.html)
* **物件導向的封裝（Encapsulation）**

  1. [Javascript 中定義私有属性（Private Properties）](https://segmentfault.com/a/1190000023136396)
  2. [JavaScript 進階：物件導向 (new、super、封裝)](https://yakimhsu.com/project/project_w17_advancedJS_07_OOP.html)
  3. [JavaScript 進階：什麼是閉包 Closure 與實際應用](https://yakimhsu.com/project/project_w17_advancedJS_03_Clousure.html)
  4. [DAY 11. JavaScript Map and Set](https://ithelp.ithome.com.tw/articles/10191607)
  5. [JS 核心觀念筆記 - 閉包基本認識、工廠模式與私有方法](https://hsuchihting.github.io/javascript/20200812/3642687092/)
  6. [JavaScript - 第五章 閉包和作用域](https://iter01.com/115094.html)
  7. [閉包：私有化變數 《JavaScript高程3》](https://iter01.com/286200.html)
  8. [JavaScript 閉包的理解](https://www.gushiciku.cn/pl/p95i/zh-tw)
  9. [ES6 Class 中實現私有屬性的幾種方法](https://juejin.cn/post/6844903880174600205)
  10. [JavaScript 中私有屬性和方法](https://www.796t.com/content/1548244810.html)

- - -

## 四、結論

老實說關於第十六週的文章的撰寫對我是困難重重，兩篇文章陸陸續續寫了快一個多月，關於 JavaScript 的底層邏輯還算是大體能夠掌握概念，而關於其物件導性的特性，由於 JavaScript 程式語言特性的緣故，其並沒有內建這機制，因此必須掌握概念後，運用掌握 JavaScript 底層邏輯的特性後，去自行歸納並且應用模擬其功能和衍伸的應用。

因此真的得在需要更多的實戰經驗上才能透過程式碼的運作掌握其一些零碎的細節，我自認為也只瞭解了七成左右，但也已盡我所能，若有撰寫上的錯誤，還請讀者不吝賜教。