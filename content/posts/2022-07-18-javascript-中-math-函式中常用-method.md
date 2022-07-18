---
title: JavaScript 中 Math 函式中常用 Method
date: 2022-07-18T03:38:11.531Z
author: Boison
slug: javascript-math-method
tags:
  - JavaScript
  - Math
draft: false
---
本篇詳細整理 Math 函式中常用 Method 有以下幾種

 1. **Math.abs(x)**

    *  絕對值: 回傳 x 的絕對值

 2. **Math.ceil(x)**

    *  無條件進位: 回傳 x 值的大於等於最小整數

 3. **Math.floor(x)**

    * 無條件捨去: 回傳 x 值的小於等於最大整數

 4. **Math.round(x)**

    * 四捨五入: 回傳 x 值小數點後四捨五入的整數

 5. **Math.trunc(x)**

    * 去除小數: 回傳 x 值去除小數的整數

 6. **Math.random()**

    * 隨機取得0\~1之間的數值

    * 延伸: 隨機取得範圍內整數

      * **Math.floor(Math.random()\* X )**

 7. **Math.max(...array)**

    * 取得數列中最大值

 8. **Math.min(...array)**

    * 取得數列中最小值

 9. **Math.sign(x)**

    * 回傳數值正、負數型態

      * 正數回傳 1，負數回傳 -1

      * 0 回傳 0，-0 回傳 -0，字串等不是數字的回傳 NaN

10. **Math.pow(base, exponent)**

    * 回傳底數 (base) 的指數 (exponent)

```javascript
// 1. 絕對值 (abs)
Math.abs(-10)       // 10

// 2. 無條件進位，取整數 (ceil)
Math.ceil(7.001)    // 8
Math.ceil(-7.01)    // -7

// 3. 無條件捨去，取整數 (floor)
Math.floor(7.001)   // 7
Math.floor(-7.01)   // -8 

// 4. 四捨五入，取整數 (round)
Math.round(10.4999) // 10
Math.round(10.5001) // 11

// 5. 去除小數，只保留整數 (trunc)
Math.trunc(10.111)  // 10
Math.trunc(-7.111)  // -7

// 6. 隨機取得0~1之間的數值 (random)
Math.random()       // 0.0000…1 ~ 0.99999…

// 7. 隨機取得範圍內整數
Math.floor(Math.random()* X )  // 0 ~ 1

let array = [10,32,51,332,42]
// 8. 取得數列中最大值 (max)
Math.max(...array) // 332

// 9. 取得數列中最小值 (min)
Math.min(...array) // 10

// 10. 回傳數值正、負數型態 (sign)
Math.sign(10);     // 1 (正數)
Math.sign(-10);    // -1 (負數)
Math.sign(0);      // 0 (正零)
Math.sign(-0);     // -0 (負零)
Math.sign('a');    // NaN (不是數字)

// 11. 回傳底數 (base) 的指數 (exponent)
Math.pow(2,3)      // 8 (2^3)
```

> **參考資料**
>
> 1. [常用Js Math方法筆記](https://medium.com/%E6%82%A0%E9%81%8A%E9%AD%9A%E4%B8%96%E7%9A%84%E8%87%AA%E5%8B%95%E5%8C%96%E7%AD%86%E8%A8%98/%E5%B8%B8%E7%94%A8js-math%E6%96%B9%E6%B3%95%E7%AD%86%E8%A8%98-7effa5967d44)
>
> 2. [JavaScript Math 对象](https://www.runoob.com/jsref/jsref-obj-math.html)
>
> 3. [JavaScript 數值與算數（Number and Math）](https://pjchender.dev/javascript/js-number-math/)