---
title: "百日轉職前端工程師：第十四週伺服器 Server 與遠端部署 《DAY 22》"
slug: Transfer-FrontEnd-100-22
date: 2020-11-22T17:48:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "AWS"]
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
---
大家好，這是百日轉職前端工程師的 Day22，也是 11/22（日），這週的主題會談到的是伺服器以及如何遠端部署，如果你想要自己架設一個網站，這週的主題會是最核心的基礎，透過購買虛擬主機你將能擁有一個遠端運作的主機，但你同時也必須瞭解部署的基本概念，並且透過電腦的非圖形化介面去下指令完全安裝的一些操作，雖然第一次學習滿多陌生專有名詞但完成令人滿有成就感的。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、虛擬空間、虛擬主機以及實體主機的差別

### 1. 實體主機
> **實體主機（Dedicated Server）其實就是一台獨享的電腦硬體**
- 我們架設網站時，就得在這台電腦硬體上安裝作業系統（例如 Linux 的 Redhat 或是 Windows  Server）、軟體、接上網路
- 這樣就可以透過網路連上這台電腦、放上網頁
- 但是很少人會買實體主機來放網站，因為自己使用一台實體主機，硬體費用、網路費、管理費、軟體費…零零總總的開銷非常多，都得自己出（依規格不同，一個月基本的費用超過1萬不算多）
- 除此之外本身也需要有硬體專業才有辦法在出問題或者要升級時去拆解主機

### 2. 虛擬主機

> **虛擬主機（Shared hosting）又稱共享主機、網頁空間，和 Web hosting......等。**
- 一台電腦自己用成本很高，但對一般網站來說，一台實體主機的資源還有很多用不到，那就發展出很多人一起用一台的虛擬主機模式
- 概念上就是像青年旅社，一個 CPU 核心分給很多人一起用，一個人約莫可以分到 3–5% 的資源可以使用
- 雖然資源沒有這麼充裕，但可以透過不同的方式分散流量帶來的負擔
- 由於虛擬主機的廠商無法開放用戶升級硬體規格，僅可以選擇不同的方案提高 CPU 資源，所以在 CPU 擴充上，虛擬主機的自主性不高
- 價格方面虛擬主機可以說是最便宜的方式了
- 但是因為用的是同一個空間，所以如果其中一個用戶的流量過大，就會影響到其他用戶的穩定性，就像住套房的通鋪，如果有一個人長期占用房間的廁所、電話一樣，而這中間不僅只是影響到頻寬，也會影響到像是 CPU、硬碟等等的資源佔用
- 因為所有的虛擬主機都歸類在同一部主機中，所以可以共用相同的組態設定，或是共用彼此的程序集，來節省硬體端的維護成本
- 最大宗的應用在 HTTP、FTP、EMAIL 等服務，用戶不需要像過去一樣，為了架網站就必須自行租賃機房或是購買主機等，透過這樣的虛擬主機服務，不僅可以節省硬體設備維護費用，關於網路頻寬、以及一些高技術性的維護等等，也可以委由租賃方來服務，可以大大降低自行處理的問題。
- 另外虛擬主機皆採用 cPane 控制台操作，也有中文介面可以選擇


標題指的「虛擬空間」，在網路上並無查到嚴格定義，可能比較接近此處所指的虛擬主機，因其使用上的彈性較低，比較像是單純的得到一個額外空間可以用來部署網站，而實際上 VPS 也能夠單純切出一個虛擬儲存空間去供自己 / 他人做應用。


### 3. VPS 主機

