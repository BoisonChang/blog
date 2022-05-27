---
title: "百日轉職前端工程師：第三週 JS 常用內建函式《DAY 9》"
slug: Transfer-FrontEnd-100-9
date: 2020-07-20T16:11:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "JavaScript"]
keywords:  ["FrontEnd", "Transfer", "JavaScript"]
author: "Boison"

---

大家好，這是百日轉職前端工程師的 Day9，也是 7/20（一）。

今天開始要來複習 JavaScript 常用的內建函式 (function)，這些多是程式導師實驗計畫第三週程式基礎下的課程內容，其實函式如果運用得好可以節省很多時間，不過在實際運用函式前，其實也練習手刻了滿多功能的，這樣子的好處是能夠瞭解函式內在邏輯是怎運作的，其次當遇到真的沒有的函式符合你的需求時，也能隨時動手作出來。

---

## 一、常用的陣列內建函式如何使用

### 1. Array.map

- 可以對陣列內每一個元素做「處理」，會回傳一個每一元素處理完後新的陣列。
- 比方說先寫好一個 double() 的 function，然後再 Array.map(double)

### 2. Array.filter

- 可以對陣列內每一個元素做「過濾」，會回傳原陣列中過濾完後回傳 true 的元素組成的新陣列。

### 3. Array.indexOf

- 幫你找到你要的值在陣列中的第一個位置，若是找不到則會回傳 -1
- 比方說 [1, 2, 4, 5, 4].indexOf(4) 則會回傳 2，在第三個的位置
- 反之，Array.lastIndexOf 會從後面找回來，比方說 [1, 2, 4, 5, 4].lastIndexOf(4) 則會回傳 5，在第五的位置

### 4. Array.reverse()

- 對陣列內每一個元素做反轉，然後回傳一個所有元素順序倒轉的陣列
- ['one', 'two', 'three'].reverse() 則會回傳 ['three', 'two', 'one']

### 5. Array.fill()

- 將指定的值填入取代陣列已有的元素，可以指定填充位置
- [1, 2, 3, 4].fill(2) 則會回傳 [2, 2, 2, 2]
- [1, 2, 3, 4].fill(5, 2, 3) 則會填充第二到第三的位置回傳 [1, 2, 5, 5]

### 6. Array.join()

- 將所有的陣列元素以一個指定的字串連接後輸出一個字串，若沒指定則以 , 連接
- ['one', 'two', 'three'].join('-') 則回傳一字串 'one-two-three'

---

## 二、常用的字串內建函式如何使用

### 1. String.trim

- 將字串「前」「後」的指定元素刪除掉回傳一新字串，若是沒指定則預設為空白。
- 比方說 ' 5566 '.trim() 則回傳 '5566'

### 2. String.toLowerCase

- 將字串中所有英文大寫轉成小寫後回傳一新字串
- 'APPLE'.toLowerCase() 則回傳 'apple'

### 3. String.endsWith

- 判斷字串是否以指定字元做結尾，若是則回傳 true
- 'My name is Paul.'.endsWith(Paul.) 則回傳 true
- 'My name is John.'.endsWith(Paul.) 則回傳 false

### 4. String.padEnd

- 將一字串不足指定長度的部分以指定值填入，若未指定值則預設填充空白，然後回傳一填充後的字串
- '012'.padEnd(10, '-') 則回傳 '012--------'
- '012'.padEnd(10) 則回傳 '012 '

### 5. String.slice

- 切下初始到結束位置的字串元素然後回傳成一個新的字串
- 'Hello, I am fine. How about you?' .slice(7, 16) 回傳 'I am fine.'

### 6. String.repeat

- 'a123'.repeat(3) ，就會輸出 'a123a123a123'

---

## 三、總結：瞭解函式的原理，才好用

以上的函式其實都能自己寫出來，所以在直接套用函式前記得先瞭解原理，當你遇到你需要的功能沒有現有的內建函式符合的時候，不要忘了其實你都可以自己寫出來。
