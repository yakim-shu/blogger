---
title: "[第三週] JavaScript - ES6 語法、Babel 轉譯器"
layout: post
description: "ECMAScript 是 JavaScript 的一種語言標準。而 ES5 及 ES6 代表著 ECMAScript 發佈的不同版本。"
category: project
image: https://i.imgur.com/7mExYJ3.jpg
tags: [JavaSciprt_JS102, ES6, Babel, project_week3]
file_name: 2019-05-01-project_w3_Javasciprt_ES6
---

( image from [Bram.us](https://www.bram.us/2016/10/31/checking-if-a-browser-supports-es6/) )

## ES5 VS ES6
- ECMAScript 5 ( ES5 )：ECMAScript 的第五版，於 2009 年發佈，這個規範幾乎所有現代瀏覽器都支援。
- ECMAScript 6 ( ES6 )：ECMAScript 的第六版，於 2015 年發佈。這個規範**大部分的現代瀏覽器都支援**，詳情可參閱 [ECMAscript ES6 支援表](https://kangax.github.io/compat-table/es6/?utm_source=caibaojian.com)。

### ECMAScript

 ( 以下繞口令取自維基百科 )  
ECMAScript 是一種由 Ecma 國際通過 ECMA-262 標準化的指令碼程式設計語言。這種語言在全球資訊網上應用廣泛，它往往被稱為 JavaScript 或 JScript，但實際上後兩者是 ECMA-262 標準的實現和擴充。 

> 簡單來說，ECMAScript 是 JavaScript 的一種語言標準。而 ES5 及 ES6 代表著 ECMAScript 發佈的不同版本。

參考資料：
- [[ES6] Javascript 開發者必須知道的 10 個新功能](https://medium.com/@peterchang_82818/es6-10-features-javascript-developer-must-know-98b9782bef44)
- [Day15-淺談JS版本差異！ES5、ES6](https://ithelp.ithome.com.tw/articles/10206587![]())
- [JavaScript、ES5和ES6的介绍和区别](http://caibaojian.com/toutiao/6864)

---

## ES6 新語法

### 變數宣告新用法： let、const ( 區塊作用域 block scope )

相較於 ES5 的變數宣告只有 `var`， ES6 多了 `let` 跟 `const` 可以用，重點多了作用域的不同。

#### `const` 常數

當變數是不會也不能有變動、只能讀取的時候，我們稱為常數，常見的有圓周率、設定好的最大、小數等等。
- `const PI = 3.14`
- `const MAX = 5`

> 注意

**這邊「 不能變動 」指的是「 記憶體位置 」**，所以除了原始類型以外，`object`、`array`、`function` 只要不是整個賦值、記憶體位置沒變，指向的內容是可以被更改的喔！

```javascript
const PI = 5; // 數字 → 原始類型
const obj = {}; // object
const arr = [5]; // array

PI = 10;
obj.name = 'Yakim';
arr[0] = 10;

console.log(PI); // 錯誤： Assignment to constant variable.
console.log(obj); // { name : 'Yakim' }
console.log(arr); // [10]

obj = { name : 'Linda' }
console.log(obj); // 錯誤：Assignment to constant variable.
```

#### `let` 塊級變數

##### 特性一： 未宣告不能使用。
`let` 可以當成是 `var` 的升級版，相較於 `var` 會有變數提升 ( Hoisting ) 的特性（ 變數可以在宣告前使用，值為 `undefined` ）， `let` 在宣告之前不能使用。

```javascript
console.log(name); // undefined
var name = 'Yakim'; 
console.log(dog); // 錯誤：dog is not defined
let dog = '小黑';
```

##### 特性二： 塊級作用域
ES5 只提供「 全局作用域 」跟「 函式作用域 」，要麻就是危險的暴露在全域、要麻就是用 function 包起來，所以 `let` 跟 `const` 提供了更多的選擇。

 `let`、`const` 的**作用域是在大括號 `{ }` 裡面**，而不是 `function` 裡，稱為「 塊級作用域 」。
 大大降低了污染到其他變數的可能性。
 
```javascript
if (true) {
    var dog = '小黑';
    let name = 'Yakim';
    const PI = 3.14;
}
console.log(dog); // 小黑
console.log(name); // 錯誤: name is not defined
console.log(PI); // 錯誤: PI is not defined
```

---

### Template Literals 模板字串符

以往在 ES5 操作字串相當麻煩，所以 ES6 提供了比較方便的字串模板，用法超簡單，只要把變數放進 `${ }` 裡面就行，而且多行字串也變得相當容易，參考以下兩種範例：

```javascript
let name = 'Yakim';
let dog = '小黑'
// ES5
// 如果字串裡有用到引號 '，就必須在前面加上反斜線 → \'
console.log(name + '\'s dog is ' + dog + '.'); 
// ES6
console.log(`${name}'s dog is ${dog}.`);

// ES5
// 以往多行字串結尾都要加上 \n 換行，再加上一堆引號跟加號，非常難閱讀
var str = '開頭\n' + 
'多行字串\n' +
'結束';
console.log(str);
// ES6
var str2 = `開頭
多行字串
結束`;
console.log(str2);
```

1. 字串 + 變數： `Yakim's dog is 小黑.`。
2. 多行字串：

```
開頭
多行字串
結束
```

---
### Destructuring 解構

通過匹配 array 中的第 n 值，或是 object 內對應的 key 的值，賦值到變數。注意，匹配陣列使用 `[ ]`、物件使用 `{ }`。

```javascript
// 陣列 ------
let arr = [1, 2, 3, 4, 5];
let [first, second] = arr; // 變數對應到 arr 的 序
console.log(first); // 1
console.log(second); // 2

// 物件 ------
let obj = { 
    name: 'Yakim',
    dog: '小黑',
    family: {
        father: 'LinPiao'
    }
}
let {name, dog} = obj; // 變數要對應到 obj 的 key
let {family: {father}} = obj; // 也可以對應到更裡面，但只會取出最深處的變數，此例是 father
console.log(name); // Yakim
console.log(dog); // 小黑
console.log(father); // LinPiao
console.log(family); // 錯誤：family is not defined

/* 舊的 ES5 寫法 ---
var first = arr[0];
var second = arr[1];

var name = obj.name;
var dog = obj.dog;
var father = obj.family.father;
-------------------
*/
```
---
### Spread Operator 展開運算子

用 `...`作為展開符號，可以**展開陣列或物件**， 由上個例子作為延伸。
```javascript
let arr2 = ['a', 'b'];
let arr3 = ['c', 'd'];
console.log([...arr2, ...arr3]); // [ 'a', 'b', 'c', 'd', 'e' ]
console.log(['new', ...arr2, 'last']); // [ 'new', 'a', 'b', 'last' ]

arr2.push(...arr3);
console.log(arr2); // [ 'a', 'b', 'c', 'd' ]

let name = [...'Yakim'];
console.log(name); // [ 'Y', 'a', 'k', 'i', 'm' ]

let obj = {name : 'Yakim'}
let obj2 = {...obj};
console.log(obj2); // { name: 'Yakim' }
console.log(obj === obj2); // false，記憶體位置不同
```

同樣也可以放進 function 裡當作引數

```javascript
function sum(a, b, c) {
    return a + b + c;
}
let arr = [1, 2, 3];
console.log(sum(...arr));
```

參考資料：
- [阮一峰 - ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/)
- [[ES6-重點紀錄] 擴展運算子 Spread Operator](https://ithelp.ithome.com.tw/articles/10195477)

---
### Rest Parameters 其餘運算子

跟展開運算子很像，也是用 `...` 符號，不過功能剛好相反，作用是**集合剩餘的元素。**

```javascript
let arr = [1, 2, 3, 4, 5];
let [first, second, ...rest] = arr;
console.log(...rest); // 3, 4, 5

let obj = {
    a: 1,
    b: 2,
    c: 3
}
let {a, ...obj2} = obj; // ...集合
console.log(a, obj2); // 1 { b: 2, c : 3 }

let obj3 = {
    ...obj2, // ...展開
    d: 4
}
console.log(obj3); // { b: 2, c: 3, d: 4 }
```

---
### Default Parameters 參數預設值

可以幫函式的參數加上預設值

```javascript
function whosDog(name = 'Yakim', dog = '小黑'){
    return `${name}'s dog is ${dog}`;
}
console.log(whosDog()); // Yakim's dog is 小黑
console.log(whosDog('小明', '小花')); // 小明's dog is 小花
```
也可以搭配解構使用，幫不存在的 `c` 丟入預設值

```javascript
let obj = {
    a: 1, 
    b: 2
}
let {a = 'a', b = 'b', c = 'c'} = obj;
console.log(a, b, c); // 1 2 'c'
```
---
### Arrow Function 箭頭函式

其實就是 function 的簡寫
- 只有一個回傳值，可以省略 `return`
- 只有一個參數，可以省略括號 `( )`

```javascript
/* ES5 寫法 -------
function fn1(name){
    return 'hello, ' + name;
}
------------------
*/
const fn1 = name => `hello, ${name}`;
const fn2 = (name, times) => `hello, ${name.repeat(times)}`;

console.log(fn1('Yakim')); // hello, Yakim
console.log(fn2('Yakim', 3)); // hello, YakimYakimYakim
```
大大的簡化了以往的函式操作
```javascript
let arr = [1, 2, 3, 4, 5]
    .filter(value => value > 1)
    .map(value => value * 2);
console.log(arr); // [ 4, 6, 8, 10 ]
```

---

### import & export 引入跟匯出

操作 `module` 的引入跟匯出，其實跟 ES5 的 `require` & `module.exports` 很像。

> 注意：

原生 node.js 還不支援 `import` & `export` 的寫法，所以要用 `npx babel-node index.js` 來執行，下一段有安裝 Babel 的教學。

#### `export` 匯出
utils.js ： 匯出的 module 檔
```javascript
export const add = (a, b) => a + b;
export const PI = 3.14;

/* 第二種 export 寫法
const add = (a, b) => a + b;
const PI = 3.14;

export {
    add,
    PI
}
*/
```
#### `import` 引入
- 可以用 `*` 引入全部
- 也可以用 `as` 取別名

index.js ： 主程式

```javascript
import {add, PI} from './utils';
console.log(add(1, 2), PI);

/* 第二種 import * 寫法，代表全部
import * as utils from './utils';
console.log(utils.add(1, 2), utils.PI);
*/
```
---

## Babel

當我們需要支援比較老舊的瀏覽器時（ 例如過街老鼠 IE ），沒辦法使用 ES6 甚至 ES5，但還是想用比較潮的寫法怎麼辦？

Babel 是一個 JavaScript 的轉譯器，可以幫你轉成需要的版本、輕鬆解決以上問題，意思是你依舊可以使用潮潮的最新語法，而不用為了使用者環境而吐血。

設定步驟：
- [Babel 官網連結](https://babeljs.io)
- 安裝必要套件：`npm install --save-dev @babel/core @babel/node @babel/preset-env`
    - `@babel/preset-env`：可以使用最新版本的JavaScript然後去編譯，不用去理會哪些語法需要轉換。
- 新增 Babal 的設置檔： `.babelrc`
- 在 `.babelrc` 裡輸入以下內容，告訴 babel 要用這個 preset：

```
{
 "presets": ["@babel/preset-env"]
}
```
- 最後使用 `npx babel-node index.js` 運行即可

參考資料：
- [Webpack教學 (四)：JavaScript 與 Babel](https://medium.com/@Mike_Cheng1208/webpack教學-四-javascript-與-babel-1d7acd911e63)