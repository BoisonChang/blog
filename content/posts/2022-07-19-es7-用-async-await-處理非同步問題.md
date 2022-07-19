---
title: "ES7: 用 Async/Await 處理非同步問題"
date: 2022-07-19T03:44:54.942Z
author: Boison
slug: ES7-Async-await
tags:
  - ES7
  - AsyncAwait
draft: false
---
> **async/await 是 Promise 的語法糖，提升了 Promise 在鏈式寫法的可讀性**

async/await 是 ES7 的寫法，可以讓非同步 call back 寫法看起來像同步的順序去執行。

1. **async**

   * async 直接翻譯成中文是異步，也就是非同步的意思

   * 若要使用 async 會將 async 放在函式旁，代表這個函式接下來會以同步方式來執行非同步的語法

   * async function 被呼叫的時候，它會回傳一個 Promise 

     * 當 async function 回傳一個值，該 Promise 狀態會是 resolved 且帶有該回傳值

     * 當 async function 拋出一個例外或某個值，該 Promise 狀態會是 rejected 且帶有該拋出的值

2. **await**

   * await 翻譯成中文，則是等待

   * 一般是放在 async 函式中的 Promise 事件旁，代表接下來的程式碼會等待 await 方法完成後才執行

   * await 會暫停 async function 的執行，並且等待傳遞至表達式的 Promise 解析完之後回傳解析值，才繼續執行 async function 

```javascript
function PromiseFn(String) {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      if (String.length >= 5) {
        resolve(`Promise 成功， ${String} 共 ${String.length} 個字 `)
      } else {
        reject('Promise 失敗')
      }
    }, 2000)
  })
}

async function usePromise() {
  const data1 = await PromiseFn('Ryder')
  console.log(data1)                       // Promise 成功， Ryder 共 5 個字
  const data2 = await PromiseFn('test')
  console.log(data2)                       // Promise 失敗
  const data3 = await PromiseFn('youtube')
  console.log(data2)                       // (不執行)
}

usePromise()
```

使用 async/await 的寫法 ，只要使用 await 的 Promise 只要一失敗，接下來會回傳 Promise 的 reject 資料，接著 await 下的程式碼都不會被執行。

這種狀況會容易造成不清楚錯誤來源，在維護上會較為困難，也因此 async/await 的寫法，有提供另一種將 Promise 執行成功、失敗分開的寫法，就是 try…catch 。

```javascript
function PromiseFn(String) {
  return new Promise((resolve, reject) => {
    setTimeout(function () {
      if (String.length >= 5) {
        resolve(`Promise 成功， ${String} 共 ${String.length} 個字 `)
      } else {
        reject('Promise 失敗')
      }
    }, 2000)
  })
}

async function usePromise() {
  try{
    const data1 = await PromiseFn('Ryder')
    console.log(data1)                    // Promise 成功， Ryder 共 5 個字 
    const data2 = await PromiseFn('test') // Promise 執行 catch
    console.log(data2) 
    const data3 = await PromiseFn('youtube')
    console.log(data2)                    // (不執行)
  } catch (err) {
    console.log('catch', err)             // catch Promise 失敗
  }
}

usePromise()
```

要注意的是 try 中的 Promise 執行失敗，是會執行 catch 中的程式碼，而 try 中的程式碼一樣不會繼續執行下去。

---

> **參考資料**
>
> 1. [Promise 語法糖 async/await](https://ithelp.ithome.com.tw/articles/10271869?sc\=iThelpR)
>
> 2. [簡單理解 JavaScript Async 和 Await](https://www.oxxostudio.tw/articles/201908/js-async-await.html)
>
> 3. [Async function / Await 深度介紹](https://www.casper.tw/development/2020/10/16/async-await/)
>
> 4. [Async and Await in JavaScript](https://pjchender.dev/javascript/js-async-await/)
>
> 5. [認識 async/await](https://ithelp.ithome.com.tw/articles/10232355)