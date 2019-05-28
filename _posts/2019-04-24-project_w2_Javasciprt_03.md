---
title: "[第二週] 基礎 JavaScript - 03 函式 Function"
layout: post_project
description: "不同的程式語言總會有些自己的內建函式，目的在於簡化一些常用的功能，總之就是有人貼心造好的輪子，就不用再自己造了。"
category: project
image: 
tags: [JavaSciprt_JS101, project_week2, function]
file_name: 2019-04-24-project_w2_Javasciprt_03
---
## 參數 Parameter & 引數 Argument

- `a`、`b` 為 `fun1()` 的參數
- `c`、`d` 為 `fun1()` 的引數（ 實際引用進來的 ）

```javascript
function fun1 (a, b) {
    return a+b;
}
var c = 10;
var d = 20;
console.log( fun1(c,d) );
```

### Function 參數也可以丟入 Function

`fun2()` 也可以當作**引數**傳入`fun1()` 當作**參數**。

以下例子中，`fun1()` 的參數 `newFunction` 其實傳入的就是 `fun2()`。
```javascript
function fun1(i, newFunction) { // newFunction 為 fun1() 的參數
    return i + newFunction;
}
function fun2(x) {
    return x * 2;
}
console.log(fun1(5, fun2(10))); // fun2() 為 fun1() 的引數
``` 

以下例子將陣列 `[1, 2, 3]` 傳入 `transform()`，目的是回傳兩倍 & 三倍的陣列值。
```javascript
function transform(arr, transformFunction) {
    var result = [];
    for (var i=0; i<arr.length; i++) {
        result.push(transformFunction(arr[i]));
    }
    return result;
}
function double(x) {
    return x*2;
}

function triple(x) {
    return x*3;
}
console.log(transform([1,2,3], double)); // [2, 4, 6]
console.log(transform([1,2,3], triple)); // [3, 6, 9]
```

依照上面的例子，也可以將函式 `double()` 跟 `triple()` 改寫成 **匿名函式 Anonymous Function** 
（函式內容一樣，只是沒有名字）
```javascript
function transform(arr, transformFunction) {
    var result = [];
    for (var i=0; i<arr.length; i++) {
        result.push(transformFunction(arr[i]));
    }
    return result;
}
console.log(transform([1,2,3], function(x){ return x*2 })); // [2, 4, 6]
console.log(transform([1,2,3], function(x){ return x*3 })); // [3, 6, 9]
```

