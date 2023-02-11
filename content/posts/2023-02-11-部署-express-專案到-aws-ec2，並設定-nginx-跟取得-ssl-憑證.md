---
title: 部署 Express 專案到 AWS EC2，並設定 Nginx 跟取得 SSL 憑證
date: 2023-02-11T03:12:09.322Z
author: Boison
slug: aws-ec2-nginx-ssl
tags:
  - AWS
  - Nginx
  - Express
draft: false
---
## 1. AWS 基礎知識

* Key pair type 中 RSA 和 ED25519 有什麼差別
collapsed:: true

  1. 金鑰大小：RSA 金鑰的大小可以從 1024 到 4096 位不等，而 ED25519 金鑰固定為 256 位。

  2. 安全性：RSA 基於質因數分解的數學概念，多年來已經得到廣泛的研究和測試。另一方面，ED25519 基於椭圆曲線密碼學，具有與 RSA 相似的安全性，但金鑰大小更小。

  3. 速度：在簽署和驗證數位簽名時，ED25519 比 RSA 更快。

  4. 簽名演算法：RSA 使用 PKCS#1 v1.5 或 PSS 簽名演算法，而 ED25519 使用 EdDSA 簽名演算法。

  5. 實現：RSA 在各種編程語言中更加廣泛地支持和實現，而 ED25519 的支持仍在增長，尽管它在流行的密碼學庫（如libsodium和OpenSSL）中是可用的。

* Private key file format 中 .pem 和 .ppk 有什麼差別
collapsed:: true

  1. 格式不同：.pem 格式是 OpenSSH 的預設格式，而 .ppk 是 PuTTY 的預設格式。

  2. 加密方式不同：.pem 使用 Base64 編碼和 PEM 加密方式，而 .ppk 使用 PuTTY 私鑰格式加密方式。

  3. 支持的密鑰類型不同：.pem 支持 RSA、DSA、ECDSA 和 ED25519 等多種密鑰類型，而 .ppk 僅支持 RSA 和 DSA。

  4. 安全性：.pem 格式支持較強的加密方式，因此相對安全。但是 .ppk 可以更方便地存儲和移動，但安全性不如 .pem。

  5. 使用的軟件不同：.pem 文件主要用於 OpenSSH，而 .ppk 文件主要用於 PuTTY 和其他 Windows 系統下的 SSH 客戶端軟件。因此，如果您正在使用 OpenSSH，則應使用 .pem 格式，如果您正在使用 PuTTY 或其他 Windows 客戶端軟件，則應使用 .ppk 格式。

* AWS 中的 security group 有什麼功能
collapsed:: true

  1. 防火牆功能：AWS 安全組可以防止不受信任的網絡流量進入您的 AWS 環境。它通過允許或拒絕網絡流量來完成此操作。

  2. 選擇性開放：您可以選擇開放特定的端口，以便通過公共網絡進入特定的 AWS 資源，例如網站或服務。

  3. 安全性：通過使用安全組，您可以加強您的 AWS 資源的安全性，防止不必要的網絡流量或攻擊。

  4. 可複利性：安全組可以分配給多個 AWS 資源，這樣您就不需要重新配置防火牆規則，以獲得更高的效率。

  5. 可管理性：安全組是一種可管理的防火牆，您可以隨時通過 AWS 管理控制台或 API 修改防火牆規則，以應對您的應用程序需求的變化。

* AWS 中的 security group 中 Inbound rules 和 Outbound rules 有什麼差別
collapsed:: true

  1. Inbound Rules：

     * Inbound Rules 指的是對於每一個 AWS 資源，允許進入該資源的網路流量規則。

  2. Outbound Rules：

     * Outbound Rules 指的是對於每一個 AWS 資源，允許從該資源發出的網路流量規則。

  3. 區別：

     * Inbound Rules 和 Outbound Rules 的主要區別在於流量的方向：Inbound Rules 控制進入資源的流量，而 Outbound Rules 控制從資源發出的流量。

  4. 功能：

     * Security group 中的 Inbound Rules 和 Outbound Rules 用於保護 AWS 資源不受未經授權的訪問，並控制網絡流量的進出。

  5. 配置：

     * 配置 Inbound Rules 和 Outbound Rules 時，必須指定源 IP 地址、目標 IP 地址、協定和端口範圍，以控制網絡流量的進出和進入。

