---
title: 貪婪演算法（Greedy Algorithm）
date: 2022-07-12T08:26:45.742Z
author: Boison
slug: algorithms-greedy
tags:
  - Algorithms
  - Greedy
draft: false
---
> **貪婪演算法就是每當執行區域性的抉擇時，都選擇當下有的選項中最好的一個選項**

也就是說，貪婪演算法並不會考慮到未來，只考慮當下，因此對於某些情況下不一定能求出最佳解。但由於貪婪演算法的高效性以及其所求得的答案比較接近最佳解，也可以用作輔助演算法或者直接解決一些要求結果不特別精確的問題。

> **貪婪演算法是一個概念，他的經隨在於局部最佳解 (Local Optimal Solution)**

把整個過程拆解成一個個步驟，然後針對第一個步驟找尋最佳解答，然後再找尋第二步驟的最佳解答，以此方式進行下去，最後的答案雖可能不是最佳解，但也會很接近最佳解。

包括 BFS、Dijkstra 都是貪婪演算法的一種。使用貪婪演算法雖然犧牲了一點點最終效益，卻能省下非常非常多的時間，因為他們十分簡單易寫。

> **常見的貪婪演算法**

* Dijkstra's Algrithm： 單一路徑之最短距離

* Huffman's Algorithm： 霍夫曼編碼或壓縮算法

* [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

* [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)

* [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

* [665. Non-decreasing Array](https://leetcode.com/problems/non-decreasing-array/)

---

## 一、範例: 零錢問題

給定不同面額的硬幣 coins，和一個總金額 amount，求出最少硬幣的組合數。如果沒有任何一種硬幣組合能組成總金額，回傳 -1。

```javascript
function coinChange(coins, amount) {
  let count = 0
  let total = 0
  for (let i = coins.length; i >= 0; i -= 1) {
    const coin = coins[i]
    while (total + coin <= amount) {
      count += 1
      total += coin
    }
  }
  return count
}

coinChange([1, 5, 10, 50], 90) // 5
coinChange([1, 5, 10, 50], 40) // 4
coinChange([1, 5, 10, 50], 15) // 2
```

對於新台幣硬幣面額，我們能使用貪婪演算法求出最佳解，但是，如果使用其他硬幣面額，就不一定能求出最佳解。

> **用動態規劃 (dynamic programming) 解零錢問題**

```javascript
function coinChange(coins, amount) {
  let dp = new Array(amount + 1).fill(Infinity)
  dp[0] = 0
  for (let coin of coins) {
    for (let i = coin; i <= amount; i += 1) {
      dp[i] = Math.min(dp[i], dp[i - coin] + 1)
    }
  }
  return dp[amount] === Infinity ? -1 : dp[amount]
}

coinChange([1, 3, 4], 6)
```

先建立 amount \+ 1 陣列（\+1 是為了對應總金額），將預設值設為 Infinity（便於與最小值的判斷），接下來遍歷每種面額對應的金額，並判斷是否為最小解。

動態規劃原理請見 [動態規劃與分治法（Divide-and-Conquer）](https://boison.tw/2022/07/algorithms-dac-dp/) 一文

---

> **參考資料**
>
> 1. [動態規劃與貪婪法題1：70 Climbing Stairs](https://www.796t.com/content/1546692737.html)
>
> 2. [（python版）第 2 章 最易懂的貪心演算法](https://tw.pythontechworld.com/article/detail/VQ326X8VF3wR)
>
> 3. [我的DSA日記 — 7](https://medium.com/@chiahunglin/%E6%88%91%E7%9A%84dsa%E6%97%A5%E8%A8%98-7-e15613948c91)
>
> 4. [30Day LeetCoding Callenge — Week4](https://matters.news/@louis0420/43659-30-day-leet-coding-callenge-week4-bafyreifuyzx647hsam4bwbxrw3sxws2evnvlu2ea7ud6gvyjdy7ywf5wxa)
>
> 5. [JavaScript 學演算法（二十四）- 貪婪演算法](https://chupai.github.io/posts/2009/greedyalgorithm/)
>
> 6. [Greedy、Dynamic Programming 與 Divide and Conquer](https://ithelp.ithome.com.tw/articles/10280042)