---
title: '用 Hugo 建立一個靜態網頁搭建在 Github Pages'
date: 2022-05-22T14:54:21+08:00
draft: true
---

## 在 Hugo 資料夾模板下寫好網頁內容

1. 打開 VS Code 切到桌面
2. brew install hugo
3. hugo new site mysite
4. cd mysite
5. git init
6. git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
7. echo theme = \"PaperMod\" >> config.toml
8. hugo new posts/my-first-post.md
9. hugo server -D
10. 打開本地端網址：[http://localhost:1313/](http://localhost:1313/)

## 用 Nelify 部署在 Github Pages

1. 在 [github](https://github.com/) 開一個新 repo 取得 URL
2. 回到 VS Code
3. git remote add origin URL
4. git push -u origin master
5. 打開 [Nelify](https://www.netlify.com/) 註冊會協助你連上 github 建立的 repo
6. Nelify 幫你把網站部署完成（到 Github Pages）：[https://boison.netlify.app/](https://boison.netlify.app/)
