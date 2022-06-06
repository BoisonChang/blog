---
title: 用 Netlify CMS 新增網頁後台介面
date: 2022-06-06T12:48:35.448Z
author: Boison
slug: Netlify-CMS-Hugo
tags:
  - Netlify
  - CMS
  - Hugo
---
> 本篇文章說明如何在兩分鐘內為你的靜態網站新增後台網頁

說實在筆電上用 VS Code 當編輯器來寫文章眼睛還是有點痛跟麻煩，前陣子印象剛好看到一篇可以在網頁新增後台編輯文章介面就來實作看看，用好之後就能在 https://boison.tw/admin 快速編輯文章了！這篇文章是第一篇不是用 VS Code 當編輯器而是用網頁後台寫的，感覺比較方便也不用輸入 Command Line 指令。

※ 註：

* 本文預設你使用[Hugo 並將網站用 Netlify 部署在 Github 上面](https://boison.tw/2022/05/hugo-blog-netlify-deploy-github/)，如連結文章教學
* 步驟可能會因時間跟環境有些微差異可參考 [Netlify CMS 官方文件](https://www.netlifycms.org/docs/configuration-options/#collections)為準，但核心原則不會差太多

- - -

## 一、先在根目錄的 static 資料夾新增資料夾裡面放兩個檔案

> 以下用終端機指令比較簡潔

1. cd 專案根目錄路徑
2. mkdir static/admin
3. touch static/admin/index.html // 制式內容如下 
4. touch static/admin/config.yml // 制式內容如下微調
5. git add .
6. git commit -m "create admin"
7. git push

### index.html

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Boison</title>
    <!-- Include the script that enables Netlify Identity on this page. -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
  </head>
  <body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
  </body>
</html>
```

### config.yml

```
backend:
  name: git-gateway
  repo: "blog/cms" # blog 存放在 Github repo 的位置
  branch: "master" # 分支
  site_url: "https://boison.tw" # 你的網站網址

media_folder: "static/img" # 存放多媒體的資料夾
public_folder: "/img"
locale: "zh_Hant" # 語系 https://github.com/netlify/netlify-cms/tree/master/packages/netlify-cms-locales/src

collections: # https://www.netlifycms.org/docs/configuration-options/#collections
  - name: 'posts'
    label: '文章'
    folder: 'content/posts'
    create: true
    slug: '{{year}}-{{month}}-{{day}}-{{slug}}'
    editor:
      preview: false
    fields:
      - { label: '標題', name: 'title', widget: 'string' }      
      - { label: '發布日期', name: 'date', widget: 'datetime' }
      - { label: '內容', name: 'body', widget: 'markdown' }
      - { label: '作者', name: 'author', widget: 'string' }
      - { label: '短網址', name: 'slug', widget: 'string' }
      - { label: "標籤", name: "tags", widget: "list", required: false}
      - { label: "分類", name: "categories", widget: "list", required: false}
  - name: "categories"
    label: "分類"
    folder: "content/categories"
    create: true
    fields:
      - {label: "標題", name: "title", widget: "string"}
      - {label: "内容", name: "body", widget: "markdown", required: false}
  - name: "tags"
    label: "標籤"
    folder: "content/tags"
    create: true
    fields:
      - {label: "標題", name: "title", widget: "string"}
      - {label: "内容", name: "body", widget: "markdown", required: false}
```

※ 教學文章說把 admin 資料夾放在根目錄我實作無法，照另外一篇說放在 static 資料夾我就可以不知為何

- - -

## 二、到 Netlify 控制台打開後台設定

1. 在 [Netlify 後台](https://app.netlify.com/)進入你的 site 點擊 Site settings
2. 左側選單找到 Identity，將之開啟 Enable Identity
3. Identity 下找到 Registration 點擊 Edit settings -> Invite only
4. Identity 下找到  Services 點擊 Enable Git Gateway -> 輸入 Github 帳號密碼
5. Identity 下找到 Registration 找到 External providers 點擊 add provider 開啟第三方登入
6. 站點上方找到 Identity 並且點擊 Invite users 輸入你信箱並去信箱收信驗證
7. 開啟 boison.tw/admin 登入網頁後台如圖

{{< img src="/static/img/screen-shot-2022-06-06-at-9.48.01-pm.png" >}}

> 圖片來源 & 參考資料：[十分鐘將 Netlify CMS 添加到現有的 Hugo 網站](https://namepluto.com/%E5%8D%81%E5%88%86%E9%90%98%E5%B0%87-netlify-cms-%E6%B7%BB%E5%8A%A0%E5%88%B0%E7%8F%BE%E6%9C%89%E7%9A%84-hugo-%E7%B6%B2%E7%AB%99/)