---
title: "Two Pointer: 低空間複雜度的解題技巧"
date: 2022-07-25T09:26:22.768Z
author: Boison
slug: algorithms-two-pointer
tags:
  - Algorithms
  - TwoPointer
draft: false
---
> **Two Pointer 是快速查找解題常用技巧，常見於處理 Array 、String 或 Linked-List** 

在特定的情況下，如已排序資料等，使用 Two Pointer 通常讓執行時間複雜度保持在線性 O(N)，降低運算時間，更能在接近不額外使用空間原地交換值，讓時間複雜度接近 O(1)。

> **KeyWords: 交換、不額外使用空間、已排序資料**

---

## 1. Two Pointer 的兩種類型

1. **左右指標 (反向指標)**

   * 指標通常沒有快慢，一個從頭一個從尾開始逐漸靠近，類似 Quick Sort 的 Hoare

2. **快慢指標 (同向指標)**

   * 指標會有快慢之分，都會一起從頭/從尾向右/左移動，類似 Quick Sort 的 Lomuto

![](https://miro.medium.com/max/700/1*f6HkCSZz_JxH21iS6wmb3w.gif)

左右指標 (反向指標), 圖片來源: [Count the Number of Triplets in… ](https://ganeshpr227.medium.com/how-to-count-the-number-of-triplets-in-an-array-with-sum-within-the-given-range-a-b-ad9b853095ff)

![](https://miro.medium.com/max/1200/1*YWjJuLuXJULb-2IyYhqnXQ.gif)

快慢指標 (同向指標), 圖片來源: [Basics of Two Pointers Technique](https://cdragon.medium.com/basics-of-two-pointers-technique-e1a0df57ba7e)

---

## 2. Two Pointer 常見題目

* **Easy**

  1. [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
  2. [1. Two Sum](https://leetcode.com/problems/two-sum/)
  3. [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)
  4. [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
  5. [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

* **Medium**

  1. [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/valid-parentheses/)
  2. [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
  3. [15. 3Sum](https://leetcode.com/problems/3sum/)
  4. [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

---

> **參考資料**
>
> 1. [【第十天 - Two-pointer 介紹】](https://ithelp.ithome.com.tw/articles/10262277)
>
> 2. [演算法筆記系列 — Two Pointer 與Sliding Window](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-two-pointer-%E8%88%87sliding-window-8742f45f3f55)
>
> 3. [Two Pointer 快慢指標篇](https://haogroot.com/2020/09/07/two-pointer-leetcode/)
>
> 4. [\[LeetCode\] 雙指標法(Two pointers)解題](https://medium.com/johnny%E7%9A%84%E8%BD%89%E8%81%B7%E5%B7%A5%E7%A8%8B%E5%B8%AB%E7%AD%86%E8%A8%98/leetcode-%E9%9B%99%E6%8C%87%E6%A8%99%E6%B3%95-two-pointers-%E8%A7%A3%E9%A1%8C-f038dd58e2b1)
>
> 5. [283.\[圖解\]Move Zeroes](https://medium.com/@urdreamliu/283-%E5%9C%96%E8%A7%A3-move-zeroes-4da4900f5aac)
>
> 6. [LeetCode 刷题进大厂(2)— Two Pointers](https://juejin.cn/post/7005094716278571022)
>
> 7. [【第十天 - Two-pointer 介绍】](https://www.25230.com/17200404/20042.html)
>
> 8. [【演算法】Two-pointer雙指標](https://venturescripts.com/%E3%80%90%E6%BC%94%E7%AE%97%E6%B3%95%E3%80%91two-pointer%E9%9B%99%E6%8C%87%E6%A8%99/)