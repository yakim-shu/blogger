---
title: "[第三週] JavaScript - npm 套件管理工具"
layout: post
description: "除了剛剛示範過的 Node.js 提供的 Module、自己寫好的 Module，也可以使用別人寫好的套件，那要怎麼去使用呢？則要透過 npm 來幫我們下載跟管理這些套件。"
category: project
image: https://i.imgur.com/vk8fV6t.png
tags: [JavaSciprt_JS102, npm, project_week3]
file_name: 2019-04-29-project_w3_Javasciprt_npm
---
( image from [bleepingComputer](https://www.bleepingcomputer.com/news/security/compromised-javascript-package-caught-stealing-npm-credentials/) )

## Module 模組 or 模塊
模組 Module 的作用是，把各種功能（ 登入、金流、留言板 ）拆開來，才不會讓彼此相依性太高，產生不小心改到其他項目的悲劇。

### 引入 Module

- 以 Node.js 提供的其中一種 Module : `os` 為例，如果想查詢電腦作業系統，可以調用`.platform()` 方法。
- 連結：[官方文件](https://nodejs.org/api/os.html)

```javascript
var checkOs = require('os');
console.log(checkOs.platform());
```
( 記得先用 `require` 呼叫 `os` 這個 Module )

參考資料：
- [如何做出一個好的 NodeJS 模組？](https://blog.techbridge.cc/2018/02/14/how-to-make-a-good-node-module/)

---

### 導出 Module

假如我今天寫了一些簡單的函式，**很多檔案都會用到這些函式**，那也可以拆開成一個 Module 集中管理、也不用重複造輪子，更能供自己或別人使用。

假設我寫了一個名為 `myModule.js` 的檔案，裡面有三個 function:  `sumOfNum` 、 `double()` 、 `triple()`，要幫我的三個函式包裝起來，只要**傳進去 `module.exports`** 就 OK 了，是不是超簡單！  

#### module.exports
- 為了示範不同寫法，以下範例刻意把函式 `triple()` 直接放在 `module.exports ` 裡面
- 而其實 `module.exports` 傳什麼都可以，比較常見的應該是**物件**，但如果只是要傳一個陣列、變數、字串... 也可以的。
- **註解內也有標示導出的另一種寫法**，不同的是，這種寫法是直接把 `exports` 當作物件傳入

```javascript
function sumOfNum(x, y) {
    return x + y;
}
function double(x) {
    return x * 2;
}

module.exports = {
    sum: sumOfNum,
    double: double,
    triple: function (x) {
        return x * 3;
    }
}
/* 第二種寫法，把 exports 當作是一個物件傳入
exports.double = double;
exports.triple = function (x) {
    return x * 3;
}
*/
```

#### 引用剛剛導出的 Module

其實就跟剛剛用 `require` 引用的方式一樣，但注意 `myModule.js` 是自己寫的檔案，所以記得加上**檔案路徑位置 `./`**。  
（ 副檔名 `.js` 要加不加都可以，優秀的 `require` 都找得到！ ）

```javascript
var test = require('./myModule');

console.log(test.double(2)); // 4
console.log(test.triple(3)); // 9
console.log(test.sum(5, 10)); // 15
```

---

## npm ( Node Package Manager )

除了剛剛示範過的 `Node.js` 提供的 Module、自己寫好的 Module，也可以使用別人寫好的套件，那要怎麼去使用呢？則要透過 npm 來幫我們下載跟管理這些套件。

> NPM 是 Node Package Manager 的簡稱，它是一個線上套件庫，可以下載各式各樣的 Javascript 套件來使用。

### 初始化

要開始使用 npm ，先將資料夾進行初始化： `npm init` 。

接著會跳出訊息問你專案的資訊，不想理就全部按 `Enter` 跳過就好，而完成後會發現多出個 `package.json` 的檔案，紀錄專案資訊跟**套件資訊**。

### package.json

（ 剛初始化過的 `package.json` 可能會長這樣 ）

```json
{
  "name": "week3",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    // 記錄套件資訊
  }
}
```
而 `"dependencies"` 那欄位還是空的，因為我們還沒有下載任何 npm package。

參考資料：
- [從零開始: 使用NPM套件](https://medium.com/html-test/從零開始-使用npm套件-317beefdf182)
- [Day5 - 關於 module.exports 的兩三事](https://ithelp.ithome.com.tw/articles/10185083)

---

### 下載 Package

假設我要引入 `left-pad` 這個套件。（ 幫字串左邊補空格或任意字元 ）

- 先連到 left-pad 的 [官方連結](https://www.npmjs.com/package/left-pad)，上面可以看到使用說明及版本號...等等資訊。
- 在專案目錄底下打入： `npm install left-pad`

此時會有兩個變動：
- 專案目錄會新增一個 `node_modules` 的資料夾，而裡面也有剛剛下載的 package: `left-pad` 資料夾
- `package.json` 裡的 `"dependencies"` 多出了套件資訊

```json
{
  "name": "week3",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "left-pad": "^1.3.0" // 剛剛安裝的 left-pad
  }
}
```

### package.json 的妙用

那這邊記錄下來的用處是什麼？
- 第一，可以很清楚的知道此專案依賴在哪些套件上
- 第二，`node_modules` 裡的套件可能會越來越多、專案變得肥大，更新 git 要花一段時間，而因為這些套件網路上都載的到，其實可以不要上傳，**那就可以把 `.gitignore` 資訊加上 `node_modules`**，忽略肥大的 `node_modules` 資料夾裡的所有檔案。

而當我們要 `clone` 別人專案時，**輸入 `npm install`，系統就會直接看 `package.json` 裡面記錄了哪些專案，一次全部下載回來**，超方便！

> 注意：

在新的版本 npm 5 以前，要輸入 `npm install <pacakge-name> --save`，才會輸入到 `package.json` 檔案裡面喔！

### 引用 package

而至於怎麼引用進來呢？步驟其實跟剛剛一樣，也是使用 `require(<package-name>)`

```javascript
var test = require('left-pad');
console.log(test('abc', 8, '0')); // 輸出 00000abc
```

( **可以不用寫路徑名**，優秀的 `require` 會自動去 `node_modules` 資料夾搜尋有沒有匹配的檔名。 )

---

### npm Script

npm 也有提供自己寫一些簡單的腳本供執行，最常見的可能是，當我們要 run 一個新專案，可能裡面有不同檔案，而接手的人不知道哪一個才是主程式，所以我們可以把**運行主程式** 這動作記錄在 `script` 裡面。

- 打開 `package.json` ，可以看到裡面有個 `script 欄位`，新增一個 `start` 
- 接著試試運行 ： `npm run start`，就會發現跑了 `node index.js`
- 可以自行發揮創意，在 `script` 欄位加上要做的指令，運行輸入： `npm run <script key>`

```json
{
  "name": "week3",
  "version": "1.0.0",
  "description": "",
  "main": "myModule.js",
  "scripts": {
    "start": "node index.js", // 新增的 start 指令
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "left-pad": "^1.3.0"
  }
}
```

( 所以如果之後主程式改成 `index_2.js`，只要改成 `"start": "node index_2.js"`，接手的人同樣是運行 `npm run start`，而不用管到底主程式是哪一個檔案。 )

---

## yarn （ 另一種套件管理工具 ）

`yarn` 是由 Facebook 開發的另一種套件管理工具。

功能、指令跟 `npm` 都非常類似，但 `yarn` 的速度比較快，差別在安裝套件的指令是： `yarn add <package-name>`。
