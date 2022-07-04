---
title: "LeetCode: Array 類型筆記 (Easy)"
date: 2022-07-04T03:05:42.288Z
author: Boison
slug: Leetcode-array-note-easy
tags:
  - Leetcode
  - Algorithm
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

---

## 一﹑應用 Set 之問題

要了解原理可先見 [ES6: Set 物件，特性是元素不重複](https://boison.tw/2022/07/es6-set/) 一文

> [217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

```javascript
//  BEST TC:O(1) SC:O(N)
function hasDuplicates(nums) {
    return new Set(nums).size !== nums.length
}
```

> [448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

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

> [136.Single Number](https://leetcode.com/problems/single-number/)

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

## 二、應用 for 代替 forEach 之問題

> [217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

```javascript

//  EASY TC:O(N^2) SC:O(N)
function hasDuplicates(nums) {
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

> [448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

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

![](https://miro.medium.com/max/700/1*VlmrccsF58VKbyi68LbGkA.png)
圖片來源: [XOR 位元運算子](https://medium.com/@hyWang/xor-%E4%BD%8D%E5%85%83%E9%81%8B%E7%AE%97%E5%AD%90-1c25b4ae15fb)

> [136.Single Number](https://leetcode.com/problems/single-number/)

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
要

> [217.Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

```javascript
//  EASY TC:O(N^2) SC:O(1)
function hasDuplicates(nums) {
    return nums.some(x => nums.indexOf(x) !== nums.lastIndexOf(x))
}
```

> [136.Single Number](https://leetcode.com/problems/single-number/)

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

> [268.Missing Number](https://leetcode.com/problems/missing-number/)

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

> [136.Single Number](https://leetcode.com/problems/single-number/)

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

> [448.Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

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

> [2022.Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array/)

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