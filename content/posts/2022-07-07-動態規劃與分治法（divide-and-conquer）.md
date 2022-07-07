---
title: 動態規劃與分治法（Divide-and-Conquer）
date: 2022-07-07T06:25:05.489Z
author: Boison
slug: algorithms-divide_and_conquer-dynamic_programming
tags:
  - Algorithms
  - DivideAndConquer
  - DynamicProgramming
draft: false
---
> **動態規劃跟遞迴其實都可以算是分治法的一種實現**

1. **動態規劃 (dynamic programming)**  其實是分治法的一種延伸與特例

   * 動態規劃將較小問題的解記錄下來
   * 使得在處理較大問題的時候，可以不用重複去處理較小的問題
   * 而是直接利用所記錄的較小問題的答案來求解。
2. **分治法（Divide-and-Conquer）**  採用由上而下（top-down）的解題思路方式

   * 可將原始問題分成多個小的
   * 而所有小問題的結果彙整合併（bottom-up）由下而上
   * 之後就可以成為原始問題的最終解法

![image](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/7/1728f224fc3acaae~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

圖片來源: [JS数据结构与算法之《分治、回溯、动态规划与贪心》](https://juejin.cn/post/6844904182680387597)

※ 註:

1. 貪婪演算法 （Greedy Algorithm）: 實際上是動態規劃算法的一種特殊情況
2. 回溯法（Back Tracking）: 回溯法是暴力搜尋法中的一種，回溯其實就是在遞迴遞增地建立所有可能中，篩選掉不可能的組合，最終得到所要的答案

- - -

## 一、分治法（Divide-and-Conquer）

> **將難以直接解決的問題切分成可簡單處理的小問題，再將小問題的結果合併即答案**

分治法（Divide-and-Conquer） 或稱 各個擊破法、切割征服法，是一種演算法思想，而不是用於解決特定問題的演算法。

分治法最適合使用遞迴來實作，但遞迴和迭代是一一對應的，分治法只是一種演算法思想，所以使用迭代來實作也沒問題。

> **分治法在每一層遞迴上都有三個步驟**

1. 分解（divide）

   * 將原問題分解為若干個規模較小，相互獨立，與原問題形式相同的子問題
2. 解決（conquer）

   * 若子問題規模較小且易於解決時，則直接求解
3. 合併（combine）

   * 將各子問題的解合併，合併後的結果為原問題的解

> **分治法所能解決的問題一般具有以下幾個特徵**

1. 該問題的規模縮小到一定的程度就可以容易地解決
2. 該問題可以分解爲若干個規模較小的相同問題

   * 即該問題具有 **最佳子結構（Optimal Substructure）** 性質
3. 利用該問題分解出的子問題的解，可以**合併**爲該問題的解
4. 該問題所分解出的各個子問題是**相互獨立**的

> **常見可用分治法的問題與方法**

* Tower of Hanoi (河內塔)
* Binary tree traversal (二元樹追蹤)
* Quick sort / Merge sort / Binary Search

- - -

## 二、動態規劃 (dynamic programming)

> **動態規劃（Dynamic Programming）會將一個大的問題拆解先處理較小問題，並將結果暫存起來再進一步建構出原始問題的可行解**

動態規劃 (dynamic programming) 與分治法最大的差別是，動態規劃經分解後得到的子問題往往不是互相獨立的，有互相重疊部分（即下一個子階段的求解是建立在上一個子階段的解的基礎上，進行進一步的求解），因此需要用記憶體儲存解子問題的解，以便下次需要同一個子問題解之時直接使用（避免重複運算）

> **動態規劃主要需要考量**

1. 定義原問題與子問題的關係，可使用遞迴關係表示
2. 以自底向上或自頂向下的記憶化方式，儲存子問題的解
3. 決定如何得到完整的最佳解

> **動態規劃適用之情形**

1. 最佳子結構（Optimal Substructure）
2. 無後效性

   * 即子問題的解一旦確定，就不再改變
   * 不受在這之後、包含它的更大的問題的求解決策影響
3. 子問題重疊的性質

> **[遞迴（Recursion）](https://boison.tw/2022/07/recursion/)一文中的費波那契數列 ([509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/))**

費氏數列雖然使用遞歸簡單明瞭，但子問題並不是獨立。

很明顯的，子問題重疊，會出現重複計算，因此導致執行效率不佳，當 n 非常大時，直接計算到爆炸，時間複雜度 O(2^n)、空間複雜 O(n)。

```javascript
// TC:O(2^N) SC:O(N)
var fib = function(n) {
  if (n < 2) {
    return n;
  }
  return fib(n - 1) + fib(n - 2);
}

f(5) = f(4) + f(3);
f(4) = f(3) + f(2);
f(3) = f(2) + f(1);
f(2) = f(1) + f(0);
f(1) = 1;
f(0) = 0;
```

> **若我們用變數 `dp` 將結果儲存，就可以避免重複計算，減少記憶體空間**

```javascript
// TC:O(N) SC:O(N)
var fib = function(n) {
  let dp = [0, 1, 1]
  for (let i = 3; i <= n; i += 1) {
    dp[i] = dp[i - 1] + dp[i - 2]
  }
  return dp[n]
}
```

> **進一步優化，只存上一個和當前的值，減少記憶體空間**

```javascript
// TC:O(N) SC:O(1)
var fib = function(n) {
  if (n === 0) return 0
  if (n === 2 || n === 1) return 1
  let prev = 1
  let curr = 1
  for (let i = 3; i <= n; i += 1) {
    let sum = prev + curr
    prev = curr
    curr = sum
  }
  return curr
}
```

> **常見可用動態規劃（Dynamic Programming）法的問題**

* Floyd-Warshall's Algrithm： 所有路徑間之最短距離
* Traveling Salesman Problem： 旅行推銷員問題
* 0/1 Knapsack Problem： 0/1 背包客問題

- - -

> 參考資料
>
> 1. [JavaScript 學演算法（二十三）- 分治法、動態規劃](https://chupai.github.io/posts/2009/divide_and_conquer_dp/)
> 2. [JS数据结构与算法之《分治、回溯、动态规划与贪心》](https://juejin.cn/post/6844904182680387597)
> 3. [遞迴函式與回溯法優化](https://ithelp.ithome.com.tw/articles/10278340)
> 4. [用JavaScript學習資料結構與演算法 10：動態規劃](https://break0344.medium.com/data-structures-and-algorithms-10-dynamicprogramming-4f5991175691)