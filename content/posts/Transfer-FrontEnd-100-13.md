---
title: "百日轉職前端工程師：第六週前端基礎 CSS《DAY 13》"
slug: Transfer-FrontEnd-100-13
date: 2020-09-03T20:55:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "CSS"]
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

大家好，這是百日轉職前端工程師的 Day13，也是 9/03（四）。今天開始要來復盤之前學過練習過前端基礎下，沒想到上一篇光是 HTML 就有那麼長的篇幅，很多東西都是寫的當下才去把資料整理齊全，花了很大心力，因為既然要寫就希望至少寫的架構是相對完整可擴充的，這篇繼續聊聊 CSS。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、CSS 是什麼？

> **CSS 全名為 CASCADING STYLE SHEET（階層樣式表）**

- 跟 HTML 一樣，CSS 既非標準程式語言，也不是標記語言，而是一種風格頁面語言
- 用來調控網頁各個區塊/元素的樣式，諸如字體、大小、顏色、背景色
- 讓我們在撰寫網頁時能夠結構更清晰的拆分出主要的內文結構跟網站的樣式

> **當你的標籤上附帶屬性時**

- 就能夠在 CSS 頁面用代碼操控這些標籤包覆的區塊在網頁中你希望他呈現的樣式
- CSS 跟 HTML 兩者的交叉使用我們俗稱為「切版」，在網頁的畫布上切出設計師圖稿上想要的樣式

舉個例子，我給了這個叫做 board\_\_btn（留言板的按鈕）的 class 各種樣式的設定，來讓它長成我心目中網頁裡按鈕該有的樣子：

```
.board__btn {
  text-decoration: none;
  color: black;
  background: white;
  font-size: 20px;
  border: solid 2px #000000;
  padding: 10px;
  display: inline-block;
}
```

---

## 二、inline、block 跟 inline-block 的區別？

> **CSS display 屬性是讓網頁設計師可以自由設定網頁元素的顯示類型**

- HTML 每一種不同的元素都會有自己的預設值，常見的有「區塊元素」與「內行元素」
- 預設為 display:block 的為「區塊元素」
- 而預設為 display:inline 的則是「內行元素」

> **不同元素的預設值**

1. 預設 inline 類型的元素
  - span, a, input, img, em...
  - 其不能調整寬和高、上下邊距也不能調
  - 多個元素也是會並排，不會在寬度未滿的狀況下自動換行
2. 預設 block 類型的元素
  - div、p、ul、li...
  - 可以設定高度（height）、寬度（width）
  - 元素寬度預設會撐到最大，使其占滿整個容器
3. 預設 inline-block 類型的元素
  - buttom, input, select...
  - 對外像 display: inline 可以並排，元素之間彼此不會自動「換行」
  - 對內像 display: block 可以調整各種屬性。（可水平排列）

> **補充：長度單位有三種**

1. px: 像素，圖像的基本單位是像素 px（沒有比像素更精細的單位了）
2. em: 以母元素的基礎字體大小作為倍數的單位（若母元素為 12px，則 1em = 12px、2em = 24px、.......）
3. rem: 以根元素的基礎字體大小作為倍數的單

---

## 三、史上最強排版助手：Flexible box （靈活的盒子）

> **Flexbox 是 CSS3 display 裡一個比較特殊的版面設計模式**

- 被設計出來的主要原因是提供更有效率的方式來完成傳統 CSS 無法輕易達成的效果
- 基本上我們要去理解 Flexbox 它就是一個外容器和內容器
- 很多人稱它為「靈活的盒子 ( Flexible Box )」
- 只要將外容器的 display 更改為 flex 或 inline-flex，能夠對其內部的元素使用 flex 屬性做些靈活的排版

### flex-direction

> **flex-direction 表示 Flexbox 內容元素的「排列方向」**

- row：預設值，由左到右，從上到下
- row-reverse：與 row 相反
- column：從上到下，再由左到右
- column-reverse：與 column 相反

### justify-content

> **justify-content  決定了盒內元素相對於 flex 外容器的對齊位置（水平）**