* Amazon Machine Image (AMI)
collapsed:: true

  1. 定義：

     * Amazon Machine Image (AMI) 是一種可以用於在 Amazon Elastic Compute Cloud (Amazon EC2) 上啟動虛擬機器的映像。

  2. 內容：

     * AMI 包含了操作系統、應用程式、配置文件等所需的全部資源，可以快速啟動虛擬機器。

  3. 選擇：

     * AWS 提供了多種預先建立的 AMI，使用者可以根據自己的需求選擇適合的 AMI。

  4. 定制：

     * 如果找不到合適的 AMI，也可以從現有的 EC2 实例創建自己的 AMI。

  5. 使用：

     * AMI 可以用於快速在 EC2 上創建虛擬機器，也可以作為 EC2 的镜像來進行自動化部署和管理。

* AWS 的 Instance type
collapsed:: true

  1. AWS Instance Type 

     * 是 Amazon Elastic Compute Cloud (Amazon EC2) 中的一种计算实例类型，它指定了每一个虚拟机的配置，包括 CPU 核数、内存大小、存储空间和网络带宽。

  2. 特性：

     * AWS Instance Type 提供了不同的资源配置，使用者可以根据自己的应用需求选择合适的实例类型。

  3. 使用：

     * AWS Instance Type 可以通过 Amazon EC2 控制台或 API 启动，从而创建一个或多个计算实例。

  4. 计价：

     * AWS Instance Type 的价格与实例的配置和使用时长相关，使用者需要根据实际需求和预算选择合适的实例类型。

  5. 可扩展性：

     * AWS Instance Type 允许用户在需要时动态扩展计算资源，以满足应用程序的需求。例如，可以在高峰期增加实例数量，在平峰期减少实例数量。

---

## 2. 透過 CLI 走 SSH 連到 AWS EC2 部署

```javascript
chmod 400 /Users/baisongzhang/Desktop/demo.pem
ssh ubuntu@13.231.222.121 -i /Users/baisongzhang/Desktop/demo.pem
mkdir Code
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
npm install
npm run build
nohup node dist/index.js & exit
```

---

## 3. 透過 Route 53 (DNS) 去綁定域名跟 IP

* 连接 AWS EC2 实例到 GoDaddy 域名的流程
collapsed:: true

  1. 在 GoDaddy 上购买域名：首先，您需要在 GoDaddy 上购买域名。 您可以通过 GoDaddy 的网站完成此操作。

  2. 在 AWS EC2 中创建实例：接下来，您需要在 AWS EC2 中创建一个实例，并将您的网站配置到该实例上。

  3. 配置 Route 53：接下来，您需要在 Route 53 中创建一个主机记录，并将其关联到您的 EC2 实例。 您需要输入您的域名名称以及您的 EC2 实例的 IP 地址。

  4. 在 GoDaddy 上配置域名：最后，您需要在 GoDaddy 上配置您的域名，以将其关联到您的 Route 53 主机记录。 您可以通过 GoDaddy 的网站完成此操作，并输入您在 Route 53 中的主机记录的名称和值。

  现在，当用户访问您的域名时，Route 53 将解析域名并将其重定向到您的 EC2 实例。 用户将可以通过您的域名访问您的网站，并与您的 EC2 实例进行通信。

* 当用户在浏览器中输入您的域名时，发生以下交互
collapsed:: true

  1. 浏览器向域名系统 (DNS) 发送请求：浏览器使用可靠的传输协议 (TCP) 向域名系统发送请求，以确定该域名对应的 IP 地址。

  2. DNS 使用 Route 53 解析域名：在本例中，Route 53 作为域名系统，接收到了浏览器的请求。Route 53 会检查其内部的记录，确定该域名对应的 IP 地址。

  3. Route 53 将 IP 地址返回给浏览器：Route 53 使用 TCP 协议将该域名对应的 IP 地址返回给浏览器。

  4. 浏览器向该 IP 地址发送请求：浏览器使用 TCP 协议向该 IP 地址发送请求，以获取您的网站的内容。

  5. EC2 实例处理请求并返回内容：您的 EC2 实例接收到了浏览器的请求，处理该请求并返回您的网站的内容。

  6. 浏览器显示网站内容：浏览器接收到 EC2 实例返回的内容，并将其显示在网页上。

  总之，当用户访问您的域名时，Route 53 负责解析域名并确定该域名对应的 IP 地址，而 EC2 实例负责处理请求并返回您的网站的内容。

---

## 4. 在 EC2 設定 Nginx 轉址到 Port 80

