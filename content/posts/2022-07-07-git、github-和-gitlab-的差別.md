---
title: Git、GitHub 和 GitLab 的差別
date: 2022-07-07T01:50:54.246Z
author: Boison
slug: git-gitlab-github
tags:
  - Git
  - GiHub
  - GitLab
draft: false
---
> **Git 是一個開源的分佈式版本控制系統，可以有效高速地處理從很小到非常大的項目版本管理**

Git 是 Linus Torvalds 為了幫助管理 Linux 內核開發而開發的一個開放源碼的版本控制軟件。而 GitHub 和 GiLlab 兩個都是基於 Web 的 Git 遠程倉庫，它們都提供了分享開源項目的平台，為開發團隊提供了存儲、分享、發布和合作開發項目的中心化雲存儲的場所。

詳細原理跟使用方法請參考 [看完《為你自己學 Git》三大收穫](https://boison.tw/2022/06/bookreview-git-github/) 一文

---

## 1. Git 

* 是一種版本控制系統，是一種工具，用於代碼的存儲和版本控制

## 2. GitHub 

* 是一個基於 Git 實現的在線代碼倉庫，是目前全球最大的代碼託管平台，可以幫助程序員之間互相交流和學習

* 對於開源項目而言，GitHub 依然是代碼託管的首選

* GitHub 讓您可以使用自己選擇的 CI/CD 工具，但需要自行集成

  *  GitHub 用戶通常使用第三方 CI 程序，比如 Jenkins、CircleCI 或 TravisCI

* 單個文件不能超過 100MB，而免費版本的存儲庫限制為 1GB

## 3. GitLab 

* 是一個基於 Git 實現的在線代碼倉庫軟件，你可以用 GitLab 自己搭建一個類似於 GitHub 一樣的倉庫

* 有完善的管理界面和權限控制，一般用於在企業、學校等內部網絡搭建 Git 私服

  * 從代碼的私有性上來看 GitLab 是一個更好的選擇，GitLab 提供開源代碼，支持企業自己用企業買的服務器搭建企業 GitLab 只供公司所有人使用

* GitLab 內置了持續集成/持續交付 (CI/CD) 和 DevOps 工作流程

---

> 參考資料
>
> 1. [Git、GitHub和GitLab的简单区别](https://juejin.cn/post/6905582387325829128)
>
> 2. [和艦長一起 30 天玩轉 GitLab](https://ithelp.ithome.com.tw/users/20120986/ironman/2733)
>
> 3. [GitHub与GitLab有何不同，您该选那个?](https://blog.desdelinux.net/zh-TW/github-vs-gitlab/?fbclid\=IwAR1ltZlI8ItAuWW7SDFUoeRWBBm4fhgRsyLp6p4DS4O38ESlCA4eFRglRF8)
>
> 4. [GitHub vs GitLab：這些平台的優缺點](https://blog.desdelinux.net/zh-TW/github-vs-gitlab/?fbclid\=IwAR1ltZlI8ItAuWW7SDFUoeRWBBm4fhgRsyLp6p4DS4O38ESlCA4eFRglRF8)
>
> 5. [GitHub、GitLab 與BitBucket，程式設計師應該怎麼選？](https://kknews.cc/zh-tw/code/33kk9p8.html)