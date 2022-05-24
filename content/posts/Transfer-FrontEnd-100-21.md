---
title: "百日轉職前端工程師：第十三週正規表達式 《DAY 21》"
slug: Transfer-FrontEnd-100-21
date: 2020-11-19T00:40:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "RegExp"]
author: "Boison"
showToc: false
TocOpen: false
hidemeta: false
comments: true
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
---


大家好，這是百日轉職前端工程師的 Day21，也是 11/18（三），今天要來聊聊滿常見的正規表達式（Regular Expression） ，這是一個新手看到會昏頭看起來很像亂碼的工具，但是實際接觸後發現沒有那麼難，於是簡單來整理一下其用法，簡單來說正規表達式是被用來匹配字串中字元組合的模式，實際上其應用廣泛，從文本資料分析查詢到人工智慧的腳本編寫，都能看到正規表達式的使用。

---

## 一、什麼是正規表達式？

> **事實上正規表達式並不是程式語言，只是一款「表達字串形式」的「邏輯式」**
- 1956 年由數學家 Stephen Kleene 所提出，包括 JavaScript, Perl, Python, Java, C/C++ 在內的多種程式語言都能支援正規表達式
- 正規表達式乃身為一個程式工程師所具備的基本常識其在上述語言中都能跑得動，只要學會正規表達式、就能跨平台使用且十年以上不退流行


比方說當你要在一文本中查詢單字，但忘記信封是拼做 envelope  ，還是 envlope 時，你可以用 enve?lope 去查詢，當你忘記灰色是 grey 還是 gray 時，你可以用 `gr[ae]y` 去查詢，而當你同時想查詢 wow、woow 跟 wooow 時，你可以寫成 wo+w。

> **聽起來很方便他可以短短一行就取代一個迴圈，但他也有以下缺點**
- 易讀性差：讀起來很困難，甚至連你自己都忘了在寫啥
- 維護性差：很難去修改微調應用不同情境

所以通常會用在一些不會重複需要使用的小地方，而不會把大的、核心的專案部分用正規表達式來處理。

---

## 二、正規表達式的使用情境

以下三種情境都能透過正規表達式處理，快速的解決。

### 情境 1：尋找資料

尋找資料庫中某個資料，但可能以些微差異儲存，例如要找蘋果的英文單詞，但資料庫中有 apple 也有 Apple，甚至有人手誤的打成 applo。

### 情境 2：驗證資料

比方說在註冊需要填寫生日，但有人寫成 1991-07-20 、有人寫成 19910720，也有人寫成 1991/07/20 但這三種寫法我驗證後都要讓其通過的話。

### 情境 3：抽取資料

比方說我有以下五筆資料，但我只想抽取信箱網域（gmail、yahoo…）名稱。
- aaa@gmail.com
- ccc@gmail.com
- ddd@yahoo.com.tw
- eee@msn.com
- fff@ptt.com

---

## 三、正規表達式的寫法

可以在這個網站  RegEx101 提供你的 RE 以及要比對的字串，試試看你的正規表達式寫法是否正確抓到你想要的資料。

### 1. 正規表達式基本寫法

- 你可以用 你可以用 `/ant/` 去找出一個字串有沒有包含 ant，還有 ant 出現在哪個位置
- 如果你想要找出所有含 a 和 n 和 t 的字串，則可以用 `/[ant]/` 去表示
- 那如果你想要去看是否有包含「數字」和「英文字」 ，你當然可以全部寫出來，但有更簡便的寫法如：`/[0-9]/` 跟 `/[a-z]/`
- 若是要找大寫的英文字母呢？ `/[A-Z]/` 且可以合併使用
- 比方說你要找含有大小寫英文字母和數字要怎麼寫？`/[0-9a-zA-Z]/` 即可


> **但以下提供再更簡化的寫法**
- 查數字
    - `/\d/ = /[0-9]/`
- 查三個連續的數字
    - `/\d\d\d/`
