---
title: "百日轉職前端工程師：第十三週 Webpack, Gulp 《DAY 19》"
slug: Transfer-FrontEnd-100-19
date: 2020-11-14T15:52:21+08:00
draft: false
tags: ["FrontEnd", "Transfer", "Webpack", "Gulp"]
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

大家好，這是百日轉職前端工程師的 Day19，也是 11/14（六），這週的主題會講的是一些現代前端工具 Webpack 和 Gulp。

Webpack 功能非常強大，能夠將各種功能作為模組打包成一個可引入運行在瀏覽器上的檔案，由於篇幅眾多獨立為一篇，是現代開發上一定會用到的工具之一。而 Gulp 則是可以將工作的流程自動化，本篇也會帶你搞懂這兩者的差別。

復盤系列將會回答我正在上的課程 Huli 的程式導師實驗計畫每一週學習上的自我檢測目標。

---

## 一、webpack 的目的以及原理

- Webpack 是一套模組整合工具（Webpack is a module bundler.）
- 可將零散的 JavaScript 模組打包，然後在瀏覽器上運行，解決舊瀏覽器不支援部分新語法的問題，也利於後續管理與維護

> **Webpack 打包流程**
1. index.js, utils.js, templates.js → 經過 webpack 打包 → main.js
2. index.html 用 src 引入 main.js 模組


> **在專案資料夾內安裝 webpack**
1. mkdir webpack-demo
2. cd webpack-demo
3. npm init -y
4. npm install webpack webpack-cli --save-dev

通常開發者會把原始的檔案放在 src 資料夾，編譯過後的檔案放在 dist 資料夾，以方便管理。執行 webpack 就可以進行編譯，預設會把 src 裡的檔案打包到 dist 內成名為 main.js 的檔案。

> **也可以在根目錄建立設定檔 webpack.config.js 來自定義路徑和要打包的模式，模式有兩種**
-  production 的壓縮程度較高，檔案會變比較小，類似 uglify 過後的一串 JavaScript
    - mode: 'production',
- development 的可讀性則相較高，但檔案較大：
    - mode: 'development'


> **webpack.config.js 檔案的格式**

```
const path = require('path'); 
module.exports = { 
mode: 'production', 
entry: './src/index.js', 
output: { 
filename: 'main.js', 
path: path.resolve(__dirname, 'dist'), 
}, 
};
```
> ***在 package.json 建立 script，之後  npm run build 就可以執行 webpack**

```
"scripts": { 
"build": "webpack", 
"test": "echo \"Error: no test specified\" && exit 1" },
```

---

## 二、使用 webpack 進行模組化開發

- 除了打包 JavaScript 之外，webpack 把 modules 的概念延伸至更廣的範圍，各種資源都可以視為一個 module
- 所以我們也可以利用 webpack 幫我們打包圖片或 CSS
- 透過各種 loader 把資源載入 webpack，也可以順便做到 uglify JavaScript 或是 minify CSS 之類的編譯

> **以下會介紹一些常見的 loader 的使用，打包 npm 上別人第三方的 modules**

### 1. 打包 npm 上別人第三方的 modules

> **透過 CSS loader 打包 CSS 檔案**
1. css-loader：打包 CSS，在 webpack.config.js 加入 loader
```
module.exports = { 
module: { 
rules: [ 
{ test: /\.css$/i, 
use: ['style-loader', 'css-loader'], 
}, ], }, };
```
2. CLI 安裝 style.loader 和 css.loader
    - npm install --save-dev css-loader style-loader
3. 在 index.js 引入 style.css 一併打包為 main.js
    - import css from './style.css';


> **透過 babel-loader 執行 babel**
1.  CLI 安裝 babel-loader
    - npm install -D babel-loader @babel/core @babel/preset-env 
2. 在 webpack.config.js 設定檔加入 babel-loader

```
rules: [ { 
test: /\.m?js$/, 
exclude: /(node_modules|bower_components)/, 
use: { 
loader: 'babel-loader', 
options: { 
presets: ['@babel/preset-env'] 
} } }
```

> **透過 sass-loader 執行 sass**
1. CLI 安裝 sass-loader
    - npm install sass-loader sass webpack --save-dev 
2. 在 webpack.config.js 設定檔加入 sass-loader

```
rules: [ { 
test: /\.s[ac]ss$/i, 
use: [
'style-loader', 
'css-loader',
'sass-loader', 
], }, ],
```


### 2. 利用 Dev Server 自動化打包

