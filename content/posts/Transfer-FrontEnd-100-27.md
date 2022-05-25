---
title: "百日轉職前端工程師：第十九週 產品開發流程《DAY 27》"
slug: Transfer-FrontEnd-100-27
date: 2021-02-22T22:27:21+08:00
draft: false
tags: ["FrontEnd", "Transfer"]
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

大家好，這是百日轉職前端工程師的 Day27，也是 2021/02/22（一），這週的主是產品開發流程，瞭解產品開發流程意味著工程師可以從更寬闊的視角去瞭解產品開發的全局，能夠更好的跟 PM（產品經理） 溝通，也能適時點出 PM 由於不熟悉技術所忽略的盲點，其中現在比較主流的開發方式為瀑布式開發，和敏捷式開發，會在文中詳細介紹。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、什麼是 Scrum？

Scrum 是「敏捷式（Agile）開發」底下的一種實作模式， 要聊到 Scrum 就先需要知道敏捷式（Agile）開發是什麼，並且早期廣為使用的「瀑布式（Waterfall）開發」。

在 Agile 中我們通常是以一個叫 User Story（使用者故事） 的東西，來描述 Product Backlog（產品待辦清單）的內容，但這並不是硬性規定的而是具有很大的彈性，各間公司很可能用不同的名詞去操作定義同樣的內容，所以後面的篇幅基本上都會帶到，並解釋這些名詞以及功能。

### 1. 產品開發方式

目前主流的開發方式有兩種，近代竄起流行於新創企業的敏捷式（Agile）開發，以及較多大型企業仍在使用的瀑布式開發（Waterfall）。

傳統的開發方式稱為瀑布式開發（Waterfall），使用預測性的設計流程，開發階段就像瀑布一樣從上往下，建立嚴謹、標準的開發程序，清楚的階段劃分，易於分工及責任歸屬，其在一開始更需要完整的溝通和規劃，讓整個較長的開發流程能夠順利無誤地進行，並且完時交付一個符合客戶需求的完整產品。

而敏捷式開發則是將這一串的流程拆解成更短的週期，普遍來說是兩週完成一個瀑布式開發的循環，理念是盡快將產品/功能做出來實際去展示給市場/客戶，瞭解其反饋後，快速修正，並且進到下一個循環，以免最後花了半天好不容易做出來的產品與客戶/市場需求不符合，徒生遺憾，如下圖所示，每一個圓環都跑完一次瀑布式開發的流程並生出一個可展示的產品/功能。


> **以下分別列出這兩種開發方式的優缺點（此處列的並非絕對）**

#### 瀑布式（Waterfall）開發

- 優點
    - 最容易管理的開發方式之一，其每個階段都有具體的可交付成果和審查程序
    - 它適用於需求易於理解的小型項目
    - 更快地交付項目
    - 流程和結果都有詳細記錄
    - 輕鬆適應變換團隊的方法
    - 此項目管理方法有助於管理依賴項
    - 瀑布模型非常適合已定義要求的項目，並且不會發生任何變化
- 缺點
    - 它不是大型項目的理想模型
    - 如果要求在開始時不明確，那麼這是一種不太有效的方法
    - 很難回到原來的階段做出改變
    v一旦開發結束，測試過程就開始了，若在以後的開發中找到錯誤，修復成本很高


#### 敏捷式（Agile）開發

- 優點
    - 專注的客戶的開發方式，確保客戶在每個階段都能不斷參與
    - 敏捷團隊要求積極主動，自我組織能力強，因此很可能從開發項目中獲得更好的結果
    - 敏捷開發的方法確保維持開發質量，隨時修正優化
    - 過程中需要不斷修正迭代，客戶和團隊能在過程中不斷討論，並以此為下次開發的開發依據，這降低了開發出的產品不符合需求的風險。=
    - 敏捷最適合頻繁更改需求的機會
- 缺點
    - 對於小型開發項目來說並不是一種有用的方式
    - 它需要專家在會議中作出重要決定
    - 與其他開發方法相比，實施敏捷方法的成本更高
    - 如果項目經理不清楚他/她想要的結果，項目很容易偏離軌道


事實上，敏捷式開發並不是一套被嚴謹被定義的方法，而是一種價值觀和開發原則，每一個團隊在採用敏捷式開發的精神發展出的最適開發模式可能都會有差異，重點是要讓團隊成員由上到下都確實掌握以下敏捷式（Agile）開發的十二大原則，根據不同團隊性質找到最適化的操作方法，而非拘泥於模仿別人的敏捷式開發模式。


