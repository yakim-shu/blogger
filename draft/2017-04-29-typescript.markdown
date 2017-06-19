---
layout: post
title: "【心得】TypeScript新手入門班"
---

密碼： skilltree Typescript

# TypeScript新手入門班

## 問題

- 一次只能 watch 一隻 ts 檔嗎？

---

## 環境

- command shift p > task config
- command shift p > watch Mode
- command shift b > build

### source map 記得打開

- tsconfig : "sourceMap": true

### 模擬sever
- install : npm install -g http-server
- operate: http-server .

### chrome debugger

- 安裝 vscode :debugger for chrome
- command shift p > launch.json
- 新增組態：

```
{
    "type": "chrome",
    "request": "launch",
    "name": "Launch Chrome",
    "url": "http://localhost:8080",
    "webRoot": "${workspaceRoot}"
}
```


## ts-node 
### 用 termianl 裡測試 typescript

安裝：npm install -g ts-node
執行：ts-node app.ts

---

## 型別

### typescrpit 特有型別

- Enum [TypeScript列舉型別](http://blog.darkthread.net/post-2014-08-20-typescript-enum.aspx)
- Any
    - 自由型別，跟javascript用var一樣，可以用 tsconfig 設定禁止用any

#### 型別預設

-沒寫型別
    - 有給初始值
        + 自動偵測
    - 沒給初始值
        + any
- vs code > 滑上去會顯示型別

### 陣列

- 同樣可以用 any

### Enum

- 沒指定數字，會從0開始
- 回去試試： 轉換為文字

```javascript
var size: Size = Size.Large
var sizeString: string = Size[size];
Console.log(sizeString); /// Large
```

---

## 變數

- var (function scope)
- let (block scope)
    + let不會有hoisting的問題
- const (常數)
    + 宣告const的時候，一定要指定初值
    + const也不會有hoisting的問題。

### let

- 建議：沒有要共用檔案，建議使用 let
- 限定範圍 (以大括號為準)
- 原理：幫你重新命名一個新變數名

### const

- 盡量少用 Magic Number，改用有意義的變數或常數

---


### 多載

- 同一個函式，不同參數使用
- 注意：只有最後一個參數可以用
- spread property: 傳入多個參數

### 匿名函式

- 不需要再特別做閉包：

```
var self = this;
```

---

### Switch

- switch 適合搭配 Enum

### for of

- 比 for in 好用
- for of 是遍歷 屬性值(property values)
- for in 是遍歷 屬性名(property names)
- 可以直接取出值，而非 index

---

### 定義 json

網路上有[轉換interface](http://json2ts.com/)

```
interface Product {
    id: number;
    xxxxx
}

let data = Product {
    id:ddd,
    xxxx
}
```

---

## 物件導向

## Class

### Porperty Private

- 內部使用，ex: 計算...
- 只是提醒，還是拿得到


### Property static

- 共用，不需初始化

> 搞懂 property 跟 constructor 



### constructor

- 在 constructor 用 public 屬性，可以直接設定屬性及建構式
```
class Student { 
    constructor(
      public name: string,
      public age: number = 18) {
  }
}
```


### interface

- 切換新程式碼可以用，尤其是改版
- 名稱前面加 i
- 搭配工廠模式

> 搞懂 interface


### Module

- 記得加 export (有點像 public)

> 練習 Demo
> 用原本的方法＆物件導向方法


- 通常是一段程式碼不斷擴充時，再來重構成 interface...
(關注點分離)

---

## 進階物件導向

---

## 非同步

### Promise ES6

- IE 不支援
- 參考 Promise 的文件寫法

Lab06 以後輸入 npm install


### Async & Await 必須成對出現


## 定義檔

vs code 不用載入 reference


---

## 泛型

- 跟any差別在於，可以檢查是不是array
- 會有程式碼提示

> 方法簽張