- flex-start：靠左對齊
- flex-end：靠右對齊
- center：水平置中
- space-between：平均分配內容元素，元素間有間隔
- space-around：平均分配內容元素，元素間和外容器間皆有間隔，後者為前者一半
- space-evenly：平均分配內容元素，元素間和外容器間皆有相同間隔

### align-items

> **justify-content  決定了盒內元素相對於 flex 外容器的對齊位置（垂直）**

- flex-start：向上對齊
- flex-end：向下對齊
- center：垂直置中
- stretch：預設值，將內容元素全部撐開至 Flexbox 的高度
- baseline：以所有內容元素的基線作為對齊標準（基線你就試試看反正我感覺預設大概是中間偏下）

### align-self

> **可以對已經使用過 align-items 的內元素再做調整**

### align-content

> **基本上遇到多行元素時使用，整體與 align-items  大同小異**

- flex-start：向上對齊
- flex-end：向下對齊
- center：垂直置中
- space-between：將第一行對齊最上方，最後一行對齊最下方
- space-around：每行平均分配間距
- stretch：預設值，內容元素全部撐開至 Flexbox 的高度

### flex-wrap

> **可以用來讓原本撐滿整行的內元素以各種形式換行**

- nowrap：預設值，單行
- wrap：多行
- wrap-reverse：多行，但內容元素反轉

### order

> **直接用數字由小到大指定內元素的排列順序。（ex: order: 1;）**

### flex-

> **用來分配內元素如何延伸或者壓縮去佔據 Flexbox 的空間**

- flex-basis
  - 預設值為 0，可對已設置過寬度的內元素再單獨額外覆蓋設定
- flex-grow
  - 若是 Flex 盒寬度大於所有內元素寬度，可用此將內元素延伸，若都設定為 1，代表平均拉伸分配剩餘空間，若有三個內元素（1 : 1 : 1）
- flex-shrink
  - 若是 Flex 盒寬度小於所有內元素寬度，可用此將內元素壓縮，若都設定為 1，代表平均壓縮分配剩餘空間，若有三個內元素（1 : 1 : 1）

> 參考資料