> *敏捷式開發的十二大原則*
1. 我們最優先的任務，是透過及早並持續地交付有價值的軟體來滿足客戶需求。(Our highest priority is to satisfy the customer through early and continuous delivery of valuable software.)
2. 竭誠歡迎改變需求，甚至已處開發後期亦然。敏捷流程掌控變更，以維護客戶的競爭優勢。(Welcome changing requirements, even late in development. Agile processes harness change for the customer’s competitive advantage.)
3. 經常交付可用的軟體，頻率可以從數週到數個月，以較短時間間隔為佳。(Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale.)
4. 業務人員與開發者必須在專案全程中天天一起工作。(Business people and developers must work together daily throughout the project.)
5. 以積極的個人來建構專案，給予他們所需的環境與支援，並信任他們可以完成工作。(Build projects around motivated individuals. Give them the environment and support they need, and trust them to get the job done.)
6. 面對面的溝通是傳遞資訊給開發團隊及團隊成員之間效率最高且效果最佳的方法。(The most efficient and effective method of conveying information to and within a development team is face-to-face conversation.)
7. 可用的軟體是最主要的進度量測方法。(Working software is the primary measure of progress.)
8. 敏捷程序提倡可持續的開發。贊助者、開發者及使用者應當能不斷地維持穩定的步調。(Agile processes promote sustainable development. The sponsors, developers, and users should be able to maintain a constant pace indefinitely.)
9. 持續追求優越的技術與優良的設計，以強化敏捷性。(Continuous attention to technical excellence and good design enhances agility.)
10. 精簡──或最大化未完成工作量之技藝──是不可或缺的。(Simplicity — the art of maximizing the amount of work not done — is essential.)
11. 最佳的架構、需求與設計皆來自於能自我組織的團隊。(The best architectures, requirements, and designs emerge from self-organizing teams.)
12. 團隊定期自省如何更有效率，並據之適當地調整與修正自己的行為。(At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.)


而基於以上敏捷式（Agile）開發的十二大原則，也發展出了各種更細膩的實作模式，其中較有名的屬於「Kanban」（看板）和「Scrum」，由於網路上資料很多就不贅述，直接來談談 Scrum 的一些基本概念。

### 2. Scrum 的運作模式

基本上 Scrum 繼承敏捷式（Agile）開發快速迭代修正的精神，但其在角色區分和開發的運作上制定了一套可彈性調整的規則。


> **Scrum 中的三個角色**
1. Product Owner
- 通常這個角色會是由擁有這個產品的人擔任，通常會由 Product Owner 決定 Item 內容，並讓開發團隊自行拆解 Task 進行開發，這個人需要有權限決定最後產品的走向，能決定什麼要開發什麼不要開發，一般來說在公司中可能會由 PM（產品經理）擔任，但在更小的新創公司可能就是創辦人或者 CEO 擔任。

2. Scrum Master
- 要負責幫助大家跑 Scrum 這個開發方法的人，這個角色必須非常瞭解 Scrum 的開發方式和原則，帶領大家討論並且實際在開發中迭代推進，但實際上在規模小的公司中非常可能也是由 PM（產品經理）擔任。


3. Team
- 基本上就是開發團隊，也就是工程師和介面設計師們，負責實際的產品開發。


