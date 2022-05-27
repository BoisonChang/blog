---
title: "百日轉職前端工程師：第十三週現代前端工具復盤 《DAY 20》"
slug: Transfer-FrontEnd-100-20
date: 2020-11-17T21:20:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "Promise"]
keywords: ["FrontEnd", "Transfer", "Promise"]
author: "Boison"

---

大家好，這是百日轉職前端工程師的 Day20，也是 11/17（二），這週的主題會講的是一些現代前端工具諸如 SaSS、Babel、Gulp、Webpack，能夠幫助工程師更方便的開發以及與團隊協作，這些工具的精神也被運用到現代框架中，因此了解這些工具的本身，也能夠在未來對於學習前端框架有一個更全面的瞭解，本文也會帶到在發送 request 常用的函式 promise 和 fetch。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、WEBPACK 功用和原理是什麼？如何用來模組化開發？

- Webpack 是一套模組整合工具（Webpack is a module bundler.）
- 可將零散的 JavaScript 模組打包，然後在瀏覽器上運行，解決舊瀏覽器不支援部分新語法的問題，也利於後續管理與維護。

---

## 二、如何使用 Promise？

### 1. Promise 要解決什麼問題？

> **Promise 是要用來解決非同步的多個 Callback function 造成的 callback hell**

簡單來說，Promise 是一個包含很多函式的物件，而 Promise 裡的 callback function 都是非同步，Promise 的誕生是要用來解決非同步的多個 Callback function 造成的 callback hell 問題，也就是 Callback function 內部又包覆著很多個一層包一層的 Callback function，其會造成程式易讀性降低，也由於高度的複雜性和連動性導致接手原本專案的人難以更動修改程式碼，但且讓我們先熟悉 Promise 的特性，要真正解決 callback hell 的問題，還要搭配上我們待會要聊的另一個 fetch 函式 。

### 2. Promise 物件的特性？

- Promise 的使用很間單，其內建兩個函式 resolve 和 reject
- 可以用其中的 then 來接收回傳的結果（resolve）
- 可以用其中的 catch 來接收回傳的錯誤（reject）
- 所以要偵錯時只要塞入一個 .catch，若是有錯誤則 .catch 底下的 .then 就不會繼續被執行

```
promise
.then(result => result + '!');
.then(result2 => result2 + '?'
.catch(()=>console.log('error!'))
.then(result3 => { console.log(result3 + '!')
})
```

### 3. Promise 如何使用？

> **先宣告一個 Promise 的 callback function 然後再做使用**

```
function sleep() {
const myPromise = new Promise(resolve => {
setTimeout(resolve, 3000)
})
return myPromise;
}

sleep()
.then((data) => { console.log('myPromise Data', data); })
.catch(err => { console.log('err', err);
})
```

> **> .sleep 可簡化成**

```
function sleep(ms) {
return new Promise(
resolve => { setTimeout(resolve, ms) })
}
```

> **>> .sleep 再簡化**

```
const sleep = ms => {
return new Promise(
resolve => { setTimeout(resolve, ms) })
}
```

> **>>> .sleep 再簡化**

```
const sleep = ms => new Promise(
resolve => {
setTimeout(resolve, ms)
})
```

> **>>>> .sleep 再簡化**

```
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms))
```

---

## 三、如何使用 fetch？

### 1. fetch 要解決什麼問題？

> **fetch 是 ES6 的新語法，主要是搭配 Promise （Promise 的基本用法）來執行請求網站和請求後獲取 Response 的處理方式**

在 JavaScript 中，網路請求（Request）其實主要分為兩種，在古代的時候只有 XMLHttpRequest （jQuery 中有名的 ajax 就是針對此做封裝），以及後來的 fetch （後來的標準，但也參考了 ajax 的做法），使用 fetch 的好處是可以避開 Callback hell 的問題。

### 2. fetch 函式的特性？

> **而利用 fetch() 這個 ajax 方法來發出 request，會回傳是一個 promise 的物件。裡面的內容如下**

1. arrayBuffer()
2. blob()
3. formData()
4. json()
5. text()

> **我們真正想要知道的是其中 text() 的內容，因此用 .then 將其取出如下**

```
const result = fetch('https://www.example_api.php')
.then((res)=> {
console.log(res.text());
})
```

> **然而印出的內容還是一個 Promise 物件，因此我們只好不死心的再用 .then 一次如下，總算拿到資料了。**

```
const result = fetch('https://www.example_api.php')
.then((res)=> {
res.text().then (text => {
console.log(text);
});
})
```

### 3. fetch 函式的 Chaining 特性

> **原本寫法是**

```
const result = fetch('https://www.example_api.php') .then((res)=> {
res.json().then (text => {
console.log(text);
});
})
```

