---
title: 用 Hugo 寫 Blog 部署在 Netlify
date: 2022-05-22T14:54:21+08:00
author: Boison
slug: Blog-Hugo-deploy-Netlify
tags:
  - Hugo
  - Github
  - Netlify
draft: false
keywords:
  - Hugo
  - Github
  - Netlify
---

這篇文章說明如何在 5 分鐘架設一個靜態網站。首先你可以利用 Hugo 這套工具產生你的網站架構，接著利用 Netlify 將網頁部署在 Netlify。

本文假設你是在 Mac 的環境下操作，若你的環境是 Windows 則雖概念相同但步驟會略有差異；若是過去完全沒有寫程式的經驗，你會需要根據注意事項花額外的時間準備好一些基礎建設並適度了解其原理。

本文著重在實作面，至於原理請見 [Netlify 靜態網站托管的功能](https://boison.tw/2022/07/netlify-101/) 一文。

※ 當然你也可以不要花太多時間搞懂這些鬼東西，只要簡單依照現有的安裝教學指引讓網站成功運行，若是過程中卡住，很歡迎你聯繫我，我很願意提供幫助。

> **名詞解釋**:

- [Hugo](https://gohugo.io/): 靜態網站生成器
- [Homebrew](https://brew.sh/index_zh-tw): 套件管理工具
- [Github](https://github.com/): 程式碼代管平台
- [Netlify](https://www.netlify.com/): 靜態網站託管平台
- [Visual Studio Code (VS Code)](https://code.visualstudio.com/): 原始碼編輯器
- [Terminal](https://zh.wikipedia.org/zh-tw/%E7%B5%82%E7%AB%AF): 終端機

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

> **注意事項**:

- 你可以選擇不使用 VS Code，只要用本機的 Terminal 介面即可
- 此處模板選擇 PaperMod 如果想選擇其他主題至此 [Hugo Themes](https://themes.gohugo.io/) 並替換掉步驟 6 和 7
- 若是沒有 Github 帳號需要先去註冊，repo 為專案意思 (開 repo 可以想成是在 google drive 開新資料夾)
- 此處安裝環境為 mac，預設已經安裝好 brew (若未安裝 brew 可見 [如何在 Mac 上安裝套件管理工具 Homebrew](https://redox-ccy.medium.com/%E7%AD%86%E8%A8%98-%E5%A6%82%E4%BD%95%E5%9C%A8mac%E4%B8%8A%E5%AE%89%E8%A3%9Dhomebrew-87f127c6ebcf) 一文安裝)
- 「本地端」的意思是網站只在你電腦上可訪問，localhost:1313 就是打開你電腦中資料夾的位置

---

## 二、用 Netlify 將網頁部署在 Github Pages

1. 在 [github](https://github.com/) 開一個新 repo 取得 URL
2. 回到 VS Code
3. git remote add origin URL
4. git push -u origin master
5. 打開 [Netlify](https://www.netlify.com/) 註冊會協助你連上 github 建立的 repo
6. Netlify 幫你把網站部署完成：[https://boison.netlify.app/](https://boison.netlify.app/) 確認遠端是否可以開啟

> **注意事項**:

- 在 Github 新建 repo 後點擊進入右上角點 Code/Https 可以取得 URL
- 在 Netlify 設定中輸入此 URL，往後每次你更新 repo 內容時會自動讀取內容並重新部署
- 程式碼一開始先放在 Github 的空間，Netlify 是協助將要發佈的程式碼網站部屬到 Netlify 可更方便的調整設定
- 「部屬」的意思是將網站內容放入遠端空間，並且註冊取得一組網址讓所有人都能訪問此空間存放之內容
