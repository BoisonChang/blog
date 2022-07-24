---
title: "LeeCode: Hashing 類型筆記 (Easy)"
date: 2022-07-24T15:33:27.359Z
author: Boison
slug: leetcode-hashing-note-easy
tags:
  - LeetCode
  - Algorithms
  - Hashing
draft: false
---
以下先將做題過程常用到的方法列出，在依序對應不同題目解法:

1. ES6: Map 物件，特性是有順序性
2. Array 的值轉成可查找的物件 key
3. String 轉為計算出現次數的物件
4. String 轉為存放原始內容的物件
5. 製作 ASCII 值對應的字典
6. 將 Map 轉成 Array

---

## 一、ES6: Map 物件，特性是有順序性

> 特性如下，詳細用法請見 [ES6: Map 物件，特性是有順序性](https://boison.tw/2022/07/es6-map/) 一文

* Map 的 key 是可以是原始型別、物件、函式

  * Object 的 key 限定為 string、 symbol

* Map 可以使用 array 的方法，像是 forEach

* Map 中的 keys 有順序性

* 需要頻繁的新增刪除屬性或找東西時，Map 的效能會比 Object 更好

* Map 物件瀏覽器的支援度卻相當的好，只有 IE 不支援

---

## 二、Array 的值轉成可查找的物件 key

> **[1. Two Sum](https://leetcode.com/problems/two-sum/)**

```javascript
// BEST TC:O(N) SC:O(N)
var twoSum = function(nums, target) {
    let lookup = {}
    for(let i = 0; i < nums.length; i++) {
        let diff = target - nums[i]
        if(lookup[diff] >= 0) return [lookup[diff], i]
        lookup[nums[i]] = i
    }
}

/*
twoSum([2,7,11,15], 9)
i=0,target=9,nums[0]=2,diff=7,lookup={2:0}
i=1,target=9,nums[0]=7,diff=2,lookup={2:0} return [0,1]


twoSum([3,7,4,3,6], 11)
i=0,target=11,nums[0]=3,diff=8,lookup={3:0}
i=1,target=11,nums[1]=7,diff=4,lookup={3:0, 7:1}
i=2,target=11,nums[2]=4,diff=7,lookup={3:0, 7:1} return [1,2]
*/
```

## 三、String 轉為計算出現次數的物件

> **[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)**

```javascript
// BEST TC:O(N) SC:O(1)
var isAnagram = function(s, t) {
    if(s.length !== t.length) return false
    const map = new Map()
    s.split('').forEach((c)=>{
      map[c] =  map[c] + 1 || 1
    }) 
    for(item of t){
        if(!(map[item] > 0)) return false
        map[item] -= 1
    }
    return true
}
```

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

## 四、String 轉為存放原始內容的物件

> **[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)**[ ](https://leetcode.com/problems/group-anagrams/)

```javascript
// BEST/EASY TC:O(N) SC:O(N)
var groupAnagrams = function(strs) {
  let hashTable = {}, str = ''
  for (var i = 0; i < strs.length; i++) {
    str = strs[i].split('').sort().join('')
    if (!hashTable[str]) hashTable[str] = []
    hashTable[str].push(strs[i])
  }
  return Object.values(hashTable)
}

/*
let strs = "Hello World"
let hashTable = {}, str = ''
for (var i = 0; i < strs.length; i++) {
  str = strs[i].split('').sort().join('')
  if (!hashTable[str]) hashTable[str] = []
  hashTable[str].push(strs[i])
}
hashTable 
console.log(hashTable)

" ": [' ']
H: ['H']
W: ['W']
d: ['d']
e: ['e']
l: (3) ['l', 'l', 'l']
o: (2) ['o', 'o']
r: ['r']

*/
```

---

## 五、製作 a-z 的 ASCII 值對應的字典

> **[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)**

```javascript
// 單就 a-z 的 ASCII 
// BEST TC:O(N) SC:O(1)
var isAnagram = function(s, t) {
    let alpha = new Array(26).fill(0)
    for (let i = 0; i < s.length; i++) {
        alpha[s.charCodeAt(i) - 97]++
    }
    for (let i = 0; i < t.length; i++) {
        alpha[t.charCodeAt(i) - 97]--
    }
    return alpha.every(i => i === 0)
} 

// 就所有的 ASCII 
// BEST TC:O(N) SC:O(1)
const CHARS_NUMBER = 256
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false
    const charCount1 = new Array(CHARS_NUMBER).fill(0);
    const charCount2 = new Array(CHARS_NUMBER).fill(0);
    for (let i = 0; i < s.length; i++) {
      const code1 = s.charCodeAt(i)
      const code2 = t.charCodeAt(i)
      charCount1[code1]++
      charCount2[code2]++
    }
    for (let i = 0; i < charCount1.length; i++) {
    	if (charCount1[i] !== charCount2[i]) return false;
    }
    return true
}
```

> **[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)**[ ](https://leetcode.com/problems/group-anagrams/)

```javascript
// BEST TC:O(N) SC:O(1)
// Map 版本
var groupAnagrams = function(strs) {
  const map = new Map()  
  for (let str of strs) {
    const key = shuffle(str)
    if (!map.has(key)) map.set(key, [])
    map.get(key).push(str)
  }
  return Array.from(map.values())
}

const shuffle = (str) => {
  const arr = new Array(26).fill(0)  
  for (let char of str) {
    const curr = char.charCodeAt() - 'a'.charCodeAt()
    arr[curr]++
  }  
  return arr.join('-')
}


// BEST TC:O(N) SC:O(1)
// Object 版本
var groupAnagrams = function(strs) {
  const hashTable = {}
  for (let str of strs) {
    const key = shuffle(str)
    if (!hashTable[key]) {
      hashTable[key] = [str]
    } else {
      hashTable[key].push(str)
    }
  }
  return Object.values(hashTable)
}

const shuffle = (str) => {
  const arr = new Array(26).fill(0)  
  for (let char of str) {
    const curr = char.charCodeAt() - 'a'.charCodeAt()
    arr[curr]++
  }  
  return arr.join('-')
}


/*
shuffle('any')
'1-0-0-0-0-0-0-0-0-0-0-0-0-1-0-0-0-0-0-0-0-0-0-0-1-0'
shuffle('nya')
'1-0-0-0-0-0-0-0-0-0-0-0-0-1-0-0-0-0-0-0-0-0-0-0-1-0'
*/
```

> 參考資料：[ASCII 控制字符(美國標準資訊交換標準碼)](http://www.eion.com.tw/Blogger/?Pid\=1128)

---

## 六、將 Map 轉成 Array

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