因為 .then 裡面回傳的還是一個 promise 物件，我們可以一直使用 .then 來對回傳的 promise 做處理。

簡單來說，.then return 的值會是下一個 .then 的值，而這個巧妙的特性設計也剛好為我們解決了層層包覆的 Callback hell 問題，運用 fetch 特性後的寫法，有效減少層數。

```
const result = fetch('https://www.example_api.php') .then(res => {
return res.json() })
.then ( value => {
console.log(value)
})
```

### 4. Fetch 使用時的注意事項

- content type：注意 content type 是什麼，看 API 要接收的是什麼格式資料
- credential：發 request 給不同來源 domain 的 API 時，加上 credentials: 'includes'，才會把 cookie 帶上去

```
const data = { hello: 'world' }
const result = fetch(api400, {
method: 'POST',
body: JSON.stringify(data),
credentials: 'includes',
headers: new Headers({ 'Content-Type': 'application/json' })
}) .then() .then() .catch()
```

- mode: 'no-cors' : 不能突破 CORS 限制，只是當發生時請瀏覽器回傳一個空的 response 不報錯。
- Async / Await：用看起像是同步的語法，做到非同步的事情。用 async 宣告一個非同步的 function，await 接一個 Promise 物件，會等到執行完 await 裡面的 promise 才往下執行，await 是逐一執行的概念。

```
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms))
async function() {
console.log('hello');
await sleep(1000);
console.log('world');
}
```

> 參考資料

