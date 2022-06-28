---
title: TypeScript 原理與快速建置
date: 2022-06-28T04:58:00.154Z
author: Boison
slug: typeScript-101
tags:
  - TypeScript
draft: false
---
## 一、TypeScript 是甚麼

TypeScript (而後簡稱為 TS) 是一個基於 Javascript 的超集合 (superset) 開源語言，可以編譯成純 JS  且可以運行在任何瀏覽器、任何伺服器或任何系統上。

JS 是一個語言的集合，而 TypeScript 完全基於 JS 的語法，有點類似「擴充板的 JS 」，多了額外擴充如型別系統與註記（Annotation）等。

- - -

## 二、TypeScript 解決甚麼問題

TypeScript 改善了過去 JS 弱型別語言的缺點，透過 TypeScript 擴充的型別系統和編譯器檢查，讓開發者在開發過程做更多的約束，撰寫更嚴謹、更少錯誤和重複、更好管理的程式碼，從而大大減少實際運行程式碼時的錯誤。

- - -

## 三、TypeScript 怎用

### 1. 基本用法

> **Type Inference (型別推斷)**

* 若是沒使用 Type Annotation (型別註記) 在宣告變數時定義型別，TS 會代勞

> **Type Annotation (型別註記)**

* 在宣告變數時手動指定型別，告訴編譯器這個資料必須永遠都是這個型別
* 大部分情況下會使用型別註解 ; 型別斷言使用情境較少
* 型別註解大部分使用在初始化階段，像是宣告變數、函式參數或回傳值型別等

> **Type Assertions (型別斷言)**

* 在賦值時根據需求覆蓋/修改一開始的型別推論
* 型別斷言可能用在接收外部參數，中間過程需要明確指定資料型別的時候

#### I. Type Annotation (型別註記)

```javascript
// 變數的型別註解
let name: string = 'Hello Peter' 

// 函式參數/回傳值 的型別註解
function sayHello(person: string): string {
    return 'Hello, ' + person
}
```

#### II. Type Assertions (型別斷言)

```javascript
// 兩種寫法，第一種是<型別>值 (angle-bracket <>)寫法
let code: any = 123
let employeeCode = <number> code

// 兩種寫法，第二種則是值 as 型別 (as keyword)寫法
// 開發 React 專案使用 JSX 語法時只能用第二種
let code: any = 123
let employeeCode = code as number

// 例子1
let obj = {}
obj.age = 18 // error: property 'age' does not exist on `{}`
obj.name = 'iris' // error: property 'name' does not exist on `{}`

/// 使用介面（Interfaces）來定義物件的型別
interface Foo {
  age: number
  name: string
}

/// 語法1: 值 as 型別
const obj2 = {} as Foo
obj2.age = 18
obj2.name = "iris"

const obj3 = {
  age: 18,
  name: "iris"
} as Foo

/// 語法2: <型別>值
const obj4 = <Foo>{
  age: 18,
  name: "iris"
};

console.log("assertions-as", obj2) //{ age: 18, name: 'iris' }
console.log("assertions-as", obj3) //{ age: 18, name: 'iris' }
console.log("assertions-<type>", obj4) //{ age: 18, name: 'iris' }

// 例子2
function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length
    } else {
        return something.toString().length
    }
}
```

- - -

## 四、TypeScript  可用型別

## 1. 基本型別(Basic Types)

* **原始型別（ Primitive Types）**

  * number、string、boolean、undefined、null、
  * ES6: symbol
* **物件型別 Object Types**： 

  * Function、Array、Object
  * ES6: Class、Class 去 new 出的物件實例 instance 
* **void**

  * 表示不回傳任何值(等於預設回傳 undefined)

## 2. TypeScript 擴充型別

### I. 列舉(Enum)

* 用來表示被限定在一定範圍集合的系列元素
* 列舉又分三種型別

  1. 數字列舉(Number enum)
  2. 字串列舉(String enum)
  3. 異構列舉(Heterogeneous enum)
* 例如一週 7 天 (限定週一至週日)，紅綠燈 (限定顏色紅、黃、綠)

```javascript
// 數字列舉(Number enum)
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}

// 字串列舉(String enum)
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}

// 異構列舉(Heterogeneous enum)
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = 3,
  Right ,
}

// 常數列舉(const enum)，如果希望所有元素都是常數
const enum Direction {
  Up = 4,
  Down ,
  Left ,
  Right ,
}

let directions = Direction.Up

// 外部列舉(Ambient enum），declare 在 TS 主要用來做聲明，表示此物件已經存在
declare enum Directions {
    Up = 1,  // 常數
    Down, // 計算值
    Left ,
    Right
}

let directions = Directions.Up
```