dev server 是一套可以在檔案改變的時候「自動」重新打包的 plugin，不用更動程式碼每次都要手動在 CLI 輸入 wbpack / npm run build 太麻煩。

1. CLI 安裝 dev server
    - npm install sass-loader sass webpack --save-dev
2. 在 webpack.config.js 設定檔加入以下，定義編譯完的檔案放在哪裡
    - devServer: { contentBase: './dist', },
3. 在 package.json 加入 start 指令，之後 CLI 輸入 npm run start，就會進入到  dev server start 的狀態
    - "scripts": { "start":  "webpack-dev-server --open", }


### 3. 使用 source map 看到原來程式碼的樣子

在 webpack.config.js 加入 source-map 程式碼，在 debug 的時候，就可以在 chrome dev tool 溯源到真正的程式碼。

```
module.exports = { 
mode: 'development', 
entry: { 
app: './src/index.js', 
}, 
devtool: 'inline-source-map', 
output: { 
filename: '[name].bundle.js', 
path: path.resolve(__dirname, 'dist'), 
}, };
```

### 4. 使用 HtmlWebpackPlugin 自動產生打包後的 html 

> **在 webpack.config.js 加入 HtmlWebpackPlugin，自動產生打包後的 html 在 index.html 檔案內**


```
const HtmlWebpackPlugin = require('html-webpack-plugin'); 
const path = require('path'); 
module.exports = { 
entry: 'index.js', 
output: { 
path: path.resolve(__dirname, './dist'), 
filename: 'index_bundle.js' 
}, 
plugins: [new HtmlWebpackPlugin()] 
};

```

