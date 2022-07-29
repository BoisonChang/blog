---
title: JavaScript 中 Array 的 Method 整理
date: 2022-06-16T04:07:29.657Z
author: Boison
slug: JavaScript-Array-Method
tags:
  - JavaScript
  - Array
---
之前寫過一篇 [百日轉職前端工程師：第三週 JS 常用內建函式《DAY 9》](https://boison.tw/2020/07/transfer-frontend-100-9/) 摘要內容如下:

前文內 String 的提到的內建方法:

1. **String.trim**

   * 將字串「前」「後」的指定元素刪除掉回傳一新字串，若是沒指定則預設為空白
2. **String.toLowerCase**

   * 將字串中所有英文大寫轉成小寫後回傳一新字串
3. **String.endsWith**

   * 判斷字串是否以指定字元做結尾，若是則回傳 true
4. **String.padEnd**

   * 將一字串不足指定長度的部分以指定值填入，若未指定值則預設填充空白
5. **String.slice**

   * 切下初始到結束位置的字串元素然後回傳成一個新的字串6
   * String.substring 功能很類似

6. **String.split**

   * 找到字串中的某元素，然後將左右兩側分開回傳
7. **String.replace**

   * 用來將字串中的字取代為另一個字。
8. **String.charCodeAt()** 

   * 轉換成字串的位置處字的 Unicode 編碼

     * 延伸 String.charAt() 
     * 'apple'.charAt(0) // ‘a’
9. **String.repeat**

```
'     5566 '.trim() 
// '5566'

'APPLE'.toLowerCase()
// 'apple'

'My name is Paul'.endsWith(Paul) 
// true

'012'.padEnd(10, '-') 
// '012-------'

'Hello, I am fine. How about you?'.slice(7, 16)
// 'I am fine.'

'Hello'.split('')
// ['H', 'e', 'l', 'l', 'o']

'a123'.repeat(3)
// 'a123a123a123'

'Hello John ! Hello Peter!'.replace('Hello', 'Hi')
// 'Hi John ! Hello Peter!'
'Hello John ! Hello Peter!'.replace(/Hello/g, 'Hi')
// 'Hi John ! Hi Peter!'

'Apple'.charCodeAt(0) // 65
'apple'.charCodeAt(0) // 97
```

本篇進一步詳細整理 Array Prototype 中的方法 Method 有以下幾種

* Basic Array Method
* Filter/Find something
* Special Method

- - -

## 一、**Basic Array Method**

### 1. push()

* Array.prototype.push(elementN)
* 在原本的 Array **後面**加上新值，回傳 Array 的長度

### 2. unshift()

* Array.prototype.unshift(elementN)
* 在原本的 Array **前面**加上新值，回傳 Array 的長度

### 3. pop()

* Array.prototype.pop()
* 移除原本陣列**最後面**的第一個值，回傳被移除掉的那個值

### 4. shift()

* Array.prototype.shift()
* 移除原本陣列**最前面**的第一個值，回傳被移除掉的那個值

### 5. concat()

* Array.prototype.concat(arrayN)
* 兩個陣列合併在一起，並回傳新陣列

```
const character = ['A', 'B', 'C']

character.push('D') 
// 3

character.unshift('D') 
// 3

character.pop() 
// 'C'

character.shift()
// 'A'

character.concat(['D'])
// ['A', 'B', 'C', 'D']
```

- - -

## 一、**Filter/Find something**

### 1. findIndex()

* Array.prototype.findIndex(item, index, array)
* 回傳**第一個**符合條件的位置 index，若都找不到則回傳 -1

### 2. find()

* Array.prototype.find(item, index, array)
* 回傳一個值，且是**第一個**抓到條件為 true 的值

### 3. filter()

* Array.prototype.filter(item, index, array)
* 回傳包含「回傳條件為 true 的值」的一個陣列

### 4. forEach()

* Array.prototype.forEach(item, index, array)
* forEach 不會回傳任何東西，單純只執行原本陣列裡的事

### 5. map()

* Array.prototype.map(item, index, array)
* 將條件運算後重新組合回傳一個數量等於 array.length 的陣列

### 6. some()

* Array.prototype.some(item, index, array)
* 回傳一個 Boolean，只要部分符合就回傳 true 停止執行

### 7. every()

* Array.prototype.every(item, index, array)
* 回傳一個 Boolean，全部符合才回傳 true，部分符合回傳 false

```
const list = [
    { product: 'A', number: 10 },
    { product: 'B', number: 13 },
    { product: 'C', number: 14 },
    { product: 'D', number: 18 },
    { product: 'E', number: 19 },
    { product: 'F', number: 22 },
]

list.findIndex(item => item.number > 10)
// 1

list.find(item => item.number > 10)
// { product: 'B', number: 13 }

list.filter(item => item.number > 10)
// [{ product: 'B', number: 13 },{ product: 'C', number: 14 },{ product: 'D', number: 18 },{ product: 'E', number: 19 },{ product: 'F', number: 22 }]

list.forEach(item => item.number > 10)
// undefined

list.map(item => item.number > 10)
// [false, true, true, true, true, true]

list.some(item => item.number > 10)
// true

list.every(item => item.number > 10)
// false
```

- - -

## 三、**Special Method**

### 1. reduce()

* Array.prototype.reduce(accumulator, currentValue, currentIndex, array \[, initialValue])
* 回傳運算結果的值，initialValue 可以設定初始值

### 2. sort()

* Array.prototype.sort(compareFunction)
* 若沒有 compareFunction，會先自動轉成字串，回傳一個根據 Unicode 排序 array.length 長度的 array

### 3. reverse()

* Array.prototype.reverse()
* 回傳反過來長度為 array.length 的陣列

### 4. slice()

* Array.prototype.slice(start, end)
* 回傳在 start(包含) 跟 end 之間的陣列

### 5. splice()

* Array.prototype.splice(start, deleteCount, item1, item2, …)
* 回傳被移掉的值放在陣列裡

### 6. indexOf()

* Array.prototype.indexOf(searchElement)
* 回傳找到第一個位置的 index，如果找不到會回傳 -1

  * 延伸 lastIndexOf()
  * 回傳找到最後一個位置的 index，如果找不到會回傳 -1

### 7. at()

* Array.prototype.at(searchElement)
* 負數從尾端開始 array\[array.length-1] === arrat.at(-1) 

### 8. join()

* Array.prototype.join(separator)
* 把所有陣列裡的值加上 separator，回傳一個字串

### 9. includes()

* Array.prototype.includes(searchElement, fromIndex)
* 看 searchElement 有沒有在 array 裡，回傳 Boolean

### 10. fill()

* Array.prototype.fill(value)
* 填充值進 array，很常用在設定預設值，如 const arr = Array(6).fill(0)

### 11. Array.from()

* Array.from()
* 將以下轉換成陣列

  1. 類似陣列的資料
  2. 可迭代（iterable）物件

```
const character = ['A', 'B', 'C', 'D', 'E', 'F', 'G']

character.reduce((acc, val) => acc += val )
// 'ABCDEFG'

character.reverse()
// ['G', F', 'E', 'D', 'C', 'B', 'A']

character.slice(2,4)
// ['C', 'D']

character.splice(2,4)
// ['C', 'D', 'E', 'F']

character.indexOf('C')
// 2

character.at(-1)
// 'G'

character.join('-')
// 'A-B-C-D-E-F-G'

character.includes('C')
// true

const character2 = ['C', 'A', 'B', 'D', 'E', 'F', 'G']
character2.sort()
//  ['A', 'B', 'C', 'D', 'E', 'F', 'G']

character.fill('X')
['X', 'X', 'X', 'X', 'X', 'X', 'X']

Array.from([1, 2, 3], x => x + x) //  [2, 4, 6]
Array.from(new Map([[1, 2], [2, 4], [4, 8]])) // [[1, 2], [2, 4], [4, 8]]
Array.from({length: 4}, i => i) // [undefined,undefined,undefined,undefined] 
Array.from({length: 4},(v, i) => i) // [0, 1, 2, 3]
```

> 參考資料:  [前端工程師用 javaScript 學演算法系列陣列 Array](https://ithelp.ithome.com.tw/articles/10213787)