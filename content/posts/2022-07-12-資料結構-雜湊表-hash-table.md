---
title: "資料結構: 雜湊表（Hash Table）"
date: 2022-07-12T06:30:55.198Z
author: Boison
slug: ds-hashtable
tags:
  - DataStructure
  - HashTable
draft: false
---
> **雜湊表很適合來存放不確定數量大小的資料，查找也很快速 (時間複雜度為 O(1))**

如果我們想要讀取儲存的資料時間複雜度都在 O(1)，最適合的方式是用到 Array，把 Key 當作 array 的索引值 index，value 儲存到 array，這樣稱為 **Direct Access Table**。

> **但這樣作有缺點，如果 key 範圍很大但數量很少，無法讓 key 連續是浪費記憶體空間**

Hash-Table 解決了浪費記憶體空間的問題，能把放資料的 table 大小壓縮至接近真正需要放進 table 資料的數量。為了達到這個目標引入了 Hash function，公式為 index=h(Key)，可以把原本雜散的 key 對應到 hash table 的緊湊有順序的索引值 index。

- - -

## 一、常見的雜湊函式(Hash Function)

主要是將不定長度訊息的輸入**雜湊函式**，會演算成固定長度雜湊值的輸出。

> **所計算出來的雜湊值必須符合兩個主要條件**

* 由雜湊值是無法反推出原來的訊息
* 雜湊值必須隨明文改變而改變

### 1. 除法 (Mod/Division)

相除取餘數來當作雜湊值。

```javascript
4 mod 11 = 4
```

### 2. 中間平方法 (Middle Square)

將值平方後，再取適當的中間位數作為雜湊值。

```javascript
235 X 235 = 55225，取中間三位數，雜湊值為522。
```

### 3. 折疊相加法 (Folding Addition)

相加方式分為兩種。

1. Shift(移位)
2. Boundary(邊界)

```javascript
987586265，拆分成 3 段相加，987+586+265，雜湊值為 1838

987586265，拆分成 3 段 987+586+265，可分為基數段反轉或偶數段反轉
偶數段反轉為: 987+685+265， 3 段相加雜湊值為 1937
```

### 4. 數位分析法 (Digits Analysis)

假設欄位資料已知，又屬於同一位數，若很集中則捨棄該位，若很分散則挑選該位。

```javascript
例如: 手機號碼欄位，都是 09 開頭 + 8位號碼，捨去 09，取用 8 位號碼作為雜湊值。
```

- - -

## 二、雜湊表 (Hash Table)

用雜湊函數運算出來的雜湊值，根據 **鍵 (key)** 來儲存在數據結構中。

> 存放這些記錄的數組就稱為 **雜湊表**

舉例來說，我們有一筆資料用字典的形式表示，每個名字都搭配性別

```javascript
{Joe:'M', Sue:'F', Dan:'M', Nell:'F', Ally:'F', Bob:'M'}
```

而我們將每個名字經過雜湊函數的運算。

```javascript
(Key)                 (hash value)     (stored index)
Joe  → (Hash function) →   4928   mod 5   =   3
Sue  → (Hash function) →   7291   mod 5   =   1
Dan  → (Hash function) →   1539   mod 5   =   4
Nell → (Hash function) →   6276   mod 5   =   1
Ally → (Hash function) →   9143   mod 5   =   3
Bob  → (Hash function) →   5278   mod 5   =   3
```

hash value 是獨一無二的，用 mod 5 來得到餘數並儲存才記憶體中。

```javascript
0： 
1： [ Sue, F ] → [ Nell, F ]
2： 
3： 
4： [ Joe, M ] → [ Ally, F ] → [ Bob, M ]
5： [ Dan, M ]
```

當我們要找資料的時候，例如 Ally 的性別，我們就把 Ally 丟到名為 Hash 的果汁機來得到 hash value，再用 mod 5 找到儲存在記憶體中的位置，但記憶體中的第一個位置並不是 Ally 是 Joe，我們根據 Joe 的鏈結找到下一個元素，直到找到答案。

> **另外一個 Hash Table 範例**

![](https://miro.medium.com/max/1336/0*0xeEq-iJDrVAU0zG.png)

圖片來源: [用JavaScript學習資料結構與演算法 7：雜湊表](https://break0344.medium.com/data-structures-and-algorithms-7-hashtables-c00605cfd5e6)

- - -

## 三、碰撞 (Collision)

但 Hash function 不是完美的，有時候會發生不同 key 對應到相同 index 的情形，稱為碰撞 collision，碰撞有兩種常見的處理方式。

### 1. Separate Chaining

Separate Chaining 藉由使用陣列或連結串列，將複數的資料儲存在表中的同一位置，因此在查找時會先指向表中的某一位置的陣列或連結串列，並對該資料結構進一步的進行查找，實際操作上可使用 Linked list 把 Hashing 到同一個 slot 的資料串起來。

### 2. Open Addressing

當碰撞產生時，會繼續查找表中的下一個位置，若無碰撞則將資料存入該位置，反之則繼續查找，當發生碰撞時，使用線性探測 (Linear Probing) 以線性的方式來往後尋找 Table 中空的 slot 存放資料，一找到就將資料放入。這種方法通常把雜湊視為環狀結構，一旦後面的位置被填滿而前面還有位置時，可以將資料往前放。

- - -

> **參考資料**
>
> 1. [【Day9】\[資料結構]-雜湊表Hash Table](https://ithelp.ithome.com.tw/articles/10268077)
> 2. [Day 12 資料結構：雜湊表 Hash Table](https://ithelp.ithome.com.tw/articles/10273568)
> 3. [用JavaScript學習資料結構與演算法 7：雜湊表](https://break0344.medium.com/data-structures-and-algorithms-7-hashtables-c00605cfd5e6)
> 4. [字典（Dictionary）和雜湊表（Hash Table）篇](https://blog.techbridge.cc/2017/03/10/javascript-data-structure-algorithm-dictionary-hash-table/)
> 5. [Hashing (雜湊) 原理介紹](https://blog.kennycoder.io/2020/02/18/%E8%B3%87%E6%96%99%E7%B5%90%E6%A7%8B%E8%88%87%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98-Hashing-%E9%9B%9C%E6%B9%8A%E5%8E%9F%E7%90%86%E4%BB%8B%E7%B4%B9/)
> 6. [雜湊 (Hash)](https://ithelp.ithome.com.tw/articles/10208884)
> 7. [JS 資料結構 hash-table 筆記](https://medium.com/@nicehorse06/js-%E8%B3%87%E6%96%99%E7%B5%90%E6%A7%8B-hash-table-9eed0e7b6f46)
> 8. [擁抱「資料結構」的「演算法」(16) - 雜湊 Hash](https://ithelp.ithome.com.tw/articles/10246777)