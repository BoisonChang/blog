---
title: "LeetCode: Array 類型筆記（Easy）"
date: 2022-07-04T03:05:42.288Z
author: Boison
slug: Leetcode-array-note-easy
tags:
  - LeetCode
  - Algorithms
  - Array
draft: false
---
以下先將做題過程常用到的方法列出，在依序對應不同題目解法: 

1. Set: Set 物件常用在處理陣列中值重複的題目中
2. for 代替 forEach: 本身不會回傳值，所以要回傳值要用 for 迴圈
3. XOR: ^ 可以用來快速找出陣列中單獨出現的元素
4. indexOf & lastIndexOf: 可以用來找出陣列中不重複出現的數字
5. 陣列加總: 陣列加總後的總合常用來找出連續數列中缺失的數字
6. 取負法: 取負法可以直接對陣列本身做註記找出連續數列中缺失的數字
7. slice: slice 可以用來對陣列做一維轉二維的內容擷取的處理
8. 對陣列中的數字排序
9. 判斷是否不是數字

---

## 一﹑應用 Set 之問題

要了解原理可先見 [ES6: Set 物件，特性是元素不重複](https://boison.tw/2022/07/es6-set/) 一文

> **[217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)**

```javascript
//  BEST TC:O(1) SC:O(N)
var hasDuplicates = function(nums) {
    return new Set(nums).size !== nums.length
}
```

> **[448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)**

```javascript
// GOOD  TC:O(N) SC:O(N)
var findDisappearedNumbers = function(nums) {
  const set = new Set()
  for (let i = 0; i < nums.length; i++) {
    set.add(i + 1)
  }
  for (const num of nums) {
    set.delete(num)
  }
  return [...set]
}
```

> **[136.Single Number](https://leetcode.com/problems/single-number/)**

```javascript
// GOOD TC:O(N) SC:O(N)
var singleNumber = function(nums) {
    let set = new Set(nums)
    let sum = 0
    for (i of set){
      sum += i
    }
    sum = sum * 2
    for (j of nums){
      sum -= j
    }
    return sum 
}
```

>  **[128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/solution/)**

```javascript
// GOOD&BEST
var longestConsecutive = function(nums) {
   let max = 0
   nums = new Set(nums)
   for (let num of nums) {
      let big = num
      let small = num
      while (nums.delete(big + 1)) big++
      while (nums.delete(small - 1)) small--
      const diff = big - small + 1
      if (max < diff) max = diff
   }
   return max
}

longestConsecutive([2, 1, 5, 0,-3, 3, -2])

/*
big 和 small 每次同時記錄起始位置
一個往上走，一個往下走，最後紀錄兩者差距


[-3, -2, 0, 1, 2, 3, 5]
[0, 1, 2, 3]
4

[-3, -2, 0, 1, 2, 3, 5]
big=-3,  small=-3,  max=0
[-3, 0, 1, 2, 3, 5]
big=-2,  small=-3,  max=0
[-3, 0, 1, 2, 3, 5]
big=-2,  small=-3,  max=0,    diff=1
[-3, 0, 1, 2, 3, 5]
big=-2,  small=-3,  max=1,    diff=1
[-3, 0, 1, 3, 2, 5]
big=0,   small=0,   max=1,    diff=1
[-3, 0, 3, 2, 5] ←
big=1,   small=0,   max=1,    diff=1
[-3, 0, 3, 5] ←
big=2,   small=0,   max=1,    diff=1
[-3, 0, 5] ←
big=3,   small=0,   max=1,    diff=1
[-3, 0, 5]
big=3,   small=0,   max=1,    diff=4
[-3, 0, 5]
big=3,   small=0,   max=4,    diff=4
[-3, 0, 5]
big=5,   small=5,   max=4,    diff=1
*/
```

---

## 二、應用 for 代替 forEach 之問題

> **[217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)**

```javascript
//  EASY TC:O(N^2) SC:O(N)
var hasDuplicates = function(nums) {
    var keep = []
    for(var i in nums){
        if(keep.indexOf(nums[i])<0){
            keep.push(nums[i])
        } else {
            return true
        }
    }
    return false
}
```

> **[448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)**

```javascript
// EASY TC:O(N^2) SC:O(N)
var findDisappearedNumbers = function(nums) {
  const arr = []
  for(let i = 1; i <= nums.length; i++){
     if(!nums.some(x => x === i)){
       arr.push(i)
     }
  }
    return arr
}
```

---

## 三、應用 XOR 之問題

目前不太知道怎樣理解最正確，暫時理解為同樣的數字碰撞會消失，就像是打泡泡遊戲同樣顏色泡泡碰撞會消失最後留下單獨的數字。

![](https://miro.medium.com/max/700/1\*VlmrccsF58VKbyi68LbGkA.png)
圖片來源: [XOR 位元運算子](https://medium.com/@hyWang/xor-%E4%BD%8D%E5%85%83%E9%81%8B%E7%AE%97%E5%AD%90-1c25b4ae15fb)

> **[136.Single Number](https://leetcode.com/problems/single-number/)**

```javascript
// BEST TC:O(N) SC:O(1)
var singleNumber = function(nums) {
    let singleEl = nums[0]
    for (let i = 1; i < nums.length; i++) {
        singleEl ^= nums[i]
    }
    return singleEl
}
```

---

## 四、應用 indexOf & lastIndexOf  之問題

要了解原理可先見 [JavaScript 中 Array 的 Method 整理](https://boison.tw/2022/06/javascript-array-method/) 一文

> **[217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)**

```javascript
//  EASY TC:O(N^2) SC:O(1)
var hasDuplicates = function(nums) {
    return nums.some(x => nums.indexOf(x) !== nums.lastIndexOf(x))
}
```

> **[136.Single Number](https://leetcode.com/problems/single-number/)**

```javascript
// EASY TC:O(N^2) SC:O(1)
var singleNumber = function(nums) {
    for(let i of nums){
        if(nums.indexOf(i) === nums.lastIndexOf(i)){
          return i
        }
    }
}
```

---

## 五、應用陣列加總之問題

> **[268.Missing Number](https://leetcode.com/problems/missing-number/)**

```javascript
// BEST TC:O(N) SC:O(1)
var missingNumber = function(nums) {
    let sum = 0
    for(let i=0;i<nums.length;i++){
        sum += i+1-nums[i]
    }
    return sum
}
```

> **[136.Single Number](https://leetcode.com/problems/single-number/)**

```javascript
// GOOD TC:O(N) SC:O(N)
var singleNumber = function(nums) {
    let set = new Set(nums)
    let sum = 0
    for (i of set){
      sum += i
    }
    sum = sum * 2
    for (j of nums){
      sum -= j
    }
    return sum 
}
```

---

## 六、應用取負法之問題

第一次看非常難以理解的方法，簡單來說為不用到額外的空間，就用原始陣列本身的「位置」跟「正負」來標註資訊。

每個數字在他對應的位置改成負數，但每當有重複一個數字，就代表有少一個數字，重複的數字的位置就是少的那個數字。下面放上一張可能有助幫助理解但依舊對第一次看的人很難懂的圖。

![](https://i.imgur.com/AXob2u6.png)

![](https://i.imgur.com/8m9y5NR.png)

圖片來源: [透過 LeetCode 解救美少女工程師的演算法人生系列 第 21 篇](https://ithelp.ithome.com.tw/articles/10225673)

> **[448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)**

```javascript
// BEST  TC:O(N) SC:O(N)
var findDisappearedNumbers = function(nums) {
    let resultArr = []
    for(let i = 0; i < nums.length; i++) {
        let idx = Math.abs(nums[i]) - 1       
        if(nums[idx] > 0) {
            nums[idx] = -nums[idx]
        }
    }
    
    for(let j = 0; j < nums.length; j++) {
        if(nums[j] > 0) {
            resultArr.push(j+1)
        }
    }
    return resultArr
}
```

---

## 七、應用 slice 之問題

要了解原理可先見 [JavaScript 中 Array 的 Method 整理](https://boison.tw/2022/06/javascript-array-method/) 一文

> **[2022.Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array/)**

```javascript
// BEST TC:O(N) SC:O(N)
var construct2DArray = function(original, m, n) {
    const arr = []
    let nextIdx = 0
    if(m*n !== original.length) return arr
    for(let i = 0; i < m; i++) {
        arr.push(original.slice(nextIdx, nextIdx+n))
        nextIdx += n
    }
    return arr
}
```

---

## 八、對陣列中的數字排序

> **[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)**

```javascript
// BEST TC:O(N) SC:O(N)
var topKFrequent = function(nums, k) {
    let res = [], map = new Map()
    nums.forEach(n => map.set(n, map.get(n) + 1 || 1))
    let sortedArray = [...map.entries()].sort((a, b) => b[1] - a[1])
    for(let i = 0; i < k; i++) {
        res.push(sortedArray[i][0])
    }
    return res
}
```

---

## 九、判斷是否不是數字

`NaN` : 全名為 Not a Number ，屬於全域物件，代表不是數字

```javascript
isNaN('3')   // false
isNaN( 3 )   // false
isNaN('s')   // true
```

> **[394. Encode and Decode Strings](https://leetcode.cn/problems/decode-string/submissions/)**

```javascript
// BEST TC:O(N) SC:O(N)
var decodeString = function(s) {
    let mulStack = [], strStack = [], num = 0, res = ''
    for (const c of s) {   
        if (!isNaN(c)) {  
            num = num * 10 + (c - '0')
        } else if (c == '[') {  
            strStack.push(res)
            mulStack.push(num) 
            res = '' 
            num = 0
        } else if (c == ']') {  
            res = strStack.pop() + res.repeat(mulStack.pop())
        } else {                   
            res += c
        }
    }
    return res;
}

decodeString('2[a]3[b4[c]]e')

/*
e
ce
cccce
bcccce
bccccbccccbcccce
abccccbccccbcccce
aabccccbccccbcccce

     ,mulStack=[   ],strStack=[        ],num=0,res=''
c='2',mulStack=[   ],strStack=[        ],num=2,res=''
c='[',mulStack=[2  ],strStack=[        ],num=0,res=''
c='a',mulStack=[2  ],strStack=[        ],num=0,res='a'
c=']',mulStack=[   ],strStack=[        ],num=0,res='aa'
c='3',mulStack=[   ],strStack=[        ],num=3,res='aa'
c='[',mulStack=[3  ],strStack=['aa'    ],num=0,res=''
c='b',mulStack=[3  ],strStack=['aa'    ],num=0,res='b'
c='4',mulStack=[3  ],strStack=['aa'    ],num=4,res='b'
c='[',mulStack=[3,4],strStack=['aa','b'],num=0,res=''
c='c',mulStack=[3,4],strStack=['aa','b'],num=0,res='c'
c=']',mulStack=[3  ],strStack=['aa'    ],num=0,res='bcccc'
c=']',mulStack=[   ],strStack=[        ],num=0,res='aabccccbccccbcccc'
c='e',mulStack=[   ],strStack=[        ],num=0,res='aabccccbccccbcccce'
*/
```

> 參考資料：[\[第三週\] JavaScript - 把你對 isNaN 的不滿都說出來](https://yakimhsu.com/project/project_w3_Javasciprt_NaN.html)