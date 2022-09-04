---
title: 快速在 mac 安裝 mongoDB
date: 2022-09-04T06:43:40.765Z
author: Boison
slug: mongodb-setup-mac
tags:
  - MangoDB
  - ""
draft: false
---
主要分為幾個步驟

1. 下載 mongodb 並且移動到電腦中比較適合存放的資料夾
2. 創建存放數據的資料夾
3. 創建存放日誌的資料夾
4. 創建快速開啟的資料夾及文件
5. 到可以新增全域變數的地方新增全域變數

- - -

* 去 [mongodb 官網](https://link.juejin.cn/?target=https%3A%2F%2Fwww.mongodb.com%2Ftry%2Fdownload "https\://www.mongodb.com/try/download") 下载對應版本的 **`mongodb`**

![](https://media.heptabase.com/v1/images/fc9c5ccc-1901-4cb2-bb69-b6fcd717ac4f/7d69f250-09bb-467e-9366-a18a01cfd0a2/Screen%20Shot%202022-09-04%20at%202.27.47%20PM.png)

* 下載後解壓縮更名 mongodb 丟入 /usr/local
* 開啟終端機
* cd /usr/local/mongodb
* mkdir data 

  * 用來存放數據，到時候可以通過配置文件讓 mongodb 把數據存儲到該目錄下
* mkdir logs 

  * 創建一個 mongodb.log 文件用來存放數據，到時候可以通過配置文件讓 mongodb 把日誌寫到該目錄
* mkdir etc 

  * cd etc
  * touch mongod.conf
  * vim mongod.conf
  * 點擊 i 開啟編輯模式貼上以下
  * \# 數據庫文件存放的目錄 

    dbpath=/usr/local/mongodb/data \
    \# 數據庫日誌存放目錄 \
    logpath=/usr/local/mongodb/logs/mongodb.log \
    \# 以追加的方式記錄日誌 logappend = true \
    \# 以後台方式運行進程 \
    \#fork=true
  * 點擊 esc 離開編輯模式，輸入 :qw 儲存並離開文件
* cd ~
* open ~/.zshrc

  * 貼上在最下面並儲存離開，用全域變數讓以後不用進到 bin 文件即可呼叫 mongod 指令
  * \# 快速用 mongod 指令使用 mongodb 相關指令

    export PATH=/usr/local/mongodb/bin:$PATH
* cd /usr/local/mongodb/bin
* xattr -dr com.apple.quarantine .

  * 因為它是一個外部下載的文件，需要刪除隔離屬性
* mongod \-\-config /usr/local/mongodb/etc/mongod.conf

- - -

> **參考資料**
>
> 1. [mac安装mongodb(一条龙服务)](https://juejin.cn/post/7068132825773375501)
> 2. [Mac OS 安装 MongoDB 详细步骤](https://juejin.cn/post/7052585815037673479)