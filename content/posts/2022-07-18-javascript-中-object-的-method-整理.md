---
title: JavaScript 中 Object 的 Method 整理
date: 2022-07-18T01:42:00.567Z
author: Boison
slug: javascript-object-method
tags:
  - JavaScript
  - Object
draft: false
---
本篇詳細整理 Object 中的方法 Method 有以下幾種


1. **new Object()**

   * 宣告物件

2. **Object.values()**

   * 取出 value 放入 array

3. **Object.keys()**

   * 取出 key 放入 array

4. **delete**

   * 刪除 object 屬性（key）

5. **Object.entries()**

   * object 轉成 array

6. **Object.fromEntries()**

   * array 轉成 object

```javascript
// 0. 宣告物件
let classmates = 
classmates['11'] = 'Peter' 
classmates['32'] = 'Paul'  
classmates['3'] = 'John'
// { '11' : 'Peter', '32': 'Paul', '3': 'John'}

// 1. 取出 value 
Object.values(classmates) // ['Peter', 'Paul', 'John']

// 2. 取出 key
Object.keys(classmates) // ['3', '11', '32']

// 3. 刪除 object 屬性（key）
delete classmates['32']  // { '11' : 'Peter' , '3': 'John'}

// 4. object 轉成 array
Object.entries(classmates) // [['3', 'John'],['11', 'Peter'],['32', 'Paul']]

// 5. array 轉成 object
let arrayClassmates = Object.entries(classmates) // [['3', 'John'],['11', 'Peter'],['32', 'Paul']]
Object.fromEntries(arrayClassmates) // { '11' : 'Peter', '32': 'Paul', '3': 'John'}
```

> **參考資料**
>
> 1. [Object 物件基本操作](https://eudora.cc/posts/26138/)