### II. 元組(Tuple)

* 用來表示一個已知元素數量和型別的陣列

```javascript
let arr:[number, boolean, string] = [3.14, true, "hello"]
// 當訪問一個已知索引的元素，會得到正確的型別

arr[2].substring(1, 4) // ell
arr[1].substring(1, 4) // 報錯，布林值沒有 substring 方法
```

### III. 介面(Interface)

* 用來約束 Class 的行為，只描述屬性 (Property) 和方法 (Method)
* 可以想成是抽象的「形容詞」，但要注意的是 Interface 不像 Class 一樣可以實例化
* 介面的概念像是訂定契約，使用此契約的物件或類別就一定會符合契約中所規定的規範
* 但是介面只會定義描述有哪些方法 (Method) 和屬性 (Property) 無法實作

```javascript
// 介面宣告
interface Phone {
    phoneType: string,
    readonly model: string, // 介面宣告，唯讀屬性(Readonly properties)
    price?: number,         // 介面宣告，可選屬性 (Optional Properties)    
    [x: string]: any        // 介面宣告，任意屬性
}

let myPhone: Phone = {
    phoneType: 'iphone',
    model: 'iphone 11', // 之後無法再修改
    width: 100
}
```

### IV. 明文型別(Literal Type)

* 一個值也可以成為一個型別
* 比如字串 "Kira" 若成為某變數的型別的話
* 在此型別底下只能存剛好等於 "Kira" 字串值，不只字串，數字、物件等其他型別也可以
* 明文型別(Literal Type) 分成幾種

  1. 字串字面值(String Literal Types)
  2. 數字字面值(Numeric Literal)
  3. 列舉字面值(Enum literal types)
  4. 布林值字面值(Boolean literal types)

```javascript
// 字串字面值(String Literal Types)，用來限定字串變數只能使用列舉的字串值
let foo: 'Hello'
foo = 'Bar' // Error:Type '"Bar"' is not assignable to type '"Hello"'.
foo = 'Hello' // OK
foo = null // OK
foo = undefined // OK

// 數字字面值(Numeric Literal Types)，可以限制變數的值為特定範圍的數字
let zeroOrOne: 0 | 1
zeroOrOne = 0 // OK
zeroOrOne = 1 // OK
zeroOrOne = 2 // Error: Type '2' is not assignable to type '0 | 1'

// 布林字面值(Boolean Literal Types)
const TRUE: true  = true
const FALSE: false = false
const TRUE: true = false  // Error: Type 'false' is not assignable to type 'true'
const FALSE: false = true // Error: Type 'true' is not assignable to type 'false'

// 列舉字面值(Enum Literal Types)
const enum HttpPort {
  Http = 80,
  Https = 443
}

function getScheme(port: HttpPort.Http): "http"
function getScheme(port: HttpPort.Https): "https"
function getScheme(port: HttpPort): "http" | "https" {
  switch (port) {
    case HttpPort.Http:
      return "http"
    case HttpPort.Https:
      return "https"
  }
}

const scheme = getScheme(HttpPort.Http)
```

### V. 特殊型別

* any、never（TS 2.0）

  * any 可以兼容所有的型別

    * 遇到 any 型別，TS 就會跳過檢查系統不會進行型別檢查

    * 函式沒有回傳值或return 表達式回傳的值之型別為 never

  * never 型別很常使用在處理函式的錯誤情況

    * 所有型別中都包含 never，never 是所有型別的子型別

* unknown （TS 3.0）

  * 可以看成是 any 型別的安全版本

  * unknown 和 any 一樣可以接受任何型別賦值

  * 但若賦值 unknown 型別給其他型別，除了 unknown 和 any 其他型別都會報錯

  * 最大的差異在於預設情況下`允許操作屬性或方法`變成`禁止操作屬性或方法`

  * unknown 型別要進行限縮才能使用屬性或方法

    1. 型別檢測 (type guard)

    2. 型別斷言 (Type Assertions) 

### VI. 複合型別

* 即聯合型別 (Union Types) 與交集型別 (Intersection Types) 的型別組合
* 這類型的型別由邏輯運算子組成，分別是 | 與 &

