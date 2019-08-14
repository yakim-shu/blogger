---
title: "[第十七週] JavaScript 進階： Primitive & Object 原始類型與物件的差異"
layout: post_project
description: ""
category: project
tags: [Primitive, Object, typeof, project_week17]
file_name: 2019-08-04-project_w17_type_primitive&object
---

JavaScript 有兩種資料類型，分為：

- Primitive: 原始 / 基本 （ 非物件 ）
- Object: 物件

#### Primitive 原始型態 
`Primitive Type` 又可以再細分成六種：
* `undefined`
* `null`
* `string`
* `number`
* `boolean`
* `symbol` (ES6)

#### Object 物件型態 
而非以上六種的類型，就是物件類型 `Object Type` ： 
* `Function`
* `Array`
* `Date`

---

## Primitive VS Object

本章的重點聚焦在這兩種資料類型，有什麼不同的差異。

#### （ 一 ）Primitive 是 Immutable： 原始形態不能改變

這裡指的原始型態不能改變是**「 呼叫方法時，不會改變到原來的值 」，而是回傳一個新的值**，看以下範例：

```javascript
const str = 'hello';
const newStr = str.toUpperCase();
console.log(str, newStr); // => hello, Hello

const arr = [1];
arr.push(2);
console.log(arr); // => [1, 2]
```

- 當 string 呼叫 `.toUpperCase()`，原本的 `str` 並不會改變，所以**必須用一個新的變數 `newStr` 來接收回傳值**。
- 而 `arr` 呼叫 `.push()` 時，原本的 `arr` 就直接產生了變化。

這邊要注意的一點是，**物件型態 `Object` 雖然能夠改變，但「 也不代表一定會改變 」**，像有些陣列的內建函式**也是把結果回傳成一個新陣列**，例如： `Array.concat()`、`Array.filter()`... 


#### （ 二 ）存值的方式不一樣

- Primitive: 直接放值，各變數不會互相影響
- Object: 存記憶體位置，會互相影響
    - 但如果是重新賦值，則會重新再指定一塊新的記憶體位置

```javascript
let arr = [];
let arr2 = arr;
arr.push('hey');
console.log('arr: ', arr); 
console.log('arr2: ', arr2);

// push 這個 method 會去找到 arr2 的記憶體位置，再進行改變 => 共享記憶體位置
// arr: ['hey']
// arr2: ['hey']


let arr3 = arr;
arr3 = ['yo'];
console.log('arr: ', arr); 
console.log('arr3: ', arr3);

// 將 arr3 重新賦值 => 指定新的一塊記憶體位置
// arr: ['hey']
// arr2: ['yo']
```

所以差別在於：
- 第一種做法 `arr.push`，是去 `arr` 的記憶體位置找到 `arr` 的值並改變。
- 第二種做法 `賦值`( `arr3 = ['yo']` )，是先重新畫一塊記憶體放值 `['yo']`，再改變 `arr3` 存的記憶體位置，可以當成一個地址，指向 `['yo']` 這個值。


#### （ 三 ）比較的方式不一樣 `===`

其實就是延續第二點，因為 Object 比較的方式是看「 記憶體位置 」。

```javascript
console.log([] === []); // false
console.log({} === {}); // false
console.log([1, 2, 3] === [1, 2, 3]); // false

console.log(NaN === NaN); // 特殊例子，NaN 永遠不會等於自己，要比對可以用 isNaN()
```

##### 小補充： == VS ===

最大的差別就是 `==` 會做類型轉換，所以還是盡量都用 `===` 做比對

詳細可以參考 [JavaScript Equality table](https://dorey.github.io/JavaScript-Equality-Table/) 網站看各種類型比對的結果。

#### （ 四 ）const 常數的運用

常數的規則是一但賦值後就不能改變，但 **不能變的是「 記憶體位置 」**，而因為 `Object` 存的是記憶體位置，所以**同樣地址的內容**其實是可以改變的。

```javascript
const obj = {
    number: 1
};

obj.number = 2; // => 沒問題，可以更改內容

obj = {
    number: 2; // => 賦值會改變記憶體位置，所以跳出錯誤訊息
}
```

---

### `typeof` : 檢查資料類型 

如果不清楚 `某資料` 是哪種資料類型，可以用 `typeof` 進行檢查。

但要注意的一點是，`typeof` 出來的答案可能會跟預期的不一樣，可以看 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof) 對照各種資料型態 typeof 的結果，像以下範例：

```javascript
typeof null // => Object，是一個 JavaScript 廣為人知的 bug
```

還有你沒有辦法用 `typeof` 去檢測變數是不是 `Array`，但可以用 `Array.isArray(<要檢驗的array>)`

#### Q: 有比 `typeof` 更可靠的檢查方法嗎？

所以如果想用個更可靠的方法來檢查的話，可以用 `Object.prototype.toString.call(<變數>)`，回傳陣列的第二個值就是資料類型。

#### Q: 那 `typeof` 存在的意義是什麼？ 

**「 可以拿來檢查變數是否存在 」**

如果對一個未宣告或未賦值的變數做 `typeof`，會回傳 `'undefined'` ( 是字串 )

所以有些時候可以拿 `typeof` 來檢查變數是否存在。

```javascript
if (typeof a !== 'undefined') {
    console.log(a); // => 確定 a 存在且有賦值，再進行對 a 的操作
}
```

