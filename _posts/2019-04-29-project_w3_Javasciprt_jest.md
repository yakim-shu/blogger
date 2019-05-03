---
title: "[第三週] JavaScript - 測試、Jest、TDD"
layout: post
description: "畢竟程式碼是照你「寫」的跑、不是照你「想」的跑，所以需要寫測試。"
category: project
image:
tags: [JavaSciprt_JS102, jest, test, TDD project_week3]
file_name: 2019-04-29-project_w3_Javasciprt_jest
---
## 測試的重要性

當我們寫好一段程式碼，想知道結果正確與否，應該說**想確認是否為我們預期的結果**。

> 畢竟，程式碼是照你「寫」的跑、不是照你「想」的跑

所以需要寫測試，而最簡單的測試方法就是把結果印出來： `console.log()`。

#### 最陽春的測試 - 印出來
假設我要寫一個重複字串的函式 `repeat()`，有 `str` 跟 `times` 兩個參數。 那就看 `log` 出來的結果跟我們預期的是否相同。而且最好多放一些**極端情況**的測試資料，例如：
- `''`  ( 空字串 ) 重複 `10` 次
-  `abc` 重複 `0` 次

```javascript
function repeat(str, times) {
    var result = '';
    for (var i=0; i<times; i++) {
        result += str;
    }
    return result;
}

console.log(repeat('abc', 3)); //abcabcabc
console.log(repeat('', 10)); // (空字串)
console.log(repeat('abc', 0)); // (空字串)
```

---

## Jest 

因為剛剛的測試方法沒辦法自動化、也會**將測試檔跟主程式混在一起，當結構越來越大時並不符合使用**。

> 所以我們引入 Jest ，這個由 Facebook 開發的 JavaScript 測試框架。

### 用 Jest 改寫剛剛的測試

1. 先用 `npm` or `yarn` 下載套件：
    - `npm install jest --save-dev`
2. 打開 `package.json` 可以看到 `jest` 已經被記錄在 `devDependencies` 欄位裡
3. 修改 `script` 欄位的 `test` 指令為 `jest`： 
    - `"test": "jest"`
4. 設定好後，只需要在 Terminal 輸入： `npm run test`

```json
// package.json
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^24.7.1"
  }
```

> 注意：

如果直接在 Terminal 輸入 `jest` 指令是不會有反應的喔，因為他是被安裝在專案目錄下，而 Terminal 內的 `jest` 指令需要安裝在**全域**下才能運行。

---

### 開始測試

`Jest` 進行測試時，**會先找專案中副檔名為 `.test.js` 的檔案**。

- 跑當**前資料夾底下的所有的測試檔**： `npm run test`
    - 其實也就等於 `npx jest`
- 跑**單一測試檔**： `npm run test <file-name>.test.js`
    - 其實也就等於 `npx jest <file-name>.test.js`

這邊建立一個 `repeat.test.js` 來測試剛剛的函式 `repeat()`。

所以我們會有兩個檔案：
- `repeat.js` 被測試的檔案 ( Unit, Module, 主程式... )
- `repeat.test.js` 拿來測試 `repeat.js` 的測試檔

#### repeat.js （ 主程式 or Module）
``` javascript
function repeat(str, times) {
    var result = '';
    for (var i=0; i<times; i++) {
        result += str;
    }
    return result;
}
module.exports = repeat; // 匯出函式 repeat()
```

#### repeat.test.js  （ 測試檔 ）
``` javascript
var repeat = require('./repeat'); // 引入函式 repeat()

describe("測試 function - repeat()", () => {

    test('"abc" 重複 3 次', () => {
        expect(repeat('abc', 3)).toBe('abcabcabc');
    });
    test('<空字串> 重複 10 次', () => {
        expect(repeat('', 10)).toBe('');
    });
    test('"abc" 重複 0 次', () => {
        expect(repeat('abc', 0)).toBe('');
    });
});
```

#### Jest api 名詞解析

- describe
    - 將相關測試案例整合成一個測試結果，通常拿來描述這些測試是屬於哪個元件或 function。
    -  ( 對照下圖 Test Suites )
- test
    - 定義一個最小的測試案例 
    -  ( 對照下圖 Test )
- 斷言
    - **expect**: 用來判斷是否和預期值相同的斷言庫。
    - **toBe**: 比較兩物件是否有相同的值，常用來比較數值。
    - ...其實還有超多種斷言，有興趣可以上官網查。
    
> 白話文就是： `expect(<裡面回傳的結果>).toBe(<應該要跟這裡相等>);`

###  測試結果

![螢幕快照 2019-04-29 下午4.06.21](https://i.imgur.com/Cqn7tgL.jpg)


參考資料：
- [用jest+enzyme來寫Reactjs的單元測試吧！](https://github.com/Hsueh-Jen/blog/issues/1) ( 寫得好清楚！ )
- [讓 Jest 為你的 Code 做測試-基礎用法教學](https://medium.com/enjoy-life-enjoy-coding/讓-jest-為你的-code-做單元測試-基礎用法教學-d898f11d9a23)


---

## TDD 測試驅動開發

全名為 Test-Driven-Development，是一種**開發流程**，指的是**在實作功能之前，先寫測試程式。**

這樣有以下好處：
- 當開發者當寫出測試時，就可以瞭解這一個元件最後會怎麼使用，同時也釐清的程式的介面會如何定義。
- 因為是先寫測試，所以也不會有趕時間上線，而忘了寫測試的情況
- 每次實作功能時，只需要剛剛好通過一個新測試即可，不多也不少，不會過度設計

看到 [[隨筆] 開發人員對 TDD 的心魔](https://dotblogs.com.tw/hatelove/2017/06/01/the-test-word-real-meaning-of-tdd) 裡的一段話，讓我對 TDD 有更清楚的了解、何謂測試不止於測試，以下我直接引用原文：

---
#### 測試不是測試？

- 如果是先有需求規格再寫 production code 呢？
- 如果是為了先想好自己的目的跟驗證方式，再寫 production code 呢？
- 如果是先想清楚，使用者、使用端到底要怎麼用我接下來提供的功能或產品，才會比較好用？

這一些都不是「測試」（CHECKING）在做的事，反而比較像是**需求描述、目標捕捉、建立邊界、迭代演進**，只把力氣精準地花在目標上而不浪費。我們只是「恰巧」用了模樣長得像是「測試程式」的方式來進行上面這幾件事罷了。

---

參考資料：
- [程式設計師升級必練內功：TDD Kata](https://tw.alphacamp.co/blog/2015-03-02-tdd-kata)
- [自動軟體測試、TDD 與 BDD](https://medium.com/@yurenju/自動軟體測試-tdd-與-bdd-464519672ac5)
- [[隨筆] 開發人員對 TDD 的心魔](https://dotblogs.com.tw/hatelove/2017/06/01/the-test-word-real-meaning-of-tdd)