1. [深入解析 CSS Flexbox](https://www.oxxostudio.tw/articles/201501/css-flexbox.html)
2. [圖解 Flexbox 基本屬性](https://cythilya.github.io/2017/04/04/flexbox-basics/)

---

## 四、什麼是 box model？

> **盒模型的解釋我認為有兩種**

1. 是用來描述網頁中像是盒子的這個區塊的內層（Padding），外框（Border），以及外框外面的一層與其他區塊的距離（Margin）
2. 打開網頁原始碼後，可以從網頁中找到盒模型的可視化區塊，會顯示每一點選區塊其盒模型的視覺化介面，讓你了解目前區塊內各「層」的大小

如果不希望設定 Padding 時，改變到整個盒子的大小，可以從 CSS 設定 box-sizing: border-box（預設是 content-box）。就會在保留原先 border 大小下加減 padding，建議一開始就在 CSS 最上方全部設定如下：

```
* {
  box-sizing: border-box;
}
```

`<p></p> <a></a> `等標籤常會有預設的 margin, padding 值，因此也建議可以一開始就在 CSS 最上方建議一開始就可以全部設定如下：

```
p, a {
  margin: 0;
  padding: 0;
}
```

---

## 五、position 的所有屬性及其差別

> **position 是用來定位元素的屬性**

- 它是指定 top、left、right、bottom 的數值，來決定元素的位置
- 它的定位是依照基準來位移，基準依 position 類型會不一樣

> **CSS 的 position 屬性有**

- static
  - 預設值，網頁預設的排版方式。會一個一個元素的繪製。（由左往右或者由上往下）
- relative
  - 相對定位：會針對「自己原本顯示的位置為基準位置」做位移，且區塊原本的空間仍會保留不會消失。
- absolute
  - 絕對定位：重新以「基準元素」（非 position: static ）為起點，可以自由指定配置位置。若無基準元素就會用 body 做為參考點
- fixed
  - 固定定位：相對於瀏覽器的定位，一開始的位置瀏覽器即使捲動還是固定在那不動。
- sticky
  - 是黏住的意思，會讓元素在畫面滾動到設定值的時候，就固定在那個位置。

---

## 六、偽類怎麼用（如 :hover, :nth-child ...等）？

偽類（ Pseudo-classes ）都會用 : 做前綴詞宣告，在網頁上，最常用的偽類應該就是 :hover，它能夠在滑鼠移到設定了 :hover 的 Element 時，讓 Element 改變原有的 CSS，因此常會看到點到網頁某區塊就變色，或者放大縮小都是運用此方法。

除了基本的 :hover 外，還有一個 :focus 也是偽類的一種，通常會用在 input 上面，讓使用者點選某輸入框輸入文字時明顯改變樣式，讓使用者知道目前正在此處進行輸入的動作。（雖然我是覺得有點多餘 XD）

> **nth-child() 意思基本上就是選取第 n 個子物件**

- 比方說你可以選取 `<body>` 中第 3 個元素中的標籤用 body p:nth-child(3) {}
- 也能夠一次選取所有奇數的標籤如 body p:nth-child(odd) {}
- 甚至也可以自己寫公式如 body p:nth-child(2n+1) {}

然而若是你的內文中夾雜不同的標籤 body p:nth-child(3) {} 就是不管三七二十一直接給你選第三個，若是你想要精確選出 `<body>` 中第三個 `<p>` 就需要用上 :nth-of-type() 如 body p:nth-of-type(3)。

> 參考資料：[CSS | 全都是假的！關於那些偽類和偽元素 - 基本用法](https://medium.com/enjoy-life-enjoy-coding/css-%E5%85%A8%E9%83%BD%E6%98%AF%E5%81%87%E7%9A%84-%E9%97%9C%E6%96%BC%E9%82%A3%E4%BA%9B%E5%81%BD%E9%A1%9E%E5%92%8C%E5%81%BD%E5%85%83%E7%B4%A0-%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95-4de48feb8607)

---

## 七、偽元素 ::before, ::after 的各種用法

- 使用「偽元素」的好處在於，可以讓程式碼看起來比較乾淨整齊，不會太亂
- 偽元素與偽類不同，為了區分它們，在設定時的前綴詞使用 ::
- 「偽元素」不像偽類只在特定條件下觸發，而是常態顯示在網頁畫面中
- ::before 、 ::after 這兩個偽元素能在原 Element 的前面或後面增加另一個 Element
- 以 display: inline-block 的屬性存在
- 一定要設定 content 的屬性，就算是只有 content: ""; 都可（沒有 content 的偽元素是不會出現在畫面上的）
- content 是個很特別的屬性，它可以使用 attr 直接獲取內容元素的屬性值 ( attribute )

---

## 八、CSS selector 靈活選取到你要的元素

如果你想要在 CSS 中選取某父元素底下的特定元素其實很簡單，比如你想選到 class="title" 的標籤中 所有的 `<div>`，只需 .title div {} 即可。

> 至於其他更進階的用法如下。

- 「>」選取器是「選到下一層的元素」
- 「+」、「~」選取器是「選到旁邊的元素」
- A + B ，如 h1 + p {}，選取到同一層中『 h1 右邊的第一個 p』
- A ~ B， h2 ~ p {} 選到同一層中 『 h2 右邊所有的 p』

要幫一排元素增加空白間距時這很好用：

```
.title div ~ div {
    margin-left: 10px;
}

```

---

## 九、CSS 中各種實用的屬性

- background
- border 與 border-radius
- margin 與 padding
- 文字相關：color、font-family、font-weight 與 line-height
- 文字相關 part2：word-break 與 white-space
- 你滿了，那我就漫出來了：overflow 與 text-overflow
- 加了我質感瞬間升級：transition
- Die Verwandlung：transform 的妙用

以上通通不贅述，請見 [ CSS/02 各種裝飾](https://tzutzu.coderbridge.io/2020/07/26/css-%E5%90%84%E7%A8%AE%E8%A3%9D%E9%A3%BE/) 一文

---

十、結論

還是老話一句。

`切版水很深，同志待努力。`