1. [從 ES6 開始的 JavaScript 學習生活](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/ajax_fetch.html)
2. [JavaScript 的同步與非同步 - 從 Callback function 到 Promise](https://nicolakacha.coderbridge.io/2020/09/11/sync-async/)
3. [一起來把 setTimeout 封裝成 Promise 吧！](https://realdennis.medium.com/callback-hell-%E8%88%87-promise-%E4%B8%80%E8%B5%B7%E4%BE%86%E6%8A%8A-settimeout-%E5%B0%81%E8%A3%9D%E6%88%90-promise-%E5%90%A7-e542ef84967f)
4. [JavaScript Fetch API 使用教學（含 Fetch 詳細參數）](https://www.oxxostudio.tw/articles/201908/js-fetch.html)

---

## 四、gulp 的目的以及原理？

- gulp 是一個前端自動化構建工具（Task Mananger）
- 開發者可使用它來建構自動化工作流程
- 通常在比較小的網站才會看到 gulp
- 如果在大型專案網站用到前端框架，任務又更多而且方向不太一樣，比較會用 webpack

---

## 五、使用 gulp 建構自動化工作流程

> **各種常用的 gulp plugin 可以幫助我們建構自動化工作流程(在 src 和 dest 之間加入)**

1. gulp-babel
2. gulp-uglify
3. gulp-rename
4. gulp-sass
5. gulp-clean-css
6. gulp-imagemin

---

## 六、什麼是 uglify 與 minify？

> **在撰寫 CSS 程式碼時，有兩種方式可以讓 CSS 檔案變得更小**

- uglify 是將其變數名稱代換，並且重新編碼，盡量的簡化但也會讓程式碼變得不同
- minify 則只是將程式碼去除所有不必要的空格，把它轉成越短越好，變成一整串連續不換行的程式碼

基本上這兩種壓縮都可以透過一些工具（Webpack, gulp......的 Plugin）去完成。

---

## 七、CSS Sprites 與 Data URI 的優缺點

> **CSS Sprite 是什麼？**

- CSS Sprite 技術將網站中使用的許多較少更動的小圖匯集成一張大圖稱為 Sprite 又稱為雪碧圖或者精靈圖等
- 透過這個處理可以節省網頁請求數量，加快網頁載入速度
- 比較常見的工具是使用 SpriteSmith 來合併圖片，並產生合併圖片後每個小圖的對應座標
- SpriteSmith 可以跟許多建構工具結合使用，像是 Grunt Gulp 以及今天要介紹的 Sprite-Smith-Plugin

> **CSS Sprite 的優點**

1. 減少圖片的體積，加快速度
2. 減少網頁的 HTTP Request 次數，加快速度
3. 解決了每週圖片命名上困擾，加快開發效率
4. 更換全部圖片風格方便，加快維護

> **CSS Sprite 的缺點**

- 如開發上要注意合併格式之類較麻煩之處，因此通常會搭配 SpriteSmith 等輔助工具使用，且用在較少更換的小圖如 logo 等上面

> **如何使用 Sprite-Smith-Plugin**

1. CLI 先安裝 Plugin
   - npm i webpack-spritesmith -D
2. webpack.common.js 設定檔調整如下

```
var path = require('path')
var SpritesmithPlugin = require('webpack-spritesmith')
module.exports = {
plugins: [
new SpritesmithPlugin({
// 定義來源圖片資料夾，可以透過glob定義那些圖片要被合併 ex: .png src: { cwd: path.join(__dirname, 'Resource/Images/sprite'), glob: '',},
// 合併完成後，圖片及scss的儲存路徑
target: {
image: path.join(__dirname, 'Resource/Images/sprite.png'),
css: path.join(__dirname, 'Resource/Sass/_sprite.scss'),},
// 可以設定一些圖片參數
spritesmithOptions: {
padding: 10,},
// 自動產生的 scss 要使用的 sprite 圖路徑
apiOptions: {
cssImageRef: '../Images/sprite.png',},
})]}
```

3. 設定好之後，將原本網站上面的 ICON 放在步驟 2 的 src 來源資料夾
4. 執行 webpack 打包：npm start

※ Data URI

> **Data URI 是什麼？**

- 除了 Sprite，Data URI 是另外一種減少 HTTP 的請求（Request）數量的方式
- Data URI 是一種檔案格式，其資料全經過 base64 編碼後以文字的方式儲存
- 以文字儲存的好處是可以直接寫進 HTML 或 CSS 中，不需要透過外部的檔案儲存
- 其效果不錯，但有資料顯示在手機上的效果較差

> **Data URI 缺點**

- 無法存快取
  - 使用 Data URI，所有的資料都是在網頁中（圖片和網頁沒有分開），無法使用快取，每次開網頁都要重新渲染一次
- 原始碼讀取不易
  - base64 編碼過後的資料很長，原始碼讀取不易，搜尋速度也較慢，但可用 SASS partial 的方式，將 Data URI 與一般的網頁分開，使用變數的方式引入解決。
- Data URI 是編碼後存在 HTML 或 CSS 中的，若每次資料要變更時要重新編碼，但可使用 SASS 與 Compass 來處理解決

> **將資料編碼的方式**

1. 線上編碼工具：使用線上編碼工具如 dataurl.net 等
2. 使用 PHP 內建的 base64 編碼函數 base64_encode()

```
<?php
function create_data_uri($source_file, $mime_type) {
$encoded_string = base64_encode(file_get_contents($source_file));
echo ('data:' . $mime_type . ';base64,' . $encoded_string);
} ?>
```

3. 使用 Compass 與 SASS
   - Compass 提供了一個很有用的函數 inline-image，如果你有使用 Compass 的話，就可以直接在 SASS 檔中使用這個函數：body { background: inline-image("../images/logo.png") }。因為這樣的方式是直接指向一個外部的檔案，所以如果該圖檔需要更動， Compass 也會自動將新的檔案編譯成 CSS
4. 使用 Ruby And Rails
   - 引入函式庫 base64，使用函式 create_data_uri()

> 參考資料: [使用 Sprite-Smith-Plugin 產生 CSS Sprite](https://ithelp.ithome.com.tw/articles/10201033)

---

## 八、CSS 優化的一些小技巧

### 1. 資源大小優化

- Minify
  - 把所有空格都去除掉
- gzip\*
  - 壓縮成不同編碼，需要解碼瀏覽器才能讀懂

### 2. 載入方式優化

- CSS Sprites
  - 把所有小的東西（如圖片）打包起來成一份，零零散散的 Request 整合起來的
- Critical CSS
  - 把一大包東西分批載入，很大的話可以把一些重要的部分先載入
- Cache\*
  - 當你今天不是第一次進入這個網站，有些東西可以先被瀏覽器暫存起來，就不用再重新要一次資源

### 3. 執行方式優化

- 選擇器
  - Selector 能否寫得更漂亮，來減少瀏覽器在語法解析上的時間
- 屬性渲染
  - 你選擇的屬性對於瀏覽器來說是否不需要涉及比較早期的渲染機制
  - 舉例用 CSS 去切換 JavaScript，是從排版期就開始重新去 render 還是在合成的階段再重新去 render

---

## 九、CSS Selector 的權重

> **假設 CSS 對同一個區塊有不同的風格，要以何者為準？又如何計算權重呢？**

- CSS 屬性是會繼承到底下的，但越底下的屬性優先次序越高。
- CSS 同一階層越後面寫的優先。
- !important > inline style > id > class > tag
- 若是同階層數量（權重）一樣，則往下一階層比！
- !Important 實務上很少會去用。

---

## 十、結論

這週內容比較多，考驗自行上網找資料的能力，也要自行融會貫通，

關於 Scss / Sass 和 babel 內容請參考 Sass 入門，和 babel 入門，前者是對 CSS 做預處理讓其在編譯上可以更加結構化，後者則是讓新版本的 ES6 功能也能轉換後在舊版的瀏覽器上正確的運行，這週就這樣啦！
