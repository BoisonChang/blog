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