有關 Function 的宣告方式其實還有另一種，更詳細的可以參考以前筆記：[Javascript大全 - 06 函式(一) 函式的定義方法](https://yakimhsu.com/javascript/JS_Daquan-06_1.html)，裡面有個經典案例去說明 JavaScript 對於**不同的宣告方式會造成的不同編譯順序結果**。

---

## 到底傳什麼？ Pass by (__) ? 

又回到那個 pass by value 還是 pass by reference 還是 pass by address 還是 pass by sharing...無窮迴圈。
這大概是每隔一陣子就要複習一次，陷入漫長的查資料地獄，發現每個人說得好像都有點道理，開始進入**越查越不懂**的恐怖循環裡。

而 Huli 分享了他之前研究此問題的過程，連結在此：[深入探討 JavaScript 中的參數傳遞：call by value 還是 reference？](https://blog.techbridge.cc/2018/06/23/javascript-call-by-value-or-reference/)

幸好之前有留下 CS50 對 C 語言中**指標**的筆記，所以那篇文章我居然看得懂（開心）！推薦大家認真看完，文章也說明了此問題在其他語言（ Java, C ）的情形，我甚至有點像坐在榕樹下聽說書人講故事的感覺 (？)，我直接引用結尾的一斷話：

> 別忘了，重點其實不在這個，而是搞清楚到底參數在操作的時候會有怎樣的行為。你要知道 JavaScript 傳 object 進去的時候，可以更改原本物件的值，但重新賦值並不會影響到外部的 object。只要知道這一點，其他的我覺得都沒那麼重要了。

看到這段話有「啊～我得到救贖了」的感覺，所以我決定不再去追根究底名詞背後的定義是什麼，我想我只要**了解其中的行為**，比該**如何定義此行為**還重要。

概念有點像之前看到一個網路上蠻有名的昆蟲教授，他說：「 要怎樣才算是瞭解一個昆蟲呢？講得出他的中英文學名、知道介門綱目科屬種分在哪一類？還是說得出他的生物特性、了解什麼環境或因素會引發什麼特殊行為。 」（ 原文找不到，其實應該不是這麼說的，我只是憑印象說明 ）

### 總之直接看 code

#### pass by value 傳「值的拷貝」 （ 無庸置疑 ）

原始型別 `num`、`string`、`boolean` 都是傳值 pass by value，原來的 `num1`、`num2` 並沒有被更改。
```javascript
function swap (a, b){
    var temp = a;
    a = b;
    b = temp;
}
var num1 = 10;
var num2 = 5;
swap(num1, num2);
console.log(num1, num2); // 印出 10, 5
```

#### pass by sharing 傳「值的記憶體位置 」（ 待討論，不是這裡的重點 ）
而物件 `object` 則是傳分享 pass by sharing，**原來的 `obj1` 被更改到了**
```javascript
function swap (obj){
    var temp = obj.a;
    obj.a = obj.b;
    obj.b = temp;
}
var obj1 = {
    a: 10,
    b: 5
}
swap(obj1);
console.log(obj1); // 印出 { a: 5, b:10 }
```

而又如果**物件 `object` 本身被重新賦值**，原來的 `obj1` 就**不會收到影響。**
```javascript
function double(obj) {
    obj = {
        a: obj.a * 2
    }
}
var obj1 = { a: 10 };
double(obj1);
console.log(obj1); // 印出 { a: 10 }
```

---

## 內建函式

不同的程式語言總會有些自己的內建函式，目的在於**簡化一些常用的功能**，總之就是有人貼心造好的輪子，就不用再自己造了。

### 常用的數學相關函式
- `Math.ceil()` ： 無條件進位
- `Math.floor()` ： 無條件捨去
- `Math.round()` ： 四捨五入 
- `Math.random` ： 產生 0 ~ 1 的隨機數（  不包括 1 ）
- `Math.max(num1, num2, num3)` ： 丟入一系列數字，回傳最大值
    - 也可以**丟陣列，但寫法要用 `展開運算子 ...`**，例如： `Max.max(...arr)`
- `Math.min(num1, num2, num3)` ： 丟入一系列數字，回傳最小值
    - 用法如同 `Math.max()` 

- `Number.MAX_VALUE` ： 回傳 JavaScript 能存的最大數字。
- `Number.toFixed(x)` ： 取到小數點後第 x 位數（ 無條件捨去 ）

- `Math.sqrt()` ： 開根號
- `Math.log()` ： 對數
- `Math.pow(x, y)` ： 次方運算
    - x 的 y 次方
 
參考資料：
- [從陣列計算最大和最小值](http://www.jstips.co/zh_tw/javascript/calculate-the-max-min-value-from-an-array/)
- [MDN - Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

#### 字串、數字間互相轉換

字串轉數字：
- `parseInt(<string>, x)`
- `Number(<string>)`

```javascript
console.log(parseInt('50', 10)) // 50，10 代表 10 進位
console.log(Number('50')) // 50
```

數字轉字串：
- `<number>.toString()`
- `<number> + ''`  ： ( 數字加上空字串 ) 

```javascript
var num = 10;
console.log(num.toString());
console.log(num+'');
```

---

### 常用的字串相關函式
- `length` ： 回傳字串的長度
- `toUpperCase()` ： 將字串全部轉為大寫
- `toLower​Case()` ： 將字串全部轉為小寫
- `charCodeAt(char)` ： 取出**字元**的 ASCII 碼
- `String.fromCharCode(num)` ： 將 ASCII 碼轉成**字元**
- `indexOf(keyword)` ： 找出關鍵字 keyword 有沒有在字串裡
    - 有，回傳 keyword 索引值
    - 沒有，回傳 `-1`
- `replace(x, y)` ： 找的**第一個 `x`** 字串，用 `y` 取代
    - 如果要取代所有的 `x`，要用正規表達式 : `replace(/x/g ,y)`
- `split(x)` ： 分割字串成陣列
    - 用 `x` 作為分割字串的參考，**回傳陣列** 
- `trim()` ： 去掉前後的空格

```javascript
var name = 'Yakim';
console.log(name.toUpperCase()); // Upper:  YAKIM
console.log(name.toLowerCase()); // Lower:  yakim
console.log(name.charCodeAt(0)); // code of Y:  89
console.log(name.charCodeAt(1)); // code of a:  97
console.log(String.fromCharCode(97)); // char of 97: a

var name2 = 'Yakim, hahahahaha';
console.log(name2.replace('ha', 'yo')); // Yakim, yohahahaha
// 注意：前面的引數類型不是字串，不用加 ''
console.log(name2.replace(/ha/g, 'yo')); // Yakim, yoyoyoyoyo
console.log(name2.split(',')); // [ 'Yakim', ' hahahahaha' ]

var name3 = '      Yakim   ';
console.log(name3.trim());  // Yakim
```

#### 字串、陣列間互相轉換

字串轉陣列：
- `str.split('')` 
    - 利用空字元 `''` ： 把 `string` 切成 `array`

陣列轉字串：
- `arr.join('')`
    - 利用空字元 `''`： 把 `array` 轉成 `string`

---
### 常用的陣列相關函式

這部分先偷懶拿以前筆記來看，不然進度太慢、我要開天窗了啊啊啊啊

- [Javascript大全 - 05(一) 陣列](https://yakimhsu.com/javascript/JS_Daquan-05.html)
- [Javascript大全 - 05(二) EMCAScript 5 陣列方法](https://yakimhsu.com/javascript/JS_Daquan-05_2.html)


重點在於不同函式之間，有**會改到原本陣列**跟**不會改到原本陣列、而回傳一個新陣列或其他變數類型**的差別，通常是在於 **回傳值也是陣列的情況下**，就很有可能會改到陣列本身，聽起來也很合理，因為既然是要對陣列做改變，就直接在陣列本身做操作麻！（ 說了通常是因為有例外，例如: `slice()`、`concat()` ）

想要看更詳細的資訊，可以上 [MDN - Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Array_instances) 裡面有超詳細的列表：
- Mutator methods 目錄底下：會改到原始 Array 的函式
- Accessor methods 目錄底下：不會改到原始 Array 的函式