> **VPS（Virtual Private Server）又稱虛擬私人主機**
- 是將一台實體伺服器分割成多個虛擬專享伺服器的服務
- 概念上就像是買了一個小套房一樣感覺，你花錢租到的不僅僅只是一個床位，而是擁有了包含衛浴、電話、網路..等等的設施
- 每個 VPS 都可分配獨立公網 IP 位址、獨立作業系統、記憶體、CPU 資源，等等，也可以做獨立的排程來和其他系統做到實際上的隔離
- VPS 就像一台獨立的實體伺服器一樣，你可以在上面做重灌作業系統，安裝程式，單獨重新開機伺服器等等服務
- 如果你購買的資源夠大，你也可以當二房東來做Web Hosting 的販售服務
- 而 Web Hosting 的特性就只是單純的可以租借一個從實體 CPU 切出來，可以擺放資料的虛擬主機空間，而且並不處理如 Linux 指令
- 再來是 VPS 可以獨立配到自己的 IP，而非 Web Hosting 是透過網址 DNS 對應的方式來做 IP 上的辨別，這樣也在 SEO 上會有比較顯著的提升
- 而這樣的方式在資安上面也會有比較高的防護性
- 如果你要架構的是一個線上交易或是電子商務的平台，追求的是系統穩定性、服務品質以及資訊安全，那 VPS 一定是優於 Web Hosting 的選擇。


VPS 與 Shared hosting 最大的差異點，因為 Shared hosting 除了是共用同一台實體機外，也分享著同一個系統資源的關係， 像是 Linux, Apache, SQL 等，也必須是共用的。更別提你不能「超量使用」這個問題，因為你一但超量就有可能影響到其他使用者的效能。等於是在同一台電腦裡不同的使用者帳戶「同時」使用一樣的電腦資源（你多用另一人就只能少用的概念）。如果一台電腦有 4GB 的 RAM（實際上不會那麼少），作業系統會看到 4GB。當你使用了 3GB，另外一邊只會剩下 1GB。


### 4. 雲端主機

雲端主機（Cloud Computing） 是雲端運算平台，以豐富的雲端生態系見長，除了虛擬機，還有資料庫、檔案儲存以及各式各樣的代管服務。

> **和 VPS 相比，雲端運算平台由多個實體主機來分載運算資源，因此可以做到**
- 需要擴增效能時，可以在幾秒內完成擴增
- 擁有防火牆、流量控管等諸多功能確保服務不會出錯
- 如果其中一個分載的實體主機掛掉，會馬上轉移到另一個實體主機上


在雲端主機中你對伺服器資源有幾近無限的自由度，也可以在主機執行時添加資源。你可以自由的增加和減少依據您的需求而無需重新啟動伺服器，而根據你使用的資源量計算費用。

> **例如，如果網站突然開始接收大量的流量**
- 你可以登錄到雲端主機控制面板，並增加更多的頻寬、流量和伺服器的 RAM，反之
- 而「在雲端中」是指你的網站代管資源池的虛擬機管理程式，在一個集中的儲存區域稱為 SAN Storage 和資料託管
- 所以，如果你的虛擬機管理程式發生故障，此時將會轉移資料到另一個管理程式，從而大量減少伺服器停機時間
- 更重要的是數據不會遺失，因為它是在 SAN Storage 上傳播：所以如果一個 SAN 失敗，另一個便會接管


> **目前市面上的雲端服務目前大都劃分為 3 大部份如下**

或許大家都聽過網格運算、叢集運算、公用運算，我們可以把他看做分散式運算的演進，目前較成熟的就叫 Cloud computing，AWS 屬於下文中的 IaaS（Infrastructure as a Service），能提供的服務更強大，而且幾乎不用自己建置設備。

####  SaaS（Software as a Service）

有些人的認知是這就是軟體租用服務或 application on-demand，比較著名的幾個軟體服務如 Gmail、Webex......。

#### PaaS (Platform as a Service)

最著名的就是 Google appengine、SaleForce force.com及Microsoft Azure platform，他們的共同點都已有全球化的軟硬體建設環境，並有成熟的網路服務開發平台，透過完全性、穩定性的考量，重新定義平台，提供給用戶完善的的開發環境去佈署網路應用程式。

