---
title: "[第三週] JavaScript - 把你對 isNaN 的不滿都說出來"
layout: post
description: ""
category: project
tags: [NaN, isNaN, project_week3]
file_name: 2019-05-03-project_w3_Javasciprt_NaN
---

> `NaN == NaN` or `NaN === NaN` → 都是回傳 `false`

寫作業時遇到這問題，卡了許久，才發現有這種奇怪的現象，剛好當天就在同學的進度報告看到解法，感謝同學助手相救！

今天找時間來研究一下，原來要用 `isNaN()` or `Number.isNaN()` 才能判斷，但這當中有更多奇葩特性，真是看得我吐了滿地血。

- `NaN` : 全名為 Not a Number ，屬於全域物件，代表不是數字
    - 通常都是作為**回傳值**，一般不會去設定跟覆寫
    - 常見是在 Math 函式計算失敗 `Math.sqrt(-1)` 、或函式解析數字失敗`parseInt("blabla")` 後才會回傳。
- `.isNaN(x)` : 判斷 `x` 是不是 `NaN` 的函式
    - **會先將 `x` 轉換成數字類型 → `.isNaN(Number(x))`**，再進行比對是否為 `NaN`
    - 所以造成了許多光怪陸離的結果...
- `Number.isNaN(x)` : 同樣也是判斷 `x` 是不是 `NaN` 的函式
    - ES6 出的新語法，又比 `.isNaN()` 可靠又嚴格一點
    - 只有傳入 **型態為 `number` 的 `NaN` 才回傳 `true`**（ 意思是 `'NaN'` 不行喔！）
    - 其於皆為 `false`
    
#### `.isNaN(x)` 
以下範例來自 MDN ：
```javascript
isNaN(NaN);       // true
isNaN(undefined); // true
isNaN({});        // true

isNaN(true);      // false
isNaN(null);      // false
isNaN(37);        // false

// 字串
isNaN("37");      // false: "37" 轉換成數字的 37 後就不是 NaN 了
isNaN("37.37");   // false: "37.37" 轉換成數字的 37.37 後就不是 NaN 了
isNaN("123ABC");  // true:  parseInt("123ABC") 是 123 但 Number("123ABC") 是 NaN
isNaN("");        // false: 空字串轉換成數字的 0 後就不是 NaN 了
isNaN(" ");       // false: 有空白的字串轉換成數字的 0 後就不是 NaN 了

// 日期
isNaN(new Date());                // false
isNaN(new Date().toString());     // true

// 這個偵測的錯誤是不能完全信賴 isNaN 的理由
isNaN("blabla")   // true: "blabla" 被轉換為數字，將其解析為數字失敗後回傳了 NaN
```

#### `Number.isNaN(x)` 
以下範例來自 MDN ：
```javascript
// 只有這三種例子會回傳 true
Number.isNaN(NaN);        // true
Number.isNaN(Number.NaN); // true
Number.isNaN(0 / 0);      // true

// 底下會回傳 false，但用 .isNaN() 會回傳 ture
Number.isNaN('NaN');      // false
Number.isNaN(undefined);  // false
Number.isNaN({});         // false
Number.isNaN('blabla');   // false

// 以下會回傳 false
Number.isNaN(true);
Number.isNaN(null);
Number.isNaN(37);
Number.isNaN('37');
Number.isNaN('37.37');
Number.isNaN('');
Number.isNaN(' ');
```

> 讓我們搭著彼此的肩膀、同聲譴責：「 JavaScript ，你好靠杯 」

參考資料：
- [JavaScript 判斷變數是否等於 NaN](https://coder.tw/?p=7435)
- [MDN - NaN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/NaN)
- [MDN - isNaN()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/isNaN)（ 十分詳細！ ）
- [MDN - Number.isNaN()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)
