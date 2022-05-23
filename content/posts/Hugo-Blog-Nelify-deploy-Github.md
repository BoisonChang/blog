---
title: "用 Hugo 寫 Blog 部署在 Github Pages"
slug: Hugo-Blog-Nelify-deploy-Github
date: 2022-05-22T14:54:21+08:00
draft: false
tags: ["Hugo", "Github", "Nelify"]
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
# author: ["Me", "You"] # multiple authors
#canonicalURL: "https://canonical.url/to/page"
# weight: 1
# aliases: ["/AirTag"]
#cover:
#    image: "<image path/url>" # image path/url
#    alt: "<alt text>" # alt text
#    caption: "<text>" # display caption under cover
#    relative: true # when using page bundles set this to true
#    hidden: true # only hide on current single page
#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: false # to append file path to Edit link
  
---

這篇文章說明如何在 5 分鐘架設一個靜態網站。首先你可以利用 Hugo 這套工具產生你的網站架構，接著利用 Nelify 將網頁部署在 Github Pages。

若是過去完全沒有寫程式的經驗，你會需要根據注意事項花額外的時間準備好一些基礎建設並適度了解其原理 (套件管理工具 Homebrew、程式碼代管平台 GitHub、Netlify 靜態網站託管平台)。

---
## 一、在 Hugo 資料夾模板下寫好網頁內容

1. 打開 VS Code 切到桌面
2. brew install hugo
3. hugo new site mysite
4. cd mysite
5. git init
6. git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
7. echo theme = \"PaperMod\" >> config.toml
8. hugo new posts/my-first-post.md
9. hugo server -D
10. 打開本地端網址：[http://localhost:1313/](http://localhost:1313/) 確認本地端是否可以開啟

**注意事項**:
- 此處模板選擇 PaperMod 如果想選擇其他主題至此 [Hugo Themes](https://themes.gohugo.io/) 並替換掉步驟 6 和 7
- 若是沒有 Github 帳號需要先去註冊，repo 為專案意思 (可以想成是再 google drive 開新資料夾)
- 此處安裝環境為 mac，預設已經安裝好 brew (若未安裝 brew 可見 [如何在 Mac 上安裝套件管理工具 Homebrew](https://redox-ccy.medium.com/%E7%AD%86%E8%A8%98-%E5%A6%82%E4%BD%95%E5%9C%A8mac%E4%B8%8A%E5%AE%89%E8%A3%9Dhomebrew-87f127c6ebcf) 一文安裝)
- 「本地端」的意思是網站只在你電腦上可訪問，localhost:1313 就是打開你電腦中資料夾的位置

---
##  二、用 Nelify 將網頁部署在 Github Pages

1. 在 [github](https://github.com/) 開一個新 repo 取得 URL
2. 回到 VS Code
3. git remote add origin URL
4. git push -u origin master
5. 打開 [Nelify](https://www.netlify.com/) 註冊會協助你連上 github 建立的 repo
6. Nelify 幫你把網站部署完成（到 Github Pages）：[https://boison.netlify.app/](https://boison.netlify.app/) 確認遠端是否可以開啟


**注意事項**:
- 在  Github 新建 repo 後點擊進入右上角點 Code/Https 可以取得 URL
- 在 Nelify 設定中輸入此 URL，往後每次你更新 repo 內容時 Nelify 會自動讀取內容並重新部署 
- 網站實際上是放在 Github 的空間，Nelify 是協助簡化將網站部屬到 Github 步驟跟調整設定
- 「部屬」的意思是將網站內容放入遠端空間，並且註冊取得一組網址讓所有人都能訪問此空間存放之內容