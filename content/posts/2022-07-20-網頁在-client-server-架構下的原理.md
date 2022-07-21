---
title: 網頁在 Client-Server 架構下的設計
date: 2022-07-20T09:27:15.206Z
author: Boison
slug: Client-Server
tags:
  - Server
  - Database
draft: false
---
一個常見網站主要會由三種 Server 交叉運作組成

1. **Web Server**

   * 主要工作根據瀏覽器 Client 發送的請求，回傳靜態的網頁檔案（HTML pages、檔案、圖片、影片）
   * 隨時監聽有沒有任何瀏覽器 Client 對自己發送 request，有的話該回應什麼樣的 response
   * 若在電腦(指主機)上執行 apache，它會在電腦上開一個連接埠為 80，那別人 (指 Client ) 就可以透過 IP 來連接到你的電腦
   * **常見的 Web Server**
     * 有 Apache、Ngnix

2. **Application Server**


   * 但是當使用者需要個人化結果時，又必須使用 Application Server 中的業務/商業邏輯，此時由 Web Server 跟 Application Server 發送請求
   * **Web Server 和 Application Server 溝通的協定**
     * 是採用 HTTP 協定，使用者 Client 理論上只能透過 Web Server 與 Application Server 做溝通
   * **Application Server 和 Database Server 溝通的協定**
     * 例如 Common Gateway Interface、Java 的 Servlet，Python 的 WSGI 和 Ruby 的 Rack 2 等
   * **常見的 Application Server**
     * 透過 Node.js、Golang 這類程式語言起的 Web Server 通常會被稱為 Application Server，如 Express

3. **Database Server**

   * Application Server 收到  Web Server 的請求在程式內處理後 (業務/商業邏輯) 向  Database Server 發送需求

※ 註：

* Web Server、Application Server、Database Server  可能被放在同一台電腦上或者不同台，在概念上我們也把這台電腦叫做 Server

* 前面提到的 Web Server、Application Server、Database Server 是指具備不同功能的程式，然而它們在不同的前後文 Context 中卻都被以 Server 指稱

![This image shows how users makes requests through a web and application server to get dynamic web content from an application database. ](https://www.serverwatch.com/wp-content/uploads/2021/05/Web_App_Flow-02-01-1024x385.png)\
圖片來源: [What Is an Application Server?](https://www.serverwatch.com/guides/application-server/)

![https://ithelp.ithome.com.tw/upload/images/20181008/20110371PHHqFwmcSE.png](https://ithelp.ithome.com.tw/upload/images/20181008/20110371PHHqFwmcSE.png)

圖片來源: [Day 8 - 一周目- 開始玩轉後端(一)](https://ithelp.ithome.com.tw/articles/10200476)

---

> **參考資料**
>
> 1. [淺談 Web Server and Application Server](https://vicxu.medium.com/web-server-and-application-server-5a6d9c940eff)
> 2. [用 Node.js 架設 http server](https://jimmyswebnote.com/node-js-http-server/)
> 3. [PostgreSQL 教學：架設 database server](https://jimmyswebnote.com/postgresql-tutorial/)
> 4. [server 是什麼？](https://jimmyswebnote.com/what-is-server/)
> 5. [CGI是什么](https://www.jianshu.com/p/c4dc22699a42?u_atoken\=ed6517e9-19ff-40e5-acbd-32dc0ae15b8b&u_asession\=01NcIOqo_1ejtvEBGMleqArQ\_-aSQqFQFoS6AO10UhIwQK9jroFTB5Ojs_cHgd4K4BX0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K9pV6uAMpqup84e_G4ZUeCtPpcarp92QKzyJKyYjREPlmBkFo3NEHBv0PZUm6pbxQU&u_asig\=058un4ynxXv3LVdcU4rcDeWH6G_I0D2JPUo0HH77NXKTa6gTssotnQpdoA9pz3Xi1WnOBpDfjxsEFKBNYx-43TioKFa4blEi7OgWtrK1mm-LZKuEblCJINow6XmK4hqIMnANmjcUxa-bqEbmqyTwgdRzzImxLKzz84rQTehc0XYW_9JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzWHaXwcfAvCJFXBgPzhwb42JajRD9vaB0FW3IY8IN--9WPRPQyB_SKrj-61LB_f61u3h9VXwMyh6PgyDIVSG1W-xljlbH1RboHv1XQ9CWmlXcLvoHMjeznVGgQJUfzM8WRaJD0fGM7bCcmJJqgDtEcDvDtENewUy5OGlVipuZnt8mWspDxyAEEo4kbsryBKb9Q&u_aref\=LQ3n27CBFoayjO%2Bl3FH2JMkVfPw%3D)
> 6. [CGI与Servlet的区别和联系](https://blog.csdn.net/DLUTBruceZhang/article/details/49933357)