---
title: "Sliding Window: 常用來解決字串問題"
date: 2022-07-29T02:44:16.929Z
author: Boison
slug: algorithms-sliding-window
tags:
  - Algorithms
  - SlidingWindow
draft: false
---
> **Sliding Window 通過不斷調整子序列的 start & end 位置，獲取滿足要求解**

Sliding Window 可以算是廣義的左右指標中的一種，但是在某些情況下，Sliding Window 可以只使用一個 point 與 一個 window size 來實作，並不用真正使用到兩個指標。

> **左右指標 (反向指標) [可見 Two Pointer: 低空間複雜度的解題技巧](https://boison.tw/2022/07/algorithms-two-pointer/)一文**

一般來說若給定的資料是線性結構，例如 array, linked list 或 string 等可以循序存取 (sequential access)，而題目要求找出滿足特定條件的最長/最短的字串、陣列或一個目標值，常可以使用 Sliding Window。

![](https://miro.medium.com/max/1400/1*HN084lMD15SWjH6epVeSAg.gif)

圖片來源: [Interview Guide Series — Sliding Window](https://medium.com/master-the-coding-interview-lazy-edition/interview-guide-series-sliding-window-4e6670e079e6)

> **Sliding Window 特性**

1. 在線性時間內完成

2. 透過操控 two pointer 找出滿足條件的區間

3. 不管是搜索的範圍或最終答案，一定都是 continuous 的資料，continuous 代表資料可以被依序存取

> **Sliding Window 常見題目**


* [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```javascript
// BEST
function lengthOfLongestSubstring(s) {
    const set = new Set()
    let start = 0, length = 0
    for (let end = 0; end < s.length; end++) {
        while (set.has(s[end])) set.delete(s[start]), start++
        set.add(s[end])
        length = Math.max(length, set.size)
    }
    return length
}

lengthOfLongestSubstring('bbs') // 2
/*
end=0,set=[b  ],length=1
end=1,set=[   ],length=1
end=1,set=[b  ],length=1
end=2,set=[b,s],length=1
end=2,set=[b,s],length=2
*/
```

* [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

* [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

* [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

---

> **參考資料**
>
> 1. [Sliding Window – 陪你刷題](https://haogroot.com/2020/09/16/sliding-window-leetcode/)
>
> 2. [Two Pointer 與Sliding Window](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-two-pointer-%E8%88%87sliding-window-8742f45f3f55)
>
> 3. [Leetcode 刷題 pattern - Sliding Window](https://blog.techbridge.cc/2019/09/28/leetcode-pattern-sliding-window/)
>
> 4. [滑動視窗（Sliding Window）演算法介紹](https://www.gushiciku.cn/pl/2Epc/zh-tw)
>
> 5. [Reliable Transmission - Sliding Window Algorithm](https://hackmd.io/@0xff07/network/https%3A%2F%2Fhackmd.io%2F%400xff07%2FrJEP4f62Z)
>
> 6. [【演演算法】滑動視窗三步走](https://www.it145.com/9/118070.html)
>
> 7. [Leetcode 必备算法：聊聊滑动窗口](https://developer.51cto.com/article/689828.html)