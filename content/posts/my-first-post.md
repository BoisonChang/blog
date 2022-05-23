---
title: '用 Hugo 寫 Blog 部署在 Github Pages'
date: 2022-05-22T14:54:21+08:00
draft: false
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

**注意事項**:
- 此處模板選擇 PaperMod 如果想選擇其他主題至此 [Hugo Themes](https://themes.gohugo.io/) 並替換掉步驟 6 和 7
- 若是沒有 Github 帳號需要先去註冊，repo 為專案意思 (可以想成是再 google drive 開新資料夾)
- 此處安裝環境為 mac，預設已經安裝好 brew (若未安裝 brew 可見 [如何在 Mac 上安裝套件管理工具 Homebrew](https://redox-ccy.medium.com/%E7%AD%86%E8%A8%98-%E5%A6%82%E4%BD%95%E5%9C%A8mac%E4%B8%8A%E5%AE%89%E8%A3%9Dhomebrew-87f127c6ebcf) 一文安裝)


## 用 Nelify 部署在 Github Pages

1. 在 [github](https://github.com/) 開一個新 repo 取得 URL
2. 回到 VS Code
3. git remote add origin URL
4. git push -u origin master
5. 打開 [Nelify](https://www.netlify.com/) 註冊會協助你連上 github 建立的 repo
6. Nelify 幫你把網站部署完成（到 Github Pages）：[https://boison.netlify.app/](https://boison.netlify.app/)
