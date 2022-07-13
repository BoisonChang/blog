---
title: 分支限界法 （Branch-and-Bound）
date: 2022-07-13T03:05:38.107Z
author: Boison
slug: algorithms-b&b
tags:
  - Algorithms
  - BranchAndBound
draft: false
---
我的理解是，分支限界法 （Branch and Bound）有點貪婪演算法（Greedy Algorithm）和回溯法（Backtracking）的精神。(可見 [回溯法（Backtracking）](https://boison.tw/2022/07/algorithms-backtracking/)和  [貪婪演算法（Greedy Algorithm）](https://boison.tw/2022/07/algorithms-greedy/)文章)



在每次做出選擇當下選擇最優解，並且將找到的最短路徑作為新的約束條件，若在遍歷過程中不符合此約束條件，就退回一步甚至是上幾步的計算重新選擇。

![](https://media.heptabase.com/v1/images/fc9c5ccc-1901-4cb2-bb69-b6fcd717ac4f/7c268d42-8519-46a3-af3a-ffc53deb913e/image.png)

圖片來源: [超圖解！一次搞懂演算法｜入門篇（Python）](https://hiskio.com/courses/1212/lectures/135337)

> **參考資料**
>
> 1. [超圖解！一次搞懂演算法｜入門篇（Python）](https://hiskio.com/courses/1212/lectures/135337)
>
> 2. [使用分支定界的旅行推銷員問題](https://www.techiedelight.com/zh-tw/travelling-salesman-problem-using-branch-and-bound/)
>
> 3. [branch and bound（分支定界）算法求解TSP旅行商问题](https://blog.csdn.net/qq_42518956/article/details/116716328)