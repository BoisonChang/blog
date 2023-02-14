---
title: ReactDOMServer 搭配 Express 和 Esbuild 實作 Server Side Rendering (SSR)
date: 2023-02-11T04:42:51.064Z
author: Boison
slug: ReactDOMServer-SSR-101
tags:
  - React
  - Express
  - SSR
  - ""
draft: true
---

## 1. Server Side Rendering (SSR) 簡介

1. 簡要說明 SSR 和它與客戶端渲染 (CSR) 的比較

   * Server Side Rendering (SSR) 是一種 Web 應用程序技術，它通過在服務器端渲染網頁內容來提高頁面載入速度和搜索引擎友好性。當用戶在瀏覽器中請求網頁時，服務器將網頁的內容渲染為 HTML，並將其傳回給瀏覽器顯示。

   * 相比之下，Client Side Rendering (CSR) 則是在瀏覽器端通過 JavaScript 渲染網頁內容。當用戶請求網頁時，瀏覽器先加載 HTML 骨架，然後再使用 JavaScript 渲染頁面內容。

2. 說明為什麼某些情況下需要 SSR

   * 提高頁面載入速度：因為 SSR 在服務器端渲染網頁內容，所以網頁的初始載入速度可以大大提高。

   * 改善搜索引擎優化 (SEO)：因為搜索引擎不能很好地解析 JavaScript 渲染的頁面，所以使用 SSR 可以改善網站的 SEO 效果。

   * 改善用戶體驗：因為 SSR 可以在網頁載入時提供更多內容，所以用戶可以更快地查看網頁內容。

   * 改善頁面結構：因為 SSR 可以確保頁面的 HTML 結構正確，並且符合標準。

---

## 2. 設定環境

首先，我們需要在 Express 中創建一個路由，以渲染我們的 React 組件，接下來，我們需要將我們的 React 組件通過 **`ReactDOMServer.renderToString()`** 方法渲染為字符串，最後，我們需要將渲染後的字符串作為 HTML 頁面的內容傳回給瀏覽器。

在這樣的渲染過程中，Express 路由的主要責任是創建一個 HTML 文件並傳回給瀏覽器。該 HTML 文件包含了 React 組件的靜態 HTML 以及需要的 JavaScript 文件。通過在服務器端渲染 React 組件，我們就能夠在頁面加載時就把所有必要的信息渲染出來，使得瀏覽器不需要再進一步發送請求來加載頁面數據。

* 安裝並設定 Express 服務器

  ```javascript
  npm install express
  ```

* 在項目的根目錄中創建一個名為 **`main.js`** 的文件

  ```javascript
  import express from "express";
  import ReactDOMServer from "react-dom/server";
  import path from "path";
  import { App } from "./client";
  
  express()
    // serve client side code
    .use(express.static(path.resolve(__dirname, "dist")))
  
    // serve index.html
    .get("/", (_, res) => {
      const html = ReactDOMServer.renderToString(
        <html>
          <head>
            <title>SSR Example</title>
          </head>
          <body>
            <div id="root">
              <App />
            </div>
            <script src="bundle.js"></script>
          </body>
        </html>
      );
      res.send(html);
    })
    .listen(3000, () => {
      console.log("Server is running on http://localhost:3000");
    });
  ```

* 安裝並設定 ReactDOMServer 以及必要的依賴包

  ```javascript
  npm install react react-dom
  ```

  ```javascript
  const express = require('express');
  const React = require('react');
  const ReactDOMServer = require('react-dom/server');
  
  const app = express();
  
  app.get('/', (req, res) => {
    // 在此處渲染 React 組件
    const html = ReactDOMServer.renderToString(
      React.createElement('h1', null, 'Hello, World!')
    );
    res.send(html);
  });
  
  app.listen(3000, () => {
    console.log('Express server is running on port 3000');
  });
  ```

* 安裝並設定 Esbuild 以加速 JavaScript 文件的編譯

  ```javascript
  npm install esbuild --save-dev
  ```

  在 package.json 中設定用 esbuild 先打包

  ```javascript
  {
    "name": "ssr-express-tsx",
    "version": "1.0.0",
    "main": "index.js",
    "license": "MIT",
    "scripts": {
      "start": "esbuild client.tsx --bundle --outfile=dist/bundle.js --allow-overwrite && tsx main.tsx"
    },
    "dependencies": {
      "express": "^4.18.2",
      "react": "^18.2.0",
      "react-dom": "^18.2.0"
    },
    "devDependencies": {
      "@types/express": "^4.17.16",
      "@types/react": "^18.0.27",
      "@types/react-dom": "^18.0.10",
      "esbuild": "^0.17.5",
      "tsx": "^3.12.2"
    }
  }
  ```

---

## 3. 實作 SSR

* 創建一個 Express 路由以渲染 React 組件

* 在路由中使用 ReactDOMServer.renderToString() 方法將組件渲染為字符串

* 將渲染後的字符串作為 HTML 頁面的內容傳回給瀏覽器

---

## 4. 加入動態數據

前面我們已經介紹了如何使用 Express 和 ReactDOMServer 實現 Server Side Rendering，但還有一個重要的步驟就是如何使用數據。您可以使用任意的數據來源，例如 API，收集數據。然後，在路由中將數據傳遞給 React 組件以呈現在頁面上。

* 使用任意的數據來源（如 API）收集數據

  ```javascript
  const app = express();
  
  app.get("/api/data", (req, res) => {
    res.json({ message: "Hello from the server!" });
  });
  ```

* 在路由中將數據傳遞給 React 組件以呈現在頁面上

  ```javascript
  import React, { useState, useEffect } from 'react';
  
  const [data, setData] = useState({});
  
  useEffect(() => {
    fetch("/api/data")
      .then(res => res.json())
      .then(response => setData(response));
  }, []);
  
  return <div>{data.message}</div>;
  ```

* 最後，我們可以在路由中將數據傳遞給組件。

  ```javascript
  const app = express();
  import ReactDOMServer from 'react-dom/server';
  import data from "..."
  
  app.use(express.static(path.resolve(__dirname, "dist")))
     .get("/", (_, res) => {
        const html = ReactDOMServer.renderToString(
          <App data={data} />
        );
      
        res.send(`
          <html>
            <body>
              <div id="root">${html}</div>
              <script src="bundle.js"></script>
            </body>
          </html>
        `);
     });
  ```

---

> **實作專案**：[Github Repo](https://github.com/BoisonChang/ssr-express-webpack)