```javascript
// 聯合型別用在資料可以是多種型別中的一種
let sign : string | number

sign = 0     // OK
sign = 'red' // OK
sign = true  // Error:Type 'true' is not assignable to type 'string | number'


// 交集型別主要用來將多個型別合併成一個型別
type Person = {
    name: string
}

type Contact = {
    phone: string
}

function showPersonContact(personContact: Person & Contact): void {
    console.log(personContact)
}

let personContact: Person & Contact = {name: "Dane", phone: "111-111-111"}
showPersonContact(personContact) //{name: "Dane", phone: "111-111-111"}
```

### VII. 通用型別(Generic Types)

* 指在定義函式、介面或類別時，不預先指定具體的型別，而是在使用時再指定型別的特性

  1. 通用函式
  2. 通用介面(Generics Interface)
  3. 通用類別(Generics Types)
* 可以達到讓傳入參數和回傳值型別相同 T => T，且傳入型別可以是任何型別

  * 在函式後面加上`<T>`
  * 其中 T 用來指代任意輸入的型別，之後就可以使用 T 作為回傳值同型別的指代
  * 倘若參數是字串型別，則回傳值亦為字串型別 
  * 倘若參數是布林型別，則回傳值亦為布林型別

```javascript
// 通用函式，執行函式時可以只傳函式參數(型別參數 TS 會自動做型別推論)
function foo<T>(arg: T): T {
  return arg
}

/// 傳遞型別參數
foo<number>
/// 傳遞函式參數(自動判斷型別參數)
foo(1)
/// 傳遞函式參數及型別參數
foo<number>(1)

/// 通用型別函式也可以傳入多個參數
function exchange<T, U, V>(tuple: [T, U, V]): [U, T, V] {
    return [tuple[1], tuple[0], tuple[2]]
}

exchange([18, 'hello',true]) /// ['hello', 18, true]
/// 若型別重複也沒關係
exchange([18, 'hello','world']) /// ['hello', 18, 'world']


// 通用介面(Generics Interface)
interface foodie{
    length: number
}

function foo<T extends foodie>(arg: T):T{
    console.log(arg.length)
    return arg
}


// 通用類別(Generics Types)
class Log <T>{
   run(value: T) {
      console.log(value)
      return value
   }
}

let log1 = new Log() // 可以不限縮型別
log1.run(1) // 1
let log2 = new Log<string>() // 也可以限縮型別
log2.run("2") // 2
```

- - -

## 四、TypeScript 在 React 專案快速建制

1. CLI 安裝

   * 法1: npm init @vitejs/app my-react-app --template react-ts
   * 法2: npx create-react-app my-react-app --typescript
2. cd my-react-app
3. npm install 
4. npm run dev

> 參考資料
>
> 1. [Typescript 初心者手札](https://ithelp.ithome.com.tw/users/20120053/ironman/2273)
> 2. [前端是該來學一下 TypeScript 了](https://ithelp.ithome.com.tw/users/20131472/ironman/4100)
> 3. [TypeScript 筆記：推斷、註記與斷言](https://simonallen.coderbridge.io/2021/08/06/ts-inference-annotations-assertiong/)
> 4. [TypeScript學習筆記（六）- 型別推論&型別斷言](https://www.gushiciku.cn/pl/gfUW/zh-tw)
> 5. [TypeScript 基礎入門：從型別談起](https://hackmd.io/@Heidi-Liu/typescript#%E5%9E%8B%E5%88%A5%E6%96%B7%E8%A8%80-Type-Assertion)
> 6. [TypeScript 入門：型別系統初探](./https%3A%2F%2Flinwei5316.medium.com%2Ftypescript-%E5%85%A5%E9%96%80-%E5%9E%8B%E5%88%A5%E7%B3%BB%E7%B5%B1%E5%88%9D%E6%8E%A2-%E8%A8%BB%E8%25A%20d69b8f50-9339-47c2-b442-b61fb25a13a8.md "!https\://linwei5316.medium.com/typescript-%E5%85%A5%E9%96%80-%E5%9E%8B%E5%88%A5%E7%B3%BB%E7%B5%B1%E5%88%9D%E6%8E%A2-%E8%A8%BB%E8%A8%98-%E6%8E%A8%E8%AB%96-%E6%96%B7%E8%A8%80-20cd07829eea")
> 7. [TypeScript: 元組型別（tuple）和列舉型別（enum）](https://www.796t.com/article.php?id=220354)