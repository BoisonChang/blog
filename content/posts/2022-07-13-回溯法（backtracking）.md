---
title: 回溯法（Backtracking）
date: 2022-07-13T02:32:00.166Z
author: Boison
slug: algorithms-backtracking
tags:
  - Algorithms
  - Backtracking
draft: false
---
[回溯法（Backtracking）](https://zh.wikipedia.org/wiki/%E5%9B%9E%E6%BA%AF%E6%B3%95)是暴力搜尋法（窮舉搜尋、枚舉法）中的一種。用試錯的思想，在分步解決問題的過程中，當探索到某一步時，發現原先選擇並不能得到有效的正確的解答的時候，就退回一步甚至是上幾步的計算重新選擇。

也可以說回溯法（Backtracking）是優化後的枚舉法，為解答加上一個約束條件，若是遇到不符合條件的則不從這個方向繼續枚舉退回上一步嘗試別的路徑。

> **回溯法（Backtracking）可以分為兩個概念**

1. 枚舉 enumerate：每一步列出所有可能的下一步一一測試。

2. 剪枝 pruning：遇到不符合條件的下一步便省略，不再繼續枚舉。

> **全排列（Permutations）問題**

1. 問題：用程式列出所有由 1、2、3 構成的 不重複序列。

2. 約束條件（不重複）：過濾重複的組合。

```javascript
const res = [];

for (let i = 1; i <= 3; i +=1) {
  for (let j = 1; j <= 3; j +=1) {
    if (i === ｊ) {continue}
    for (let k = 1; k <= 3; k +=1) {
      if (i === k || ｉ === k) {continue}
      res.push([i, j, k])
    }
  }
}
/* 
[ [ 1, 2, 3 ], 
   [ 1, 3, 2 ], 
   [ 2, 1, 3 ], 
   [ 2, 3, 1 ], 
   [ 3, 1, 2 ], 
   [ 3, 2, 1 ] ] 
*/
```

> **常見問題**

1. [46. Permutations](https://leetcode.com/problems/permutations/)

2. [51. N-Queens](https://leetcode.com/problems/n-queens/)

3. [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

---

> **參考資料**
>
> 1. [JavaScript 學演算法（二十五）- 回溯法](https://chupai.github.io/posts/2009/backtracking/)
>
> 2. [Day27:Backtracking -回溯法](https://ithelp.ithome.com.tw/articles/10273084)
>
> 3. [簡單易懂的回溯演算法（Back Tracking)](https://www.gushiciku.cn/pl/pXfx/zh-tw)
>
> 4. [超圖解！一次搞懂演算法｜入門篇（Python）](https://hiskio.com/courses/1212/lectures/135334)