- 查大小寫數字和底線
    - `/\w/ = /[a-zA-Z0-9_]/` 
- 查任意一個字元
    - `/./`

特別注意在程式碼中字串中要把` \d` 改成 `\\d`，不然的話會被當作跳脫字元來看待。

> **但是你用上述的方法會查到所有「包含」上述字元條件的字串，如果你想要查開頭或者結尾包含呢？**

- 查開頭包含 xyz 的字串
    - ` /^xyz/`
- 查結尾包含 abc 的字串
    - `/abc$/`
- 查開頭是 0988 的八位手機號碼
    - `/^0988d\d\d\d\d\d$/` 

> **如果是要找重複的連續字元可以再簡化**
- 找重複的連續字元
    - `/^0988\/ = /^0988\d{4}$/`
- 查開頭是 0988 後面重複 4 次以上的連續數字
    -`/^0988\d{4,}$/` 
- 查開頭是 0988 後面重複 4~7 次的連續數字
    - `/^0988\d{4,7}$/`
- 查開頭是 0988 後面是不固定次數的連續數字 
    - `/^0988\d+$/`

### 2. 正規表達式實際用法

實際使用上一個例子是要定義三組變數：(1). 你要尋找的字元，(2). 被搜尋的字串，(3). 匹配的結果。

```
var re = /李.明/
var str = '李曉明王阿明王小明李大明太大明阿明無名小站'
var result = str.match(re)
console.log(result)
/*
輸出：
0: "李曉明"
groups: undefined
index: 0
input: "李曉明王阿明王小明李大明太大明阿明無名小站"
*/
```

若是要找全部出現的字元的話，要在 // 後面加一個 g（golobal）如 /李.名/g ，並且使用 str.matchAll(re)。

> **最後再介紹個工具 Capturing Groups ()，加在正規表達式中可以幫助你抓出專有名詞，來解決前面的信箱網域名稱問題。**

```
var emails = [
'aaa@gmail.com',
'ccc@gmail.com',
'ddd@yahoo.com.tw',
'eee@msn.com',
'fff@ptt.com'
]
var re = /^.+@(.+?)\./
for(let email of emails) {
var result = email.match(re)
console.log(result[1])
}
/*
gmail
gmail
yahoo
msn
ptt
*/
```

> **拆解一下上述的匹配式如下**
- `^.+@ `
    - 是代表開頭是數字或者大小寫英文字母，連續 N 次接一個 @
- `(.+?)`
    - 抓出數字或者大小寫英文字母任意個英文字母，? 代表捉出匹配的字元越少越好
- `^.+@(.+?)\.`
    - 則是抓出` ^.+@(.+?)\` 這串的「專有名詞」，匹配最後接上一個大小寫英文字母任意個英文字母
    - 特別注意在字串中要把 `.` 改成 `\.` 否則會被當跳脫字元

---

## 四、總結

以上大概用最簡單的方式介紹了正規表達式（Regular Expression）的概念，但要完整瞭解所有用法請參考以下本文之參考延伸資料。


> 參考資料
1. [胡立 - 簡易 Regular Expression 入門指南](https://blog.huli.tw/2020/05/16/introduction-to-regular-expression/)
2. [[Javascript] 初探Regex 正規表達式](https://moojing.medium.com/javascript-%E5%88%9D%E6%8E%A2regex-%E6%AD%A3%E8%A6%8F%E8%A1%A8%E9%81%94%E5%BC%8F-1da2f4d94795)
3. [寫點科普 - 文本分析基礎-正規表達式(RegExp)](https://kopu.chat/%e6%96%87%e6%9c%ac%e5%88%86%e6%9e%90%e5%9f%ba%e7%a4%8e-%e6%ad%a3%e8%a6%8f%e8%a1%a8%e9%81%94%e5%bc%8fregexp/)
4. [Regular Expressions for Regular Folk](https://refrf.dev/)