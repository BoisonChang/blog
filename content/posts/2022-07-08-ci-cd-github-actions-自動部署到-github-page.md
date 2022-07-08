---
title: "CI/CD: GitHub Actions 自動部署到 GitHub Page"
date: 2022-07-08T02:50:09.540Z
author: Boison
slug: CICD-GitHub-Actions-ghpages
tags:
  - CI/CD
  - GitHubActions
  - GitHub
draft: false
---
GitHub Actions 是透過設定一系列的腳本來運行 CI/CD 的流程，因此我們需要將腳本的內容寫在一個 workflow.yml 的檔案中。(名稱自訂不一定要是 workflow)

此處不多解釋 CI/CD 原理，會把重點放在如何在 GitHub 上用 Actions 撰寫 CICD 的腳本 Workflows，不過練習為了簡化邏輯，並沒有撰寫測試和程式碼檢測等內容，因此 Workflows 其實只撰寫持續交付(Continuous Deployment, CD) 的自動部署的部分。

※ CI/CD 原理請見 [CI/CD 原理: 持續集成&持續部署/交付](./CI%2FCD%20%E5%9F%BA%E7%A4%8E%E5%8E%9F%E7%90%86%20154d8df4-29f0-4fb5-8dff-d65b45f93c60.md "CI/CD 原理: 持續集成&持續部署/交付")

---

## 1. 建置專案

* `npx create-react-app my-app-cicd`

* `cd my-app-cicd`

* `npm run start`

* `git init`

* `git add .`

* `git commit -m “first commit“`

* 在 GitHub 上開設 repo 取名 my-app-cicd

* `git remote add origin <https://github.com/BoisonChang/my-app-cicd.git>`

* `git push -u origin main`

---

## 2. 手動建立部署一次 GitHub Pages

* `npm install --save gh-page`

* 設定 package.json 檔案內容

  * 加上 name

  * 加上 homepage

  * scripts 中加上 "predeploy": "npm run build"

  * scripts 中加上 "deploy": "gh-pages -d build"

```javascript
{
  "name": "my-app-cicd",
  "homepage": "https://myusername.github.io/my-app-cicd",
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

## 3. 設定

* 先到 Setting 產生一組可以訪問 repo 的 token

  * [++https://github.com/settings/tokens++](https://github.com/settings/tokens) 

  * 勾選 repo 所有

  * 產生一組 token 先把 token 複製再關掉頁面

* 到 repo 中的 Settings 選擇 Secrets

  * 點擊 new secret 

  * 加入我們剛剛得到的 access token 在 value

  * 其他隨意命名後 Add secret

* 到 repo 的 Actions 中建立腳本 Workflows

  * 點擊 New workflow

  * 點擊 set up a workflow yourself

  * 貼上下面寫好的腳本內容後點擊 Start commit 

    * 注意: 此腳本內容有時效性，可能因版本更新而需要自行修改內容

```javascript

name: Github Pages Deployment

on:
  push:
    branches:
      - "main"

jobs:
  deploy:
    name: deploy GH pages
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup node
        uses: actions/setup-node@v3
        with:
          node-version: 16.x
      - name: Install and build
        run: npm install && npm run build
      - name: Deploy
        uses: JamesIves/github-pages-deploy-action@4.1.1
        with:
          branch: gh-pages
          folder: build
```

---

## 4. main 分支內容改變自動部署到  gh-page 分支

* 修改 app.js 內容

* `git commit -am “edit content“`

* `git push -u origin main`

* 此時到 repo 的 Actions 頁面會自動執行腳本開始部署成功後[頁面更新](https://boisonchang.github.io/my-app-cicd/)

---

## 5. Workflow 設定檔解釋

1. **name**

   * 這個 Workflow 腳本的名稱

2. **on**

   * 執行這個腳本的「執行條件」

   * 推送 main 這個分支啟動這個 Workflow 運作腳本

3. **jobs**

   * 不同的工作流都會寫在 jobs ，將編譯、測試、部署等等的工作各自寫成不同的工作流

   * 此處只撰寫了 deploy 部署的工作流

   * **runs-on**

     * 設置腳本運行時的 Docker base-image

   * **steps**

     * 實際動作命令名稱為 Checkout

     * 使用 actions/checkout@v2 來設置 GitHub Actions 的相關環境

     * 實際動作命令名稱為 Setup node

     * 使用 actions/setup-node@v3 來設置使用的 Node.js 版本

     * 實際動作命令名稱為 Install and build

     * 執行 npm install && npm run build

     * 實際動作命令名稱為 Deploy

     * 使用 JamesIves/github-pages-deploy-action@4.1.1 部署

---

> **參考資料**
>
> 1. [作为前端，要学会用Github Action给自己的项目加上CICD](https://juejin.cn/post/7113562222852309023)
>
> 2. [前端工程師在GitHub上持續整合與部署(CI/CD)](https://yusong.io/blog/frontend-ci-cd-on-github/)
>
> 3. [搭配Github actions建立一套 CI/CD系統](https://github.com/yichunsung/github_actions_studying)
>
> 4. [5 分钟教你快速掌握 GitHub Actions 自动发布 Npm 包和网站](https://segmentfault.com/a/1190000041753907)
>
> 5. [\[淺談\]用GitHub Actions 簡易的 CI 來發布GitPages](https://medium.com/@JainKai/%E6%B7%BA%E8%AB%87-%E7%94%A8github-actions-%E7%B0%A1%E6%98%93%E7%9A%84-ci-%E4%BE%86%E7%99%BC%E5%B8%83gitpages-6ae48e52467e)
>
> 6. [GitHub Actions 的 Workflow 設定檔解析](https://ithelp.ithome.com.tw/articles/10246589)