> 參考資料:
1. [Webpack 打包工具入門](https://nicolakacha.coderbridge.io/2020/09/09/webpack/)
2. [Webpack 官方網站文件](https://www.webpackjs.com/)

---

## 三、 gulp 的目的以及原理？

- gulp 是一個前端自動化構建工具（Task Mananger）
- 開發者可使用它來建構自動化工作流程
- 但通常在比較小的網站才會看到 gulp
- 如果在大型專案網站用到前端框架，任務又更多而且方向不太一樣，比較會用 webpack

### 1. 為什麼需要 gulp？

- 為了將工作流程自動化，也就是整合前端開發環境
- 藉由簡化工作量，可讓開發者將重點放在功能的開發上

> **例如下列功能**
1. 讓網頁自動重新整理
2. 編譯 SASS 、程式碼檢測
3. 壓縮 CSS, JS, 圖檔
4. 自訂任務流程…


### 2. 如何安裝 gulp？

1. 先用 npm 安裝  gulp-cli：npm install --global gulp-cli
2. 開啟使用 gulp 的專案資料夾
3. 進行初始化：npm init
4. 安裝 gulp package：npm install --save-dev gulp
5. 使用指令檢查是否安裝成功：gulp --version


### 3. gulp 提供的 API

- gulp.task
    - 執行工作
- gulp.src
    - 執行資料來源
- gulp.dest
    - 執行結果位置
- gulp.watch
    - 監視執行過程中，資料是否變更

使用 gulp 內建的 API watch()，在檔案每次被改動的時候自動執行 gulp 工作流程，例如我們可以在 SCSS 檔案被更改的時候，自動執行 complieCSS 這個 task，就不需要每次都要手動執行流程。

但要注意的是，使用 watch() 的 task 不能是同步的，如果是同步的任務，將無法確認 task 的完成情況，task 將不會再次運行。

> **gulp API 使用實例**

```
const { src, dest, series, parallel, watch } = require('gulp'); 
const babel = require('gulp-babel'); 
const uglify = require('gulp-uglify'); 
const sass = require('gulp-sass'); 
sass.compiler = require('node-sass'); 
const cleanCSS = require('gulp-clean-css'); 
const rename = require('gulp-rename'); 

function compileJS() { 
return src('src/*.js') 
.pipe(babel()) 
.pipe(uglify()) 
.pipe(rename({ extname: '.min.js' })) 
.pipe(dest('dist')); 
} 

function compileCSS() { 
return src('src/*.scss') 
.pipe(sass().on('error', sass.logError)) 
.pipe(dest('./css')) 
.pipe(cleanCSS({ compatibility: 'ie8' })) 
.pipe(rename({ extname: '.min.css' })) 
.pipe(dest('./css')) 
} 

exports.default = function() { 
watch('src/*', series(compileCSS, compileJS)); 
}
```

---

## 四、利用 gulp 提供的常用 plugin 建構自動化工作流程

各種常用的 gulp plugin，在 src 和 dest 之間加入這些 plugin，可以幫助我們建構自動化工作流程。

### 1. gulp-babel

> **就是在 gulp 任務管理器內執行 babel 轉換語法的 plugin**

```
const { src, dest } = require('gulp'); 
const babel = require('gulp-babel'); 

function defaultTask() { 
return src('src/test.js') 
.pipe(babel({ presets: ['@babel/env']})   // .babelrc 第一次用要放參數 
).pipe(dest('dist')); 
} 

exports.default = defaultTask;
```

### 2. gulp-uglify

> **gulp-uglify 針對 JS 檔案進行壓縮與最小化，檔案會被壓縮成一行變數會被簡化，可以減少檔案大小**

```
const { src, dest } = require('gulp');
const babel = require('gulp-babel'); 
const uglify = require('gulp-uglify'); 

function defaultTask() {
return src('src/*.js') .pipe(babel()) 
.pipe(uglify()) 
.pipe(dest('dist')); 
}
```

### 3. gulp-rename

> **經過 uglify 的 JavaScript 檔案，通常會命名成 .min.js 以做為區別，所以可以再引入 rename 套件**

```
const { src, dest } = require('gulp');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');

function defaultTask() {
return src('src/*.js') .pipe(babel())
.pipe(uglify())
.pipe(rename({extname: '.min.js'}))
.pipe(dest('dist'));
}
```

### 4. gulp-sass

> **gulp-sass 套件可將 sass 轉換成 css**

```
const { src, dest } = require('gulp');
const sass = require('gulp-sass');
sass.compiler = require('node-sass');

function defaultTask() {
return src('src/*.scss')
.pipe(sass().on('error', sass.logError))
.pipe(dest('./css'));
}

exports.default = defaultTask;
```

### 5. gulp-clean-css


> **gulp-clean-css 類似 minify js 對 JS 在做的事，可以把 css 檔案壓縮**

將 clean-css 加入流程，設定 sass 的編譯器，同時也加入 rename 這個 plugin，讓編譯完的 css 重新命名為 min.css。做兩次 dest()，編譯完 sass 先複製一次，變成 min.css 後再複製一次，這樣資料夾內就同時有被壓縮前和被壓縮後的檔案。

```
const { src, dest } = require('gulp');
const sass = require('gulp-sass');
sass.compiler = require('node-sass');
const cleanCSS = require('gulp-clean-css');
const rename = require('gulp-rename');

function compileCSS() {
return src('src/*.scss')
.pipe(sass().on('error', sass.logError))
.pipe(dest('./css'))
.pipe(cleanCSS({ compatibility: 'ie8' }))
.pipe(rename({ extname: '.min.css' }))
.pipe(dest('./css'))
}
```

### 6. gulp-imagemin

> **gulp-imagemin 可將圖片檔案壓縮變小**

```
const { src, dest, series, parallel, watch } = require('gulp');
const imgMin = require('gulp-imagemin');

function minifyImg() {
return src('src/img/*')
.pipe(imgMin())
.pipe(dest('lib/img'))
}

exports.default= minifyImg;
```

---

## 五、gulp 引入 series / parallel 執行任務

- 引入 series：按順序執行 compileCSS 和 compileJS 兩個任務
    - exports.default = series(compileCSS, compileJS);
- 引入 parallel：同時執行 compileCSS 和 compileJS 兩個任務
    - exports.default = parallel(compileCSS, compileJS);
- 執行單個任務
    - exports.compileCSS = compileCSS

---

## 六、Webpack 和 Gulp 的差別？

> **Webpack**
- 是一套模組整合工具（module bundler）
- 目的：利用模組化的概念，將各種資源打包成能在瀏覽器上執行的程式碼
- 功能：將各種資源視為一種模組引入打包成 main.js

> **gulp**
- 是一套任務管理工具（task manager）
- 目的：提供自動化與流程管理，整合前端開發環境，藉由簡化工作量，可讓開發者將重點放在功能的開發上。
- 功能：提供自訂任務流程，例如 babel、scss、壓縮、重新整理、校正時間等。


> **兩者目標並不相同，為截然不同的工具，但是恰巧均能夠達到部分功能如以下，因此容易被混淆**
- 使用 Babel 將 ES6 編譯成 ES5 語法
- 將 SASS 檔編譯成 CSS 檔
- 壓縮 CSS, JS, 圖檔等

--- 
## 七、總結

>  『想說的話一杯酒說不盡，那不如就打包外帶吧。』