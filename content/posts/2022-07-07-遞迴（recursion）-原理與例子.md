---
title: "遞迴（Recursion）: 原理與實例"
date: 2022-07-07T03:19:20.319Z
author: Boison
slug: Recursion
tags:
  - Recursion
  - Algorithms
draft: false
---
遞迴（Recursion） 最早是用在數學定義式，稱作 遞迴定義（recursive definitions），使用被定義對象的自身來為其下定義，使用被定義對象的自身來為其下定義。簡單來說，就是「自己定義自己」。

> **遞迴函式（recursive function）簡單來說就是在一個函式當中再去呼叫它自己**

之所以能夠透過遞迴函式，是因為函式堆疊（stack）在執行時有一個特性，當某個函式呼叫另一個函式時，需要等到裡面的函式執行完產生結果後，才會繼續回來執行自己的函式內容，而這樣的情況也被稱作 後進先出（Last in First Out, LIFO）。

> **遞迴（recursive function）的方式有二種分別為**

* 直接遞迴(Direct Recursion) ：直接在函式中呼叫函式本身
* 間接遞迴(Indirect Recursion) ：在函式中呼叫另個函式，再從另個函式呼叫原本函式

遞迴的常見應用題型有費波納契數列 (Fibonacci Sequence)、正整數階乘 (factorial)、最大公因數 (GCD)、河內塔 (Hanoi Tower)、N 個字元的排列組合等。

- - -

## 一、遞迴的構成條件

> **一個基本的遞迴函式一定要有**

1. 終止條件（基本條件）
2. 遞迴條件（呼叫自己的條件）

```javascript
// 初始（終止）條件為 n === 1，遞迴條件為 n !== 1。
function sum(n) {
  if (n === 1) {
    return 1;
  }
  return n + sum(n - 1);
}

// 遞迴過程
sum(5)
  5 + sum(4)
    4 + sum(3)
      3 + sum(2)
        2 + sum(1)
          return 1
        return 2 + 1   // 3
      return 3 + 3     // 6
    return 4 + 6       // 10
  return 5 + 10        // 15
```

事實上遞迴的時間複雜度相當高，並不會實際使用在計算中，需要搭配動態運算等方法把已經計算過的數值記錄下來，優化計算時間。

- - -

## 二、遞迴與迭代

> **遞迴可以改寫成迭代，迭代反之亦然**

1. **迭代**

   * 迴圈結構，是由下而上（Bottom-Up），一步步逼近答案
   * 用新值覆蓋舊值，直到滿足條件後結束，因為不保存中間值，因此不會消耗很多記憶體空間
2. **遞迴**

   * 選擇結構，是由上而下（Top-Down），慢慢地將問題縮小，來求得答案
   * 將問題分解成干個子問題再回頭運算答案，因此會消耗大量記憶體空間，但程式碼簡單明瞭
   * 容易產生 [堆疊溢位（stack overflow）](https://zh.wikipedia.org/wiki/%E5%A0%86%E7%96%8A%E6%BA%A2%E4%BD%8D)，因此非必要，不建議使用遞迴

- - -

## 三、例子: 費波那契數列

```javascript
// 初始（終止）條件為 n === 0 || n ===1，遞迴條件為 n !== 0 && n !== 1
function fibo(n) {
  if (n === 0) {
    return 0;
  } else if (n === 1) {
    return 1;
  } 
  return fibo(n-1) + fibo(n-2)
}
fibo(3)

// 遞迴過程
fibo(3)
  fibo(2) + fibo(1) 
    fibo(1) + fibo(0)
      return 0           // 0
    return 1 + 0         // 1
  return 1 + 1           // 2
```

![](https://miro.medium.com/max/1400/0*8l3i67OHS_YSXPKX.jpeg)

圖片來源: [演算法筆記：遞迴（Recursion）](https://medium.com/traveling-light-taipei/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98-%E9%81%9E%E8%BF%B4-recursion-e66e81566679)

> **F(1) & F(0) 這就是費氏數列遞迴的 Base case，也就是停止的時機**

因此找到這兩項後，就可以開始往前加總出其他項的值，而往前加總的順序如上圖。

> **可以求出最後時間複雜度約等於為 O(2^n)**

* 每往下一層，步驟數變 2 倍
* 總共有 n 層 （空間複雜度 O(n)）
* 時間複雜度：O(2^n)

> **動態規劃（Dynamic programming ) 優化後的費波那契數列解法**

```javascript
// BEST TC:O(N) SC:O(1)
var fib = function(n) {
  const dp = [0, 1]
  return fibonacci(n, dp)
};

function fibonacci(n, dp) {
  if (n <= 1) return dp[n]  
  dp[n] = fibonacci(n-1, dp) + fibonacci(n-2, dp)
  
  return dp[n]
}
```

- - -

## 四、例子: 正整數的階乘（factorial）

```javascript
// 初始（終止）條件為 n === 0，遞迴條件為 n !== 0
function factorial(n) {
  if (n === 0) {return 1;}
  return n * factorial(n - 1);
}

// 遞迴過程
factorial(4) 
  4 * factorial(3) 
    3 * factorial(2)
      2 * factorial(1)
        1 * factorial(0)
          return 1      // 1
        return 1 * 1    // 1
      return  2 * 1     // 2
    return 3 * 2        // 6
  return 4 * 6          // 24 
```

- - -

> **參考資料**
>
> 1. [演算法筆記：遞迴（Recursion）](https://medium.com/traveling-light-taipei/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98-%E9%81%9E%E8%BF%B4-recursion-e66e81566679)
> 2. [JavaScript 學演算法（二十二）- 遞迴 Recursion](https://chupai.github.io/posts/2008/alg_recursion/)
> 3. [遞迴 (Recursive) 介紹與經典題型](https://kopu.chat/%E9%81%9E%E8%BF%B4-recursive-%E4%BB%8B%E7%B4%B9%E8%88%87%E7%B6%93%E5%85%B8%E9%A1%8C%E5%9E%8B/)﻿
> 4. [演算法筆記系列 — Dynamic programming 動態規劃](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-dynamic-programming-%E5%8B%95%E6%85%8B%E8%A6%8F%E5%8A%83-de980ca4a2d3)