---
title: Next.js 13 升級功能：Turbopack、 file-based routing，React Server Components
date: 2022-11-05T08:15:31.739Z
author: Boison
slug: next-13-release
tags:
  - Next
draft: false
---
> **向下版本支援，新增功能如 Turbopack、更完善的 file-based routing，更無痛的 React Server Components**

1. 打包工具採用 Turbopack

   * 次世代的打包速度（速度更優於早些新推出的 Vite）

2. 更完善基於資料夾的路由 router 系統

   * 過往放在 page 資料夾下的資料夾名稱即等於路由，現在 app 資料夾同之前，只要在資料夾下建立一個 page.js 檔案當作該路由主頁面，即可放置其他檔案

   * 將測試檔案，相關功能/頁面元件 Component 等集中放置同一資料夾集中管理

3. 更無痛的寫 React Server Components

   * 在 app 資料夾下放置的組件默認為 React Server Components，且在 page 資料夾下若有需求更無痛的寫 React Server Components

   * 在 page 資料夾下要寫 React Server Components，只要使用非同步處理，則不須去撰寫語法傳遞 client side 和 server side 的變數

※ 註：其他更新還有 next/font、next/image，和 next/link 不用手動增加 `<a>` 為子項的優化，詳情請見[官方文件](https://nextjs.org/blog/next-13#server-components)

---

## 一、打包工具採用 Turbopack

Turbopack 是打包工具 Webpack 外新推出的次世代打包工具，比之前新推出的 Vite 還要快，是由 Rust 語言編寫而成，即使 Vite 已經非常快，但採用 Turbopack 對於非常大的軟體開發上仍可能帶來非常有感的打包速度提升。

但若需要對於 Webpack 做非常多額外的設定，在 Vite 跟 Turbopack 的社群分享即相關套件可能都不如 Webpack，這點是需要留意和持續觀察的點。

> **更改設定檔 package.json 實現 Turbopack 打包**

```javascript
// Before Next.js 13  > package.json
{
...
"script": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  

},
...
}

//  Next.js 13 > package.json
{
...
"script": {
  "dev": "next dev --turbo",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  

},
...
}
```

> **一些網站整理 Turbopack 在特定環境下的比較數據如下**

* 更新速度比 Webpack 快 700 倍

* 更新速度比 Vite 快 10 倍

* cold starts 速度比 Webpack 快 4 倍

![](https://oscimg.oschina.net/oscnet/up-d5c360f539c002174dc3553b85d08a4fbdb.png =500x165)

圖片來源：[Turbopack vs Vite ](https://turbo.build/pack/docs/comparisons/turbopack-vs-vite)

※ 註：關於 Vite 的介紹，與 Vite 和 Webpack 的比較可見 [Vite 概念整理與快速建置](https://boison.tw/2022/06/vite-101/) 一文

---

## 二、更完善基於資料夾的路由 router 系統

過去在 page 資料夾下開資料夾，其資料夾名稱即會被讀成路由，該路由主頁面吃其下的 index.js 檔案，但一個資料夾中只能放一個 indes.js 檔案。

現在 next.js 改成在 app 資料夾下開資料夾，其資料夾名稱同樣被讀成路由，但現在只要在其中開一個檔案名稱為 page.js 當作該路由主頁面，即可放置其他檔案，可以將測試檔案，相關邏輯/UI 元件 Component 等集中放置同一資料夾集中管理。

> **app 資料夾下每一資料夾內默認必帶的檔案**

1. `page.tsx` : 該路由主頁面

> **app 資料夾下每一資料夾內非必帶默認的四支檔案**

1. `layout.tsx` : 該路由下的共用部分，切換路由時不會刷新，可放如 header, footer

2. `template.tsx` :該路由下的非共用部分，切換路由時會刷新，可放如不同商品資訊

3. `loading.tsx` : 該路由的主頁面還在非同步渲染時，會顯示此檔案內容

4. `error.tsx` : 該頁面的路由渲染出錯時會顯示的內容

---

## 三、更無痛的寫 React Server Components

幾個重點如程式碼

1. app 資料夾中會默認採用 React Server Components

2. page 資料夾中也可更簡單採用 React Server Components

3. React Server Components 中的 fetch 支援不同情境的功能

4. app 資料夾中的檔案加上 ‘’use client，使用只在 client-side 執行的組件

> **1. Next.js 13 的 app 資料夾中會默認採用 React Server Components**

```javascript
// Before Next.js 13 
// page/categorypage/category.js
export async function getServerSideProps {
  const getData = await fetch('...api...');
  return {
    props: {
      data: getData
    },
  };
};

export default function CategoryPage(data) {
  return (
    <>
      <div>Category: {data.category}</div>
    </>
  )
}


// Next.js 13  
// app/categorypage/category.js
import { use } from 'react';

async function getData() {
  const res = await await fetch('...api...');
  return res;
}

export default function CategoryPage(category) {
  const data = use(getData());
  return (
    <>
      <div>Category: {data.category}</div>
    </>
  )
}
```

> **2. Next.js 13 的 page 資料夾中也可更簡單採用 React Server Components**

```javascript
// 需要在外層包 suspend
// Next.js 13  
// app/categorypage/page.js
import { Suspense } from 'react';
import CategoryPage from './app/category/page.js';

export default function Posts() {
  return (
    <>
      <Suspense>
        <CategoryPage />
      </Suspense>
    </>
  );
}

// app/categorypage/category.js
import { use } from 'react';

async function getData() {
  const res = await await fetch('...api...');
  return res?.category;
}

export default function CategoryPage(category) {
  const data = use(getData());
  return (
    <>
      <div>Category: {data.category}</div>
    </>
  )
}
```

> **3. React Server Components 中的 fetch 支援不同情境的功能**

```
// 這個請求應該被緩存，直到手動失效，類似於 'getStaticProps'
// 'force-cache' 是默認值，可以省略
fetch(URL, { cache: 'force-cache' });

// 這個請求應該在每個請求上重新獲取，類似於 'getServerSideProps'
fetch(URL, { cache: 'no-store' });

// 這個請求應該被緩存 10 秒的生命週期，類似於帶有 'revalidate' 選項的 'getStaticProps'
fetch(URL, { next: { revalidate: 10 } });
```

> **4. Next.js 13 的 app 資料夾中的檔案加上 ‘’use client，使用只在 client-side 執行的組件**

```javascript
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

---

> **參考資料**
>
> 1. [Next.js 13… this changes everything](https://www.youtube.com/watch?v\=\_w0Ikk4JY7U)
>
> 2. [Next.js 13 - The Basics](https://www.youtube.com/watch?v\=\__mSgDEOyv8)
>
> 3. [Next.js 13 带来比 Webpack 快 700 倍的打包器：Turbopack](https://www.oschina.net/news/214982/nextjs-13-released)
>
> 4. [你好，Next.js 13](https://juejin.cn/post/7160084572942630926)