* Code

  ```javascript
  sudo apt-get install nginx
  sudo vim /etc/nginx/sites-available/default
  sudo service nginx restart
  ```

  ```javascript
  server {
          # 監聽 443 埠，使用 SSL 加密，支援 HTTP2 協定，並設為預設伺服器。
          # 監聽 IPv6 的 443 埠，使用 SSL 加密，支援 HTTP2 協定，並設為預設伺服器。
          # 包含名為 ssl-boison.online.conf 的 SSL 設定片段。
          # 包含名為 ssl-params.conf 的 SSL 參數設定片段。
  
  
          listen 443 ssl http2 default_server;
          listen [::]:443 ssl http2 default_server;
          include snippets/ssl-boison.online.conf;
          include snippets/ssl-params.conf;
  
          # 設定網站根目錄為 /var/www/html
          # 設定伺服器名稱為通配符 "_" 
          # Set the server name to "_", a special value in Nginx that matches any name
  
          root /var/www/html;
          server_name _;
  
          location / {
              # 設定代理伺服器為 localhost:3000
              # Pass incoming requests to the localhost on port 3000
              # 設定代理使用 HTTP 版本為 1.1
              # Set the HTTP version to 1.1
              # 設定代理抬頭升級請求
              # Set the "Upgrade" header to the value of the "Upgrade" header in the incoming request
              # 設定代理抬頭連接類型
              # Set the "Connection" header to the string "upgrade"
              # 設定代理抬頭主機名稱
              # Set the "Host" header to the value of the "Host" header in the incoming request
              # 設定代理升級請求時跳過快取
              # Bypass cache for requests with an "Upgrade" header
  
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection 'upgrade';
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
          }
          # 設定 well-known 路徑可以被任何人存取
          # Match any request under the ".well-known" directory
          
          location ~ /.well-known {
                  # Allow all requests to be processed
                  allow all;
          }
  }
  
  server {
    # 監聽 IPv4 & IPv6 80 埠
    # 設定伺服器名稱為 boison.online
    # 將 HTTP 請求重定向到 HTTPS
  
    listen 80;
    listen [::]:80;
    server_name boison.online;
    return 301 https://$server_name$request_uri;
  }
  
  server {
    # 監聽 IPv4 & IPv6 80 埠，並設為預設伺服器
    # 設定伺服器名稱為 www.boison.online
    # 將 HTTP 請求重定向到 HTTPS
  
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name www.boison.online;
    return 301 https://$server_name$request_uri;
  }
  ```

* 解釋
collapsed:: true

  * These two lines include external configuration files. The **`ssl-boison.online.conf`** file contains the SSL certificate and private key for the **`boison.online`** domain. The **`ssl-params.conf`** file contains SSL configuration parameters.
collapsed:: true

    ```javascript
      include snippets/ssl-boison.online.conf;
      include snippets/ssl-params.conf;
    ```

  * This **`location`** block defines the configuration for requests to the root **`/`** path. The **`proxy_pass`** directive specifies that incoming requests should be forwarded to the specified URL. In this case, the URL is **`http://localhost:3000`**, which is the local address and port of the web application. The **`proxy_http_version`** directive sets the HTTP version to use for the proxy connection to the backend. The **`proxy_set_header`** directives set various headers for the proxy request. The **`proxy_cache_bypass`** directive sets a variable to bypass any caching that may be performed by a proxy server.
collapsed:: true

    ```javascript
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    ```

  * This **`server`** block listens on port 80 for incoming requests to the **`boison.online`** domain. The **`return`** directive sends a **`301 Moved Permanently`** HTTP response, redirecting the client to the secure **`https://`** version of the same URL.
collapsed:: true

    ```javascript
    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name www.boison.online;
        return 301 https://$server_name$request_uri;
    }
    ```

---

## 5. 在 EC2 安裝 Git 去更方便的部署

```javascript
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
git remote remove origin
git remote add git@github.com:BoisonChang/typescript-express-aws.git
git push --set-upstream origin main
```

---

## 6. 安裝 Certbot 取得 SSL 憑證

* SSL憑證(Secure Socket Layer certificate)
collapsed:: true

  * 定義

    * SSL 會將Client端與Server端之間所傳輸的資料進行加密，而這些加密後的資料會受到安全機制的保護，防止在資料傳輸時，遭到第三方攔截或修改資料。當web擁有SSL憑證時，網址會由HTTP轉為HTTPS，意謂著這個web是有經過CA(Certificate Authorities)認證的，可以放心瀏覽web內容。

  * SSL的好處：

    * 用於識別web身份。

    * 將client端及server端之間的資料進行加密，保護敏感資料。

* 安裝 Certbot
collapsed:: true

  ```javascript
  sudo add-apt-repository ppa:certbot/certbot
  sudo apt-get update
  sudo apt-get install certbot
  ```

* 修改 Nginx 設定
collapsed:: true

  ```javascript
  sudo vim /etc/nginx/sites-available/default
  
  server {
  ...
    location ~ /.well-known {
         allow all;
    }
  
  ...
  }
  
  sudo systemctl restart nginx
  ```

