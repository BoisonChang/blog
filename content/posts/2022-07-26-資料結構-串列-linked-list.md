---
title: "資料結構: 串列 Linked List"
date: 2022-07-26T03:31:24.956Z
author: Boison
slug: dsa-linked-list
tags:
  - DataStructure
  - LinkedList
draft: false
---
> **Linked List 是線性的資料結構由節點 (node) 構成，每個節點有兩個部分** 
>
> 1. **資料 val**: 存放的資料
> 2. **指針 next**:  指向下一節點

和熟悉的陣列不同的是，Linked List 在記憶體存放順序並不是連續的，所以沒有 index 的概念，如果需要查找資料，不能像陣列一樣直接用 array\[index] 去找，而一定要從頭開始一個一個找。

![](https://static.studytonight.com/data-structures/images/array-vs-linked-list.png)

圖片來源：[Difference between Array and Linked List](https://www.studytonight.com/data-structures/linked-list-vs-array)

- - -

## 1. Linked List v.s. Array

> **Linked List在刪除和新增資料時，比陣列時間複雜度還要低**

* 陣列因為每次刪除/新增都需要把陣列的其他元素「往後排」或是「往前補」，所以時間複雜度是 O(n) 
* Linked List 只要透過斷掉鏈結、重新接上的方式，便可以刪除/新增一筆資料，時間複雜度是 O(1)。

|         | **Linked List**                           | **Array**               |
| ------- | ----------------------------------------- | ----------------------- |
| 說明      | 類似把很多個物件關聯起來，並有順序性                        | 同一記憶體空間的一個區間            |
| 記憶體     | 不需要連續的記憶體空間，因此不需要預留空間                     | 需要連續的記憶體空間，宣告時就需要決定空間大小 |
| 資料插入/刪除 | 相對快，插入和刪除本身是 O(1)，但有時需要先搭配搜尋 O(n) 來找到節點位置 | 相對慢，插入與刪除均為 O(n)        |
| 資料取出    | 相對慢，要從頭開始找 O(n) 到後取出                      | 相對快，可以直接使用索引（index）取出   |
| 適用時機    | 1. 無法預期資料數量時                              | 1. 需要快速取出資料時            |
|         | 2. 需要頻繁增刪資料時                              | 2. 不需要頻繁增刪資料時           |
|         | 3. 不需要頻繁查詢並取出資料時                          | 3. 資料的數量不會有太大變更時        |

資料來源: [\[資料結構] Array and Linked List](https://pjchender.dev/dsa/dsa-array-linked-list/)

- - -

## 2. Linked List 實作

> **簡單用 JavaScript 實作模擬多個 Node組合成一個Linked List**

```javascript
class ListNode {
  constructor(data, next = null) {
    this.data = data
    this.next = next
  }
}
let n1 = new ListNode(2), n2 = new ListNode(4), n3 = new ListNode(6)
n1.next = n2
n2.next = n3

console.log(n1)
/*
ListNode {
  data: 2,
  next: ListNode {
    data: 4,
    next: ListNode {
      data: 6,
      next: null 
  } 
} 
*/
```

- - -

## 3. Linked List 的 Method

> **由於資料結構特性， Linked List 一般會有的一些方法**

以下只列出功能，有興趣看程式碼用 JavaScript 實作請見參考資料。

```javascript
class ListNode {
  constructor(data, next = null) {
    this.data = data
    this.next = next
  }
}
class LinkedList {
  constructor() {
    this.root = null
  }
  toString()               {...}     // 用以呈現 linked list 字串模樣
  length()                 {...}     // 回傳 linked list 長度
  isEmpty()                {...}     // 判斷 linked list 是否有節點
  get(index)               {...}     // 取得指定節點
  removeAtIndex(index)     {...}     // 在 linked list 移除節點
  addAtIndex(index, value) {...}     // 在 linked list 增加節點
}
```

![](https://miro.medium.com/max/1400/1*v9uMEKfoRPHe1KUlLRH0RA.gif)

圖片來源: [What Are Linked Lists?](https://medium.com/@avinash.sarguru/what-are-linked-lists-ccdc19694b69)

![](https://imgs.developpaper.com/imgs/3314328623-7162b0384de0ae0f_articlex.gif)

圖片來源: [Basic summary of linked list](https://developpaper.com/basic-summary-of-linked-list/)

- - -

## 4. Linked List 的類型

1. **單向鏈結串列（Singly Linked List）**

   * 最基本的鏈結串列，其特點是鏈結串列的鏈結方向是單向的
2. **雙向鏈結串列（Doubly Linked List）**

   * 最大的區別在於，每個結點中都有兩個指標，分別指向上一個和下一個結點
3. **迴圈鏈結串列（Circularly Linked List）**

   * 與一般的鏈結串列操作基本一致，但串列頭尾的指標會連接再一起形成一個環

- - -

## 5. 常見題目

> [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
*/
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
*/
// BEST&EASY
var mergeTwoLists = function(l1, l2) {
    const dummy = new ListNode(-Infinity)
    let prev = dummy
    while(l1 && l2){
        if(l1.val <= l2.val){
            prev.next = l1
            prev = l1
            l1 = l1.next
        } else {
            prev.next = l2
            prev = l2
            l2 = l2.next
        }
    }
    if (!l1 || !l2)  l1 ? prev.next = l1 : prev.next = l2
    return dummy.next
}

// BEST
var mergeTwoLists = function(l1, l2) {
    if (!l1 || !l2) return l1 ? l1 : l2
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    } else {
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
    }
}

mergeTwoLists([1,2,4],[1,3,4]) // [1,1,2,3,4,4]


mt({ListNode} [1,2,4], {ListNode} [1,3,4])      // (1 -> 2) (1 -> 3)
l2.next = mt(l1, l2.next)                       // (1 -> 2) (3 -> 4)
           = l1.next = mt(l1.next, l2)          // (2 -> 4) (3 -> 4) 
             = l1.next = mt(l1.next, l2)        // (4 ->  ) (3 -> 4) 
               = l2.next = mt(l1, l2.next)      // (4 ->  ) (4 ->  )  
                 = l2.next = mt(l1, l2.next)    // (4 ->  ) (      )
                   l2.next = 4  // (4 ->  ) (4 -> 4)
                 l2.next = 4    // (4 ->  ) (3 -> 4 -> 4)
               l1.next = 3      // (2 -> 3 -> 4 -> 4) (3 -> 4 -> 4)  
             l1.next = 2        // (1 -> 2 -> 3 -> 4 -> 4) (3 -> 4 -> 4)
        =  2                    // (1 -> 2 -> 3 -> 4 -> 4) (1 -> 1 -> 2 -> 3 -> 4 -> 4)
```

- - -

> 資料來源
>
> 1. [\[資料結構] Array and Linked List](https://pjchender.dev/dsa/dsa-array-linked-list/)
> 2. [來了解鏈結串列(Linked List)並實作它吧!](https://ithelp.ithome.com.tw/articles/10217020?sc=rss.iron)
> 3. [JavaScript 學演算法（五）- 鏈結串列 Linked list](https://chupai.github.io/posts/200427_ds_linkedlist/)