Amazon AWS 以 IaaS 出發，目前也跨足到 PaaS 上來了，開發者可以利用平台的程式框架、storage、database，開發需要大量運算或執行的 application 。

#### IaaS （Infrastructure as a Service）

- 又稱 CaaS（Computer as a Service）或 HaaS（hardware as a Service）
- IaaS 的 vendor 有 Amazon 的 AWS，中華電信最近推出的HiCloud、GoGrid、Rackspace
- 可以依自己的使用量隨時取用一大群的Server，也可以隨時關閉
- 伺服器的使用價格可以小到以小時計算，Storage 、Database 也都已使用量來計算費用
- 申請完後，你就可以直接管理這些虛擬的 Server
- 可以說是 VPS 的進階版，整體都是虛擬機器的概念，可以依用戶需求隨時做資源調配


> 參考資料:
1. [請問 VM 和 VPS 的差異？](https://ithelp.ithome.com.tw/questions/10193833?sc=rss.qu)
2. [學習 VPS 的重要性與各大 Linux VPS 比較](https://progressbar.tw/posts/131)
3. [架站用 VPS 還是 Web Hosting，差在哪？](https://progressbar.tw/posts/102)
4. [【VPS】什麼是VPS? 使用VPS有什麼好處嘛?](https://coder.tw/?p=4526)
5. [雲端平台怎麼選？比較三大雲端供應商 GCP 與 AWS 與 Azure](https://ikala.cloud/google-cloud-v-aws-v-azure/)
6. [雲端服務與虛擬主機之比較(一)](https://coolman911.wordpress.com/2010/04/02/%E9%9B%B2%E7%AB%AF%E6%9C%8D%E5%8B%99%E8%88%87%E8%99%9B%E6%93%AC%E4%B8%BB%E6%A9%9F%E4%B9%8B%E6%AF%94%E8%BC%83%E4%B8%80/)


---

## 二、什麼是網域（Domain）？

> **網域名稱（Domain），簡稱域名、網域**
- 是由一串用點分隔的字元組成的網際網路上某一台電腦或電腦組的名稱，用於在資料傳輸時標識電腦的電子方位
- 域名可以說是一個 IP 位址的代稱，目的是為了便於記憶後者


> **例如，google.com 是一個域名，和 IP 位址 172.217.160.78 相對應**
- 人們可以直接存取 google.com 來代替 IP 位址
- 然後域名系統（DNS）就會將它轉化成便於機器辨識的 IP 位址
- 這樣，人們只需要記憶 google.com 這一串帶有特殊含義的字元，而不需要記憶沒有含義的數字


> **域名的核心是域名系統（Domain Name System，DNS）**
- 域名系統中的任何名稱都是域名。在域名系統的階層中，各種域名都隸屬於域名系統根域的下級
- 域名的第一級是頂級域，它包括通用頂級域，例如 .com、.net 和 .org，以及國家和地區頂級域，例如 .us、.cn 和 .tk
- 頂級域名下一層是二級域名，一級一級地往下
- 這些域名向人們提供註冊服務，人們可以用它建立公開的網際網路資源或執行網站
- 頂級域名的管理服務由對應的域名註冊管理機構（域名註冊局）負責
- 註冊服務通常由域名註冊商負責


網域名稱水還很深，這邊再簡單介紹一點如這個網址其結構為：
> http://www.youtube.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocum
- 通訊協定（protocol）
    - 又稱為 scheme，一般最常見的是 http 或 https ，其他還有 ftp 等等
    - 這個部分根據 RFC 可以不分大小寫，但正式的情況下還是會使用小寫
- 第三層網域名稱
    - 上述 www 這個部分，又稱子網域（subdomain）或主機名（host）
- 第二層網域名稱（Second Level Domain, SLD）
    - 上述 youtube 的部分，又稱為網域名稱
- 頂級網域（Top Level Domain, TLD）
    - 上述 .com 的部分，就是最高層級的網域
    - 簡單說，就是網址的最後一個部分
- 連接埠（port）
    - 預設是 80，所以可以不用填
- 網頁路徑（Path）
    - 上述 /path/to/myfile.html 的部分，這個部分根據伺服器的作業系統有所不同，建議不要更改大小寫
- 參數（parameters）
    - 上述 ?key1=value&key2=value2 的部分
    - 用來傳訊息給伺服器
    - 大小寫不能任意更改
- 錨點（anchor）
    - 上述 #SomewhereInTheDocument 的部分
    - 可以對應到網頁 HTML 中的 id 或 name
    - 大小寫不能更改


> 參考資料
1. [網域名稱是什麼？](https://ithelp.ithome.com.tw/articles/10214204)
2. [[網際網路] 認識網址與網域名稱（Domain Name, URL, DNS）](https://pjchender.dev/internet/internet-domain-dns/)

---

## 三、如何設定網域（A、C NAME）

- 讓我們先從註冊一個域名說起，最早註冊一個域名是不需要費用的
- 但後來影響域名價格的因素是域名管理局的收費
- 域名管理局是負責特定頂級域名的公司
- 頂級域名就是網址結尾 “.” 點後的部份
- 目前最受歡迎的頂級域名為 .com，而其它熱門域名有 .net 和 .org 等等
- 但當然不只這三種域名，今日已有上千種不同的頂級域名，其註冊價格也都有所不同
- 價格不同是因為每一個頂級域名都是由不同的域名管理局管理，這些公司有各自管理旗下域名規則的權利
- 而他們現在面臨的挑戰是要為這些域名找到合適的出售價格
- 購買域名你可以透過域名註冊代管公司如 Godaddy 和 Gandi，它們會代為你處理註冊事宜將步驟簡化並提供些額外服務，但也會酌收低額費用

> **接下來你可以在域名註冊代管公司中去設定你購買的網域的以下參數**
- A Record （A 紀錄）
    - 是最常見的設定方式，就是把網址與 IP address 做雙向對應的綁定
    - 這綁定的事 ipv4，如果要綁 ipv6 要用 AAAA Record
- C Name Record （別名紀錄）
    - 可以給 A Record 使用另外一筆（或多筆）名稱讓外面查詢
    - 例如把 www.AAA.com 與 AAA.com 一起綁定到 111.11.11.1 的這台主機上
    - 而 DNS server，就是主要進行 DNS 設定的主機。
- MX Record （郵件交換紀錄）
    - 是用來「收」郵件的
    - 讓你可以用自己的 xxx@domain.name來收信。
- TXT （文字紀錄）
    - 隨意的文字，常見用來放一些驗證
    - 如 google-site-verification

> 參考資料
1. [註冊一個域名要花多少錢？](https://news.gandi.net/zh-hant/2020/09/how-much-does-a-domain-cost-and-what-comes-with-it/)
2. [網域 Domain 的申請](https://ccckmit.medium.com/%E7%B6%B2%E5%9F%9F-domain-%E7%9A%84%E7%94%B3%E8%AB%8B-77f08e0b7af7)
3. [網址是什麼? 如何買網址?，9個常見問題彙整給您](https://www.newscan.com.tw/all-faq/faq-detail-58.htm)


---

## 四、如何用 SSH 遠端連線到自己的主機

> **SSH 是 Secure Shell protocol 的縮寫**
- 這是一個加密的協定，所以傳送出去的封包都是經過加密的，即使被擷取，也需要很大的功夫才能解開，比較安全
- 此外，SSH 也是一個會壓縮的協定，因此可以節省頻寬、加速傳遞資料等
- 不只如此，SSH 不只可以達到 Telnet 的遠端連線功能
- 還可以提供類似 FTP、但較安全的 sFTP 功能（FTP 也是明碼）等等等許多應用
- 如果你的電腦環境預設沒有 SSH，則需要另外安裝
- 透過 SSH 遠端連線的功能，可以輕易地將一台裝有 Ubuntu 作業系統的電腦建置成簡易的伺服器（Server），允許其他電腦遠端登入操作


> **由於 EC2 的遠端主機已經安裝和設定好 SSH，因此我們只需下載私鑰後使用 SSH 遠端連線進入**

```
Permissions 0640 for ‘/Users/●●●/Desktop/●●●.pem’ are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key “/Users/●●●/Desktop/●●●.pem”: bad permissions
Permission denied (publickey).
```

初次使用 Key Pair 登入 EC2 可能會遇到這個警告，通常是因為這個鑰匙檔的權限太過於公開，不過其實剛剛上圖「Connect」訊息中就有提示，我們也可以直接使用終端機來處理這個問題。

```
chmod 400  ~/路徑/密鑰名稱.pem
```
由上述指令將 .pem 檔改成只有使用者可以讀取的權限，接下來則可以使用 SSH 輸入私鑰登入 EC2

```
ssh -i "<私鑰檔案路徑>" ubuntu@ec2-<IPv4 位置>.us-east-2.compute.amazonaws.com
```
照理說能夠成功登入

> 參考資料
1. [連線 EC2 三種方法 – SSH、Instance Connect、System Manager](https://www.ecloudture.com/%E9%80%A3%E7%B7%9A%E8%87%B3ec2%E7%9A%84%E4%B8%89%E7%A8%AE%E6%96%B9%E6%B3%95%E8%88%87%E6%AF%94%E8%BC%83-ssh%EF%BC%8Cec2%E5%AF%A6%E4%BE%8B%E9%80%A3%E6%8E%A5%EF%BC%8C%E7%B3%BB%E7%B5%B1%E7%AE%A1%E7%90%86/)
2. [從本機 Windows 連線至 AWS EC2 的 Linux Instance (筆記)](https://medium.com/@charming_rust_oyster_221/%E5%BE%9E-windows-%E9%80%A3%E7%B7%9A%E8%87%B3-linux-instance-%E7%AD%86%E8%A8%98-732a23ccbb3e)
3. [SSH 遠端連線回家中的 Windows 電腦](https://medium.com/@fortunatemaker2603/ssh-%E9%81%A0%E7%AB%AF%E9%80%A3%E7%B7%9A%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%9A%84-windows-%E9%9B%BB%E8%85%A6-7e5267ae1e93)


---

## 五、伺服器運作的架構 LAMP

以下先講整個讓伺服器運作的架構 LAMP，再回來講部署的過程，才能夠了解部署中每個步驟安裝的工具背後的含意。

> **LAMP 是透過 Linux + Apache + MySQL + PHP 這種方式來架設網站伺服器組合的簡稱**

其實這些開源軟體一開始並不是被設計為一定要綁在一起使用的，例如 WAMP 就是以 Windows 取代 Linux 作為該開發環境的作業系統。

> **但回到 LAMP 其分別代表著**
- Linux 作業系統
- Apache http 網頁伺服器
- MySQL 資料庫
- PHP 網頁後端語言

> **在整個平台設計上面，目前常見的有兩大系統**
- 一個是 Linux 作業系統上面，搭配 Apache + MySQL + PHP 等而達成，這個系統被稱為 LAMP
- 另一個則是微軟的 IIS + MSSQL + ASP (.NET) 伺服器
- 在能見度與市佔率方面，應該還是以 LAMP 為主


> **在 LAMP 裡面 Linux 和其他元素就讓我們來談談：**

### 1. Linux

- Linux 處在最低層，提供作業系統
- 其他每個組件實際上也在 Linux 上運行，但是，並不一定局限於 Linux
- 如有必要，其他組件也可以在 Microsoft® Windows®、Mac OS X 或 UNIX® 上運行

### 2. Apache

- Apache 是目前最受歡迎的網頁伺服器
- 網頁伺服器負責在網頁之間傳遞資訊並遵循 HTTP（通訊安全協定）的規範、將網頁資料儲存於伺服器
- 當使用者在網址列上輸入一串網址時，會向 Server發出一個請求，而 Server就會遵照 HTTP協定取出該網頁的資料並回傳給使用者
- 在 Internet 中，有 65% 的網頁都以Apache為網頁伺服器
- 優點除了為開源軟體，可依使用者需求加以客製化之外，也相當安全可靠
- 1995 年以前就有很多的 WWW 伺服器軟體，其中以 HTTPd 佔有率較高
- 後來 HTTPd 經過多次臭蟲的修訂後，才在 1995 年後發佈 Apache （A patch server）
- 主要提供 WWW 的伺服器平台，後面談到的 PHP 必須要在這玩意兒上才能運作

> **在 LAMP 架構的網站中，當 Client 向 Server 上的 Apache 要求連線後，若 Apache 發現它要求的網頁檔是 PHP 撰寫時**
- 就會先送到 PHP 直譯器去執行
- 若程式碼裡面有需要查詢資料庫的地方，就由 PHP 引擎和 MySQL 查詢資料
- 然後 PHP 把單純的 HTML 結果送回去給 Apache
- 最後交給 Client 端的瀏覽器顯示在銀幕上
- 所以在客戶端是完全沒有執行到程式的，它只不過是把伺服器送來的純網頁顯示到銀幕上而已
- 所有的計算都是在伺服器上完成，所以伺服器上也可以選用不同的網頁程式語言，如 jsp、perl 或 python 等，完全不會影響到客戶端的結果


### 3. MySQL

- 傳統的檔案讀取是很麻煩的，如果你只要讀取該檔案當中的一小部分，系統還是會將整個檔案讀出來
- 若又有多人同時讀取同一個檔案時，那就會造成效能與系統上的問題，所以才會有資料庫系統的推出。
- 資料庫其實是一種特殊格式的檔案，這種檔案必需要透過特殊介面（資料庫軟體）來進行讀寫
- 由於這個特殊介面已經針對資料的查詢、寫入做過最佳化設計， 因此很適合多人同時寫入與查詢的工作
- 針對資料庫的語法有所謂的 SQL 標準語法，任何根據這種資料檢索語法發展出來的資料庫，就稱為 SQL 資料庫
- 比較知名的自由軟體資料庫系統有 MySQL 及 PostgreSQL
- 其中 MySQL 的使用率又比較高一些
- MysQL 可以透過網頁程式語言來進行讀寫的工作，因此很適合例如討論區、論壇等的設計， 甚至很多商業網站的重要資料也是透過 MySQL 這個資料庫軟體來存取

### 4. PHP

- PHP 是一個工具可以被用來建立動態網頁，PHP 程式碼可以直接在 HTML 網頁當中嵌入， 就像你在編輯 HTML 網頁一樣的簡單
- PHP 是一種『程式語言』，這種程式語言可以直接在網頁當中編寫， 不需要經過編譯即可進行程式的執行
- 由於具有：自由軟體、跨平台、容易學習及執行效能高等優點， 目前是很熱門的一個設計網頁工具
- 它主要會被嵌入在 HTML 上在 Web 伺服器上運行，當程式被瀏覽器請求時，程式會在伺服器上執行
- 結果會轉成普通的 HTML 文件回傳給 Client 端瀏覽器，所以整個流程在 Client 端是不需要計算的
- 因為它是直譯式語言，有優秀的跨平台能力，所以幾乎在不同的系統都可以運作一樣的程式碼，造就了各種不同 PHP 應用軟體的蓬勃發展


> 參考資料
1. [鳥哥的Linux 私房菜-- 伺服器架設篇](https://linux.vbird.org/linux_server/)

---