* 申請憑證
collapsed:: true

  * 使用 certbot 這個工具來通過 webroot 驗證方式獲取 SSL 憑證\
    collapsed:: true
    這個步驟的目的是為了獲取 SSL 憑證，以提供加密的 HTTPS 網站連接。要達到的功能是使用 certbot 這個工具來通過 webroot 驗證方式獲取 SSL 憑證，以保證您的網站連接是安全的。

    ```javascript
    sudo certbot certonly --webroot --webroot-path=/var/www/html -d domain.com -d www.domain.com
    
    參數說明：
    certonly：只獲取 SSL 憑證，不進行自動配置。
    --webroot：使用 webroot 驗證方式。
    --webroot-path：指定網站根目錄路徑。
    -d domain.com：指定要獲取憑證的網站域名。
    -d www.domain.com：指定要獲取憑證的 www 網站域名。
    ```

    &nbsp;

  * webroot 驗證方式

    * webroot 驗證方式獲取 SSL 憑證，是通過在網站伺服器的指定目錄中創建特殊文件，以驗證域名所有權的一種方式。

    * 這種方法允許憑證授權機構通過檢查這些文件的存在確認您控制該域名。在此命令中指定的 webroot 目錄為“/var/www/html”。正在驗證的域名是“domain.com”和“***[www.domain.com”。](http://www.domain.xn--com-9o0a./)***

  * 憑證放的位置為 /etc/letsencrypt/live/www.domain.com/fullchain.pem

  * 為了增加安全性 我們要產生一個 2048-bit Diffie-Hellman 的密碼組合，會產生在/etc/ssl/certs/dhparam.pem
collapsed:: true

    ```javascript
    sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
    ```

* Nginx 設定憑證與啟動 https

  * 要在位置產生一個 nginx的configuration snippets 來設定 SSL Key 跟 Certificate  輸入以下指令

    ```javascript
    // 透過 sudo 指令，以 root 身份執行 vim 程式，打開檔案
    sudo vim /etc/nginx/snippets/ssl-domain.com.conf
    ```

  * 在檔案中加上以下內容 (注意:請依照自己的實際檔案位置修改domain.com的部分)

    ```javascript
    // 指定 SSL 憑證的路徑，憑證由 Let's Encrypt 發行，路徑為以下
    ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem;
    
    // 指定 SSL 憑證的私鑰路徑，私鑰的路徑為 
    ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem;
    ```

  * 接下來產生另一個nginx的configuration snippets來設定Strong Encryption 輸入以下內容

    ```javascript
    // 透過 sudo 指令，以 root 身份執行 vim 程式，打開檔案
    sudo vim /etc/nginx/snippets/ssl-params.conf
    ```

    ```javascript
    // 定義要支持的 SSL/TLS 協議版本，這裡支持 TLSv1，TLSv1.1 和 TLSv1.2。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    // 強制 Nginx 服務器使用伺服器支持的加密算法。
    ssl_prefer_server_ciphers on;
    // 定義 Nginx 服務器支持的加密算法。
    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
    // 定義 ECDH 加密算法的曲線。
    ssl_ecdh_curve secp384r1;
    // 定義 SSL 工作階段緩存的大小。
    ssl_session_cache shared:SSL:10m;
    // 禁用 SSL 工作階段票證。
    ssl_session_tickets off;
    // 啟用 SSL Stapling 功能。
    ssl_stapling on;
    // 啟用 SSL Stapling 驗證功能。
    ssl_stapling_verify on;
    // 定義 DNS 解析器的 IP 位址，這裡使用 Google 的公用 DNS。
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    // 定義 DNS 解析器的超時時間。
    resolver_timeout 5s;
    // 增加 X-Frame-Options 頭部，以防止網頁被以 frame 的方式嵌入其他網站。
    add_header X-Frame-Options DENY;
    // 增加 X-Content-Type-Options 頭部，以防止瀏覽器進行 MIME 类型探測。
    add_header X-Content-Type-Options nosniff;
    // 指定 DH 參數的位置
    // Diffie-Hellman (DH) 算法是一种密钥交换协议
    // 在 SSL/TLS 协议中用于生成对称密钥
    // 通过指定 DH 参数的位置，可以在 SSL/TLS 协议中使用更安全的密钥交换方式。
    ssl_dhparam /etc/ssl/certs/dhparam.pem;
    ```

    &nbsp;

> 實作專案：[Github Repo](https://github.com/BoisonChang/typescript-express-aws)｜[Demo](https://www.boison.online/)

---

> **資料來源**：[\[教學\] 申請Let’s Encrypt憑證與啟用https (Nginx)](https://xenby.com/b/101-%E6%95%99%E5%AD%B8-%E7%94%B3%E8%AB%8Blets-encrypt%E6%86%91%E8%AD%89%E8%88%87%E5%95%9F%E7%94%A8https-nginx)