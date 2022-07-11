---
title: GitHub Pages 的原理與功能
date: 2022-07-11T03:43:14.092Z
author: Boison
slug: gh-pages-101
tags:
  - GitHubPages
  - GitHub
draft: false
---
> **GitHub Pages 是在 GitHub 裡面，可以解析靜態網頁/文件並渲染成頁面的網頁托管服務**

在 GitHub 的專案內新建一個分支名稱為 gh-pages 時，GitHub 便會預設將此分支內的檔案以靜態網頁的方式呈現，因此可以利用此類特性來製造一個簡單的網站。

所以最簡單的 GitHub Pages 應該是這樣，新建一個項目裡面放 index.html 檔案，在 setting 左邊 Code and automation 找到 pages，設定 GitHub Pages 的分支後即產生一個網址給你，若是分支內的檔案符合 GitHub Pages 的限制即可看到渲染成功後的網頁。

---

## 1. GitHub Pages 的限制

> **使用 gh-pages 時會有以下的限制**

1. 僅能呈現靜態頁面的內容（即 HTML 網頁） 

  * 無法提供動態網頁

  * 例如利用 PHP、JSP 等等網頁程式所撰寫的檔案

2. 不支援如 .htaccess 之類的密碼驗證功能，即不能針對 gh-pages 的頁面設定密碼保護

3. 所有的 gh-pages 內的頁面都是公開的，表示所有的人都可存取到相關的頁面

4. gh-pages上的檔案庫大小限制為 1GB

5. 每個月的頻寬限制為 100GB

6. 每小時最多能提交 10 個版本的檔案

---

## 2. GitHub Pages 綁定個人網域

1. 在該專案的根目錄放一個檔名為 `CNAME` 的檔案，內容只要放你想要客制化的那個網址

2. 網域後台設定一組 CNAME 指到 [`https://BoisonChang.github.io/my-app-cicd`](https://BoisonChang.github.io/my-app-cicd)  原始網址

---

## 3. 使用套件快速部署到 GitHub Pages

* 建置專案

  1. `npx create-react-app my-app-cicd`

  2. `cd my-app-cicd`

  3. `npm run start`

  4. `git init`

  5. `git add .`

  6. `git commit -m “first commit“`

  7. 在 GitHub 上開設 repo 取名 my-app-cicd

  8. `git remote add origin <`[`https://github.com/BoisonChang/my-app-cicd.git`](https://github.com/BoisonChang/my-app-cicd.git)`>`

  9. `git push -u origin main`

* `npm install --save gh-page`

* 設定 package.json 檔案內容

  1. 加上 “name”: “OOXX“

  2. 加上 “homepage” : “<https://myusername.github.io/name>“

  3. scripts 中加上 "predeploy": "npm run build"

  4. scripts 中加上 "deploy": "gh-pages -d build"

```
{
  "name": "my-app-cicd",
  "homepage": "https://BoisonChang.github.io/my-app-cicd",
  // ...
}
...
  "scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build",
  "start": "react-scripts start",
  "build": "react-scripts build",
    ...
}
```

* `npm run deploy`

* 若是成功，可在 [homepage 設定之 URL](https://boisonchang.github.io/my-app-cicd/) 打開網頁

---

> **參考資料**
>
> 1. [善用 gh-pages分支功能　無料架設靜態網站](https://www.netadmin.com.tw/netadmin/zh-tw/technology/89C148A5BC09490785753668A11280B8)
>
> 2. [使用 GitHub 免費製作個人網站](https://gitbook.tw/chapters/github/using-github-pages)
>
> 3. [使用 GitHub Pages 打造簡易短網址系統](https://blog.poychang.net/how-to-use-github-pages-build-a-short-url-app/)
>
> 4. [使用 GitHub Pages 展示你的 Single Page Application](https://blog.timtnlee.me/post/development/spa-on-github-page)
>
> 5. [github pages与travis ci运作原理](https://www.cnblogs.com/zhangnan35/p/10830010.html)