> 參考資料: [淺談重構](https://jimmylab.wordpress.com/2018/10/01/refactoring/)

---

## 二、Scrum 中通常會有哪些元素

大家會先在 Backlog（待辦清單）上列出想要開發的功能，Procuct Backlog 是這個產品所有要做的功能，而 Sprint Backlog （衝刺待辦清單）則是這個 Sprint 中所有我們要做的功能。


在 Sprint 開始時會有個 Spring Planning Meeting，會由PM（產品經理）大致上決定工程師要做什麼，然後工程師跟 PM 討論我們能做多少？通常會用人力時數（點數票）的方式投票每個功能要耗費多少點數票，讓 PM 能夠實際掌握狀況並將其這次 Spring 要開發的功能寫上這次的 Sprint Backlog（衝刺待辦清單），基本上 Sprint backlog（放置本次 Sprint 要開發的 Item）決定後就後若是有新的票要進來，必須要跟 PM 額外溝通，或者是就是不準插單，先跑完這輪再說。

Scrum 中重要的概念跟「Kanban」（看板）很像，最大的差異可能就是那個兩個禮拜循環一次的時程。而「Kanban」（看板）的開發週期則是可能更短，甚至可能到一兩天部署（Deploy）一次，但這兩種實作模式基本都是深刻應用敏捷式（Agile）開發的十二大原則，我認為 Kanban 可能更適合一到兩人的微型團隊去使用，基本上角色分配可以非常單純。


基本上由於開發週期只有兩週，每天都會有短暫的 Daily Standup / Daily Standup Meeting，去快速溝通 

1. 今天要做什麼
2. 昨天做了什麼
3. 然後開發中是否 遇到什麼問題

讓整個團隊的人都能快速瞭解每個人的開發情況，在後續 Daily Standup 讓 PM 去與遇到問題的 Team member 去溝通協調解決問題。至於為什麼要站著，大概是因為站著很累大家比較會長話短說吧（笑）。

Sprint 結束後，就會統一把 Sprint 開發的東西部署（Deploy）到測試環境去。而每一個 Sprint 跑完後會有一個 RetroSpective 的會議，去檢討這兩週做了什麼？哪裡做得不好？哪裡做得很好？以便在下一個 Sprint 更好的優化開發流程。


> **基本上完整循環的 Sprint 可能會有四種會議**

1. 衝刺計畫會議（Sprint Planning Meeting）
- 每個衝刺期間的一開始必須先舉行衝刺計畫會議
- 主要用來決定該衝刺期間的待辦項目，以及團隊的衝刺任務
2. 每日站立會議（Daily Standup Meeting）
- 衝刺期間的每一天早上，都要執行15分鐘的站立會議
- 主要可以用來了解團隊的工作執行狀況。
3. 衝刺審查會議（Sprint Review Meeting）
- 每個衝刺的最後一天會先執行衝刺審查會議，隨後執行衝刺回顧會議
- 在衝刺審查會議中，主要用來展示並了解該衝刺的待辦項目達成狀況
4. 衝刺回顧會議（Sprint Retrospective Meeting）
- 相較之下，前述的衝刺審查會議，其討論的主題鎖定在「產品」上頭
- 而此處的衝刺回顧會議，其討論的主題則聚焦在團隊的「開發程序」上頭
- 主要用來討論並調整下一期衝刺的開發程序


> **基本上完整循環的 Sprint 可能會有幾種項目和清單**

- Item（物件）
    - 又稱 Story，是 Product Owner 定義的產品產出。Item 大小要講究，要可以讓團隊在一般的速率下，可以完成3-5個。太多太繁雜，太少萬一沒做完就感覺整個 Sprint 一事無成，對團隊信心是個打擊
- Task（工作）
    - 是開發團隊針對 Item（物件），列出完成 Item 所需的工作。工作分配是開發團隊自己安排。Task 是完成 Story 的施工項目，在 sprint planning meeting 時，團隊會將每一個 Story 再細分若干個 Task。典型的 Task 有設計使用者介面，coding，寫自動化單元測試程式，設計資料庫表格格式，寫 DAO（data access object），寫自動化功能測試，寫使用手冊等等
- 產品待辦清單（Product Backlog）
    - 其實，產品待辦清單的概念很簡單，它主要包含了一般常見的功能性需求和非功能性需求；不過，比較特別的是，它還包含了技術團隊提出的需求，而不只是照顧到客戶的需求而已
    - 也因此，Scrum敏捷開發特別採用了「產品待辦清單」（Product Backlog）的字眼，用來跟傳統作法上只照顧客戶的系統需求，做字面上的分野。所有關於此專案或是產品的代辦事項就稱為 Product Backlog，放在 Product Backlog 裡面的東西稱為 product backlog item（PBI），需要依據優先順樹經過排序，比較重要，比較優先要施工的 PBI 放在前面
- 衝刺待辦清單（Sprint Backlog）
    - 衝刺待辦清單中的待辦項目，其實是產品待辦清單的子集。在每一次的衝刺規畫會議中，才會開會決定要處理哪些待辦項目。Sprint backlog 就是某一個 Sprint 準備施工的 User Story（可以想成是 Product Backlog 的子集合），同樣的 Sprint Backlog 裡面的 Story 也是要排序過的，比較重要的先開工
- 潛在可交付產品增量（Potentially Shippable Product Increment）
    - 開發團隊的產出，簡單的說就是 Product Owner 說要上線就可以馬上上線的東西才算數
- 燃盡圖（Burndown Chart）
    - 簡單來說，我們可以繪製燃盡圖，來呈現一個時間區段中，剩餘的工作量。所以，有些文章或書籍中，也將「Burndown Chart」中譯為「剩餘工作圖」或者是「剩餘時間圖」，原因正是如此
    - 嚴格講起來 brundown chart 可以分成三種，從大到小分別是 release burndown chart, Story burndown chart 以及 Task burndown chart，在這邊先解釋 Task burndown chart 
    - 在 sprint planning meeting 結束之後，先把所有 stories 的 tasks 施工的小時數加總起來，假設有 250 小時。然後第每一天 daily - Scrum 會議之後，Developer  會報告說有一些 tasks 已經被做完了，假設第二天有 15 小時的工作已經完成了，那麼Scrum 團隊就可以畫一張圖，把 250 小時減掉每天完成的工作時數，剩下來的就是尚未完成的工作。所以只要每天看這張 burndown chart，團隊就可以知道進度是否正常（理論上在 sprint 最後一天剩下未完成的工作時間應該是 0）


> 參考資料
1. [快速認識Scrum的三四三口訣](https://www.ithome.com.tw/node/68213)
2. [瀑布 VS. 敏捷：必須知道的差異](https://www.cybermedian.com/%E7%80%91%E5%B8%83-vs-%E6%95%8F%E6%8D%B7%EF%BC%9A%E5%BF%85%E9%A0%88%E7%9F%A5%E9%81%93%E7%9A%84%E5%B7%AE%E7%95%B0/)


---

## 三、什麼是 user story（使用者故事）

User Story（使用者故事）就是一個當你在寫 spec（規格） 時可以幫助你更清楚寫下你真正想要什麼的一個方法。你可以用這種方法去寫 spec，也可以不用，所以有些 PM 也有可能會不寫 User Story 。

更嚴謹的來說，User Story（使用者故事）是一段簡單的功能敘述，以客戶或使用者的觀點撰寫下有價值的功能 (functionality/feature)。與其說它是規格文件 (documentation)，不如說它代表客戶的一個需求而已，因為實做細節將延後至開發時才會確定。

> **敏捷宣言簽署者 Ron Jeffries 用三個 C 描述 User Story（使用者故事）**

- Card（卡片）
    - User Story（使用者故事）一般寫在小的記事卡片上。卡片上可能會寫上故事的替代描述，工作量排出等。
- Conversation（對話）
    - User Story（使用者故事）背後的細節預測和客戶或產品負責人的交流溝通。
- Confirmation（確認）
    通過驗收測試確認 User Story（使用者故事）被正確完成。

> **User Story（使用者故事）的六個特性：INVEST**
一個好的 User Story（使用者故事）應該遵循 INVEST 原則
- Independent（獨立性）
    - 要盡可能的讓一個 User Story（使用者故事）獨立於其他的 User Story。User Story 之間的依賴使得制定計劃，確定優先級，工作量估算都變得很困難。通常我們可以通過組合 User Story 和分解 User Story 來減少依賴性。
- Negotiable（可協調性）
    - 一個 User Story（使用者故事）的內容要是可以協商的，User Story 不是合同。一個 User Story 卡片上只是對 User Story 的一個簡短的描述，不包括太多的細節。具體的細節在溝通階段產出。一個 User Story 卡帶有了太多的細節，實際上限制了和用戶的溝通。
- Valuable（價值性）
    - 每個 User Story（使用者故事）必須對客戶具有價值（無論是用戶還是購買方）。一個讓用戶故事有價值的好方法是讓客戶來寫下它們。一旦一個客戶意識到這是一個 User Story 並不是一個契約而且可以進行協商的時候，他們將非常樂意寫下故事。
- Estimable（可估算性）
    - 開發團隊需要去估計一個 User Story（使用者故事）以便確定優先級，工作量，安排計劃。但是讓開發者難以估計故事的問題來自：對於領域知識的缺乏（這種情況下需要更多的溝通），或者故事太大了（這時需要把故事切分成小些的）。
- Small（短小）
    - 一個好的故事在工作量上要盡量短小，最好不要超過 10 個理想人/天的工作量，至少要確保的是在一個迭代或 Sprint 中能夠完成。User Story（使用者故事）越大，在安排計劃，工作量估算等方面的風險就會越大。
- Testable（可測試性）
    - 一個 User Story（使用者故事）要是可以測試的，以便於確認它是可以完成的。如果一個 User Story 不能夠測試，那麼你就無法知道它什麼時候可以完成。一個不可測試的 User Story 例子：產品應該是易於使用的。

> **幾個 User Stories 的範例如下**
**範例 1**
- 身為使用者，我可以：
    - 編輯我的個人檔案
    - 新增、修改、刪除文章
    - 文章可支援markdown語法
    - 可上傳圖片
    - 看到貼文的留言
    - 將貼文做分類
- 身為訪客，我可以：
    - 看到所有的貼文
    - 可以對於貼文留言
    - 可以利用分類來篩選文章

**範例 2**
- 需求
    - Markdown 的文字方式
- User Story:
    - 我在新增文章的時候，可以使用 Markdown 進行編輯我的部落格
- 完整的 User Story 
    - 我在新增文章的頁面，輸入這篇文章的標題，並且以 Markdown 的格式輸入文章的內容，按下存檔，這篇文章內容就被儲存


> **初學的範本可以是：作為一個 (某個角色) 使用者，我可以做 (某個功能) 事情，如此可以有 (某個商業價值) 的好處**
1. 角色
    - 誰要使用這個功能
2. 活動
    - 需要完成什麼樣的功能
3. 商業價值
    - 為什麼需要這個功能，這個功能帶來什麼樣的價值

撰寫 User Story（使用者故事）好處是他也能變成是一個 Ticket。而 User Story 一方面可以幫助你之後把一些 Product spec （產品規格）留下來之外，令一方面可以幫助你在開發上更容易，開發進行時也可以隨時透過檢視 User Story 去檢視自己漏掉哪些部分。

在 Product Owner 列出 Item (物件)，也就是明確的羅列出待開發的項目，由 Scrum 團隊接手自行拆解 Task 進行開發。

> **針對 User Story ，建議額外加上了幾個訊息讓觀看者更清楚，包含**
- Story Title（顧名思義）
- Story ID（Story 專屬的 ID）
- Sprint Number（屬於第幾個Sprint）
-  Story Ref Number（代表這個 Story 其實是從某個 Story 功能繼續衍伸下來，但必須把功能進行更詳細拆分的時候，Story Ref Number 可以很快地知道前一步驟的 Story 是甚麼）
- Product Owner
- Story Point（後面有個步驟會進行評分)
- Story Version（目前更動最新是第幾版）
- Story Priority（優先順序高中低）
- 故事描述
- Acceptance Criteria（測試案例包含：前置條件、操作步驟、預期產出等等）

在 Story ID 的命名上，如果有個代號會更清楚，EX: S01 (代表跟Search 有關的功能)、SC01（與 Shopping Cart 購物車有關的功能）、Search-F-01（加上 F 表示這個是前臺的功能）等等，會讓大家在 User Story 觀看上更加輕鬆明朗。

另外，User Story 所描繪的功能不能太大或太小，應當要適度讓開發團隊在 Sprint 期間可以完成至少 3 個以上，否則團隊沒有成就感；最大值筆者是掌握在 7 個，不讓團隊認為功能做不完。


當開發團隊成員如果不是全心全意一周 40 小時在 Scrum 上面時候，筆者會額外加一個「預估可用時間」讓開發團隊成員填寫，如此在規劃 Item 時候，對應到小時可以縮放要完成的 Item（物件） 大小。

最後，Item 在每一次 Sprint Meeting 當中會做所謂評分的動作，記得Item所描繪的內容，時間上盡量至少要花團隊 1 天，最多不超過團隊 3 天為主，團隊以及 PO 可藉著每次的 Sprint 慢慢抓到精準度。


> **好像漏了什麼？前面提到 Spec（規格），但到底什麼是 Spec？它又包含什麼呢？**

### 1. Spec（規格）

基本上 Spec（規格）跟 Product Requirements Document （產品需求規劃書）可以某種程度視為同個概念的同義詞，舉個例子，軟體業常撰寫 PRD（Product Requirement Document），硬體業常寫 Spec 文件（Product specifications），每個領域有不同的說詞，但其屬性與功用其實大同小異。，PRD（Product Requirements Document 產品需求規劃書）這個文件類型，有時也叫做產品需求規劃書（Product/Feature Spec），更小型的功能修改則可能只是一張票（Ticket）。

PRD (Product Requirement Document)，中文全名叫做產品需求規格書。PRD 的主要目的是幫助公司在開發產品時需要的規格書，不論是新產品開發或是舊產品疊加新功能進行產品迭代，都需要撰寫 PRD。（PRD 不是絕對，很多公司甚至根本就沒在寫 PRD）

PRD 在本質上是一種溝通協定 (Protocal)，為一種統一 PM、UIUX、RD 間的溝通方式。寫 PRD 一定沒有口頭溝通快，但一定會比起口頭溝通來的準確。其實從上大小的文件有種類型，從 BRD（Business Requirement Document，商業需求規劃書），到 MRD（Market Requirement Document，市場需求規劃書），最後到 PRD（Product Requirement Document，產品需求規劃書）。

一言以敝之，「BRD」就是在討論現在做什麼產品比較好？然後決定有沒有價值？那要不要做？「MRD」就是討論假如競爭對手很多人都做它，那我們公司要做出哪些不同特色？「PRD」就是既然决定做這個產品了，這個產品要做成什麼樣子？更多內容茲分別說明如下：

#### I. BRD（商業需求規劃書）

BRD 在告訴利害關係人目前要進行的專案的商業價值與商業策略，短小精簡，沒有太多產品細節，主要溝通對象是高階管理者 C-Level（eg：CEO、CTO、COO）。跟他們說明為什麼要做這件事？商業價值是什麼？向 C-Level 闡明值得投入時間與金錢進行這項投資


#### II. MRD（市場需求規劃書）

MRD（Market Requirement Document，市場需求規劃書）指的是公司用來闡述新產品專案之市場需求的說明書，通常是由產品經理（或是產品行銷經理）來撰寫。簡單來說，MRD必須要能充分了解市場問題在哪裡以及顧客真正需要或想要的是什麼？

> **大體來說，MRD 至少須包含以下內容**
- Executive summary（摘要總結） 
- Vision、Mission、Core Values（願景、使命、核心價值） 
- Market research（市場研究） 
- Target Market（目標市場） 
- Competitive Positioning（競爭優勢及定位） 
- List of Feature（特色清單）

#### III. PRD（產品需求規劃書）

PRD（Product Requirement Document，產品需求規劃書）則是用來闡述公司目前在進行中的新產品或是現有產品需要增加新的功能需求之說明書，其目的是將產品需求描述清楚，方便工程設計及品保…等單位成員使用。

PRD 著重於產品實作方法與流程，涉及交互、文案、頁面邏輯等，具體將產品各個細節解釋清楚，向研發團隊、設計團隊、測試團隊、UX 團隊等說明產品規格。


> **大體來說，PRD 至少須包含以下內容**
- Product Overview（產品概述） 
- Flow chart（產品流程圖） 
- Use Cases（Functional Requirements，功能需求）
- Usability Requirements（可用性需求）
- Technical Requirements（技術需求） 
- Environmental Requirements（環境需求）
- Criteria for acceptance（產品驗收標準）

> PRD/文件概況
修訂紀錄與文件概況：其中包含日期、時間、專案負責人、平台版本、目前產品版號、利害關係人等。

> PRD/產品簡介與功能(Product Value)/產品簡介與目標說明
主要在集中在產品功能的描述；如產品簡介 (Overview)、產品價值論述 (Value)、目標客群 (Target User)、用戶故事 (User Story)、功能條列 (Feature)、開發排程 (develop phase)、版本號 (Release Version)、進版時間 (Release Time)。

> PRD/產品簡介與功能(Product Value)/開發排程 (Priority)
將功能盤點出來，區分工作排程的先後順序，如 Phase0、1、2、3。在P0部分則為該產品的 MVP，先把最小可運行的產品功能定好，在依照開發資源切分等。

> PRD/產品架構與流程/功能心智圖 (Function Map)
功能心智圖為產品功能模塊的集合，其顆粒度粗細必須RD對於該功能需求了解到何處，以及未來產品開發的路線圖。如「會員資料」功能模塊中的「編輯會員資料、上傳會員頭像」或是更特別的「註銷會員帳號」子功能都可以寫上去。

很多 UIUX 入門者可能都寫過這份 Map，你可以用 Xmind 寫，也可以用 Miro 寫，不管使用哪種心智圖工具，只要所有人能了解「現在」與「未來」要做的功能，都是好工具。

> PRD/產品架構與流程/產品資料結構圖 (IA)

IA 圖主要是幫助設計師思考功能內的資料結構與相關定義，如果有更細節的定義要寫出來，這樣的表單可以幫助後端工程師在前期就開好 DB 儲存欄位。如果以「手機登入註冊」功能為例，要去設法列出所有可能的問題並具體陳述解決方案：

- 手機最多可以幾碼？
- 台灣來說是否要 0 開頭？
- 手機可以輸入英文嗎？
- 最多可以多長？
- 獲取驗證碼重複按後，是否可以重發？
- 如果按下獲取驗證碼打一個 API 錯了怎麼辦？要回覆什麼錯誤訊息？要顯示在哪？怎麼顯示？
- 同意的條款點了會去哪裡？開一個新分頁嗎？還是在這個頁面跳訊息？若是跳到新頁面要怎麼回來？
- 沒有填資料按登入，是否要顯示錯誤訊息？
- 若是資料都填了按登入，但是驗證碼錯誤要顯示什麼錯誤訊息？顯示在哪裡？
- 聯繫客服點了以後，要發生什麼事情？

寫得越清楚詳細完善越好！因為所有能想到的詭異小問題都千千萬萬使用者會遇到的大問題。

> PRD/產品架構與流程/全局、局部功能邏輯圖 (Flow Chart)
撰寫邏輯圖，不論全局或是局部功能的。例如產品會因為登入前後的關係會加多不同的功能需清楚寫下來。

> PRD/圖形化流程 (flow) 與原型 (prototye)
不管是高低保真原型、線框圖流程 (Wireframe & flow)、精細圖流程(mockup & flow)。不管用任何工具做，只要呈現產品單一功能流程的樣貌都可以歸類在這邊。

> PRD/產品指標 (Metrics)
在產品指標的部分，當每一個功能釋出時，您都會先想一下該功能需要達成的「商業目標」為何？然後根據 Goal -> Signal -> Metric 的思考方式（由大到小，也可以說是樹幹、樹枝、樹葉的思路），去考該功能的評量方式，之後在與 RD 人員進行事件 (Event) 埋點。

> PRD/字串表 (Strings Table)
若稿件進入開發，則定 key 與不同的 value，該規則視不同公司會有差異。舉例來說，可以是每個 APP 在開發實需將字串填入表格，最後在某個時間點進版時間點，需將不同 APP 的字串表收集到字串管理軟件 Poedit 中納管。

> PRD/附件
可能會將不同平台的切圖鏈結與相關 APP 的工作時間表放在這邊，讓專案關係人進行查閱。

> 參考資料
1. [什麼是 User Story?](https://ihower.tw/blog/archives/2090)
2. [什麼是用戶故事（User Story）?](https://www.scrumcn.com/agile/scrum/4823.html)
3. [Day_02 user story？](https://ithelp.ithome.com.tw/articles/10235116)
4. [【Day 3】功能準備作業：User Story](https://ithelp.ithome.com.tw/articles/10186586)
5. [產品經理一定要掌握的 PRD/SPEC 心法與撰寫教學](https://www.inside.com.tw/article/19541-how-to-communicate-as-a-product-manager)
6. [【SOP不藏私】系列＃EP1「連猴子也會的PRD指南」](https://medium.com/as-a-product-designer/sop-ep01-prd-3c6d33880c34)
7. [深入淺出「敏捷軟體開發：Scrum」(上)](https://b98606021.medium.com/%E6%B7%B1%E5%85%A5%E6%B7%BA%E5%87%BA-%E6%95%8F%E6%8D%B7%E8%BB%9F%E9%AB%94%E9%96%8B%E7%99%BC-scrum-4a9d357ac0a4)
8. [如何撰寫新產品開發相關文件？MRD與PRD是甚麼？](https://www.pmtone.com/mrd-vs-prd/)
9. [Scrum 是什麼（2）：Scrum 的內涵](http://teddy-chen-tw.blogspot.com/2011/12/scrum-2.html)
10. [PRD 到底該怎  麼寫？更全面的文檔範例來了](http://www.woshipm.com/pmd/3327770.html)