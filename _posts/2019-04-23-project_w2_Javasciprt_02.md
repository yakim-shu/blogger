---
title: "[第二週] 基礎 JavaScript - 02 變數"
layout: post
description: "變數的命名有一套規則是非常重要的，語意化的命名可以一看名稱、就知道它的用處是什麼，才不用看整個程式架構來猜，非常的浪費時間。"
category: project
image: 
tags: [JavaSciprt_JS101, project_week2]
file_name: 2019-04-23-project_w2_Javasciprt_02
---

## 變數的命名方式

變數的命名有一套規則是非常重要的，語意化的命名可以一看名稱、就知道它的用處是什麼，才不用看整個程式架構來猜，非常的浪費時間。

常見的命名方式有駝峰式、[匈牙利命名法](https://zh.wikipedia.org/wiki/匈牙利命名法)。

- 駝峰式命名法：
    - 單字間用大小寫分隔
    - 例如：`myFirstName`、`myLastName`
- 匈牙利命名法：
    - 前面加上資料型態
    - 例如：`intTotal`、`strName`

參考資料：
- [程式設計命名規則：駝峰命名法和匈牙利命名法](https://codertw.com/程式語言/555937/)
- [程式基礎概念─變數命名規則](https://ithelp.ithome.com.tw/articles/10108301)

---

## `++int` VS `int++`

這還蠻酷的，原來把 `++` 放前面或後面，執行的順序是不一樣的！

```javascript
var int1 = 0;
var int2 = 0;
console.log(int1++ && 30, "int1: " + int1); // 輸出 0, int1: 1
console.log(++int2 && 30, "int2: " + int2); // 輸出 30, int2: 1
```

兩個變數的輸出結果都是 `1`，這沒有問題。

但為什麼前面的邏輯比較不一樣勒？**因為 `int1++` 是會先跑完整句、才執行**。所以當 `int1++ && 30` 的時候、變數 `int1` 的數值其實還是 `0`，是等到比較完了 `int1` 才 `+1`。

---

## 變數類型

在 JavaScript 中，除了原始類型，其他一切都物件。可以使用 `typeof <Variable>` 查看變數類型。

- 原始型別：`string`, `int`, `boolean`, `undefined`, `null` 
    - 不過 `typeof null` 會出現 `object`，神秘的歷史問題
    - ES6 版本有新增一個原始型別 `Symbol`，暫不討論
- 物件型別：`object`, `array`, `function` ...


此外因為 JavaScript 非常的隨性，就算一開始定義好了 `變數1` 的類型，**後來還是可以再做更改**，當然沒事請不要這麼做就是了。

> 以下範例就是教你：「 如何造成別人跟自己的困擾 」：

```javascript
var intNum = 3;
intNum = "hahaha";
console.log(typeof intNum); // 會輸出 string，但此變數名稱會變得非常擾民
```

參考資料：
- [鐵人賽：動態型別的 JavaScript](https://wcc723.github.io/javascript/2017/12/08/javascript-typeof/)

---

## 陣列
通常是放同類型的資料，例如 `arrNum = [10, 20, 5, 7]`、`arrString = ["小明", "小花", "小黑"]`。

複習一下以前寫的陣列筆記：[【筆記】Javascript大全 - 05(一) 陣列](https://yakimhsu.com/javascript/JS_Daquan-05.html)

## 物件
由 `key: value` 組成，取 `value` 的方法有兩種：
- `object.key`
    - `key` 是寫死的，**無法用變數**
- `object[key]`
    - `key` 可以帶入變數，所以通常是帶變數時、才會使用 `中括號 [ ]`
    - `key` **只能是字串**，若不是字串則會被強制轉為字串。

看以下 3 種方法都會輸出 `小黑`，而也可以將 `object`、`array`、`function` 放入 `value` 裡面：

```javascript
var changeKey = "dog";
var oProfile = {
    name: "Yakim",
    dog: "小黑", 
    oFather : {
        name: "LingPiao",
        dog: "小花"
    }
}

console.log(oProfile.dog);  // 小黑
console.log(oProfile["dog"]); // 小黑
console.log(oProfile[changeKey]); // 小黑
console.log(oProfile.oFather.dog); // 小花 ← 放入物件
```

參考資料：
- [你懂 JavaScript 嗎？#17 物件（Object）](https://cythilya.github.io/2018/10/24/object/)

---

## 變數運算要注意陷阱！

### 陷阱一：注意型別

運算子 `+` 、 `-` 要特別注意型別。

例如：要是 `<string> + <num>`， 會先將 `num` 改成 `string`，再做字串相加： `<string> + <string>`，例如：

```javascript
'1' + 2 // 輸出 12 ( 變成 '1' + '2')
``` 

參考以前筆記：[【筆記】Javascript大全 - 02 運算式與運算子](https://yakimhsu.com/javascript/JS_Daquan-02.html)

#### 將 string 轉成 number 的方式有兩種：

1. `Number(<string>)`
2. `parseInt(<string>, 10)` ： 第 2 個參數為要轉換的進位制，例如 2, 10, 16

### 陷阱二：注意浮點數 float

```javascript
console.log(0.1+0.2); // 0.30000000000000004 
```

這是一個很重要的觀念，事實上不只 JavaScript，所有的程式語言都會有這個問題。

因為輸入的值本身就不精確，且電腦記憶體有限，運算結果也很有可能造成誤差，這點在 CS50 也有提到，就不再寫了：[【筆記】CS50 - week 1 ( 2019年更新 )](https://yakimhsu.com/cs50/CS50_week1.html) ( 文章最後面 )

但說實在，要搞懂為什麼還蠻困難的，根據 [JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9) 指出： 

( 此篇文章我只看得懂這段... ) 

首先要搞清楚 JavaScript 如何存储小数。和其它语言如 Java 和 Python 不同，JavaScript 中**所有数字包括整数和小数都只有一种类型 — Number**。它的实现遵循 IEEE 754 标准，使用 64 位固定长度来表示，也就是标准的 double 双精度浮点数（相关的还有float 32位单精度）。

```javascript
// 0.1 和 0.2 都转化成二进制后再进行运算
0.00011001100110011001100110011001100110011001100110011010 +
0.0011001100110011001100110011001100110011001100110011010 =
0.0100110011001100110011001100110011001100110011001100111

// 转成十进制正好是 0.30000000000000004
```

#### 解決方法

網路上有非常多文章再討論如何解決浮點數精度問題，看得我頭昏眼花，整合了一下，假設要取浮點數 `result`：

1. 不要用...
2. 引入數學庫 library
3. `parseFloat(result).toFixed(x)`： `x` 填入 `0~20`位數，一般不建議使用
4. `Math.round(result * 100000) / 100000;`： 先把數字變成更大的數值在去掉不想要的尾數變成整數後，再縮回原本位數
5. 使用加減乘除函式（有點複雜）：將浮點數轉為字串，把字串分割成整數位 & 小數位，進行完運算後，再將結果轉回浮點數，有興趣可以看：[JavaScript 浮点数运算的精度问题](https://www.html.cn/archives/7340)


參考資料：
- [JavaScript 浮點數計算的精確作法](https://pvencs.blogspot.com/2018/06/javascript.html)
- [Number​.prototype​.toFixed()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)

---

## `==` VS `===`

在之前的筆記裡 ：[【筆記】Javascript大全 - 02 運算式與運算子](https://yakimhsu.com/javascript/JS_Daquan-02.html) 也有提到在 `==` 比較式中，JavaScript 會先幫兩邊不同的變數類型、先進行類型轉換、再進行比較。

所以 `"1" == true // 回傳 true` 當中，在比較之前，會先執行以下兩步驟：
- string `"1"` 轉成 number `1`
- boolean `true` 轉成 number `1`

結論：為了避免對變數類型的疑慮，建議平常都使用嚴格相等 `===`。

--- 

## JavaScript 中的物件特性

非常重要的觀念，一定要釐清。

#### 記憶體位置

假設有兩個物件 `obj` 跟 `obj2`。
```javascript
var obj1 = {a: 1};
var obj2 = {a: 1};
console.log(obj1 === obj2); // false
```

為什麼上面的兩個看似內容一樣的 object 卻回傳 `false`？ 
因為在 JavaScript 中，**物件**是比較**記憶體位置**：
- 第一個 `obj` 會在電腦裡**新增**一個新的記憶體位置，假設是 `0x01`，裡面存放著 `{a: 1}`
- 第二個 `obj2` 會在電腦裡**再新增**一個新的記憶體位置，假設是 `0x05`，裡面存放著 `{a: 1}`
- 而 `obj === obj2` 比較的其實是 `0x01 === 0x05`，所以當然會回傳 `false` 啦！

#### 物件的更新

一樣是兩個物件 `obj` 跟 `obj2`，但此例中的 `obj2` 是指向 `obj`。
```javascript
var obj = {a: 1};
var obj2 = obj;
```

當**更改 `obj2.a` 的值**時，觀察以下 `obj` 的變化。

```javascript
obj2.a = 2;
console.log(obj); // 2
console.log(obj === obj2); // true，記憶體位置相同
```

因為 `obj2` 跟 `obj` 都指向同一個位置，而那裡存放的內容是 `{a: 1}`。
所以當 `obj2.a` 改成 `2`，因為**指向的位置是一樣的，所以 `obj.a` 也會一起更改**。

![螢幕快照 2019-04-23 下午8.35.29](https://i.imgur.com/pLk5k8h.jpg)
（ 上圖為課程截圖 ）

- `obj` 跟 `obj2` 的記憶體位置都是 `0x01`，也都指向 `{a:2}`

---

第二個例子就比較奇特了：

```javascript
obj2 = {b: 1};
console.log(obj); // {a: 2}
console.log(obj2); // {b: 1}
console.log(obj === obj2); // false，記憶體位置不同，連結被斷開了（登愣！）
```

當**物件本身** `obj2` 被傳入新的內容，此時 `obj` 跟 `obj2` 的連結就被斷開了。

![螢幕快照 2019-04-23 下午8.35.51](https://i.imgur.com/ZxFESXu.jpg)
（ 上圖為課程截圖 ）

- `obj` 的記憶體位置 `0x01` 指向 `{a:2}`
- `obj2` 的記憶體位置 `0x05` 指向 `{b:1}`

---

## 三元運算子

當只有一個判斷式 Condition 的時候，有兩個實用小技巧。

舉一個成績判斷為例子。
```javascript
var score = 70;
var result;
if (score >= 60) {
    result = true;
} else {
    result = false;
}
```

#### 技巧一：
如果只是輸出 `boolean` 值 ( `true` or `false` ) 的話，其實可以改寫成：
```javascript
var score = 70;
var result = score >= 60;
```
---

那如果輸出值不是 `boolean`、而是 `string`，如以下範例：
```javascript
var score = 70;
if (score >= 60) {
    console.log("pass");
} else {
    console.log("fail");
}
```
####技巧二：
也可以利用三元運算式來簡寫成： `Condition ? A : B`

- 當 `Condition` 為 `true`，執行 `A`
- 當 `Condition` 為 `false`，執行 `B`

所以將剛剛的範例改成 ：
```javascript
var score = 70;
var result = score >= 60 ? "pass" : "fail"
```
