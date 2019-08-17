---
title: "[第十七週] JavaScript 進階：什麼是閉包 Closure 與實際應用"
layout: post_project
description: ""
category: project
tags: [Closure, project_week17]
file_name: 2019-08-07-project_w17_advancedJS_03_Clousure
---
> Closure 的好處： **可以保存狀態、且不需要用到容易污染的全域變數**，也可以讓外部修改到狀態。

#### 一般寫法

假如寫一個計數器的小程式，傳統的寫法如以下，但因為 `count` 暴露在外部 ( 全域 )，會有不小心修改到 `count` 的可能：

```javascript
var count = 0;
function addCount(){
    count++;
    return count;
}

console.log(addCount()); // => 1
console.log(addCount()); // => 2
```

---
### Closure 寫法

所以可以用閉包 Closure 的寫法、改寫成以下：

```javascript
function createCounter() {
    var count = 0;
    function addCount() {
        count++;
        return count;
    }
    return addCount; // => 重點，回傳一個 addCount function
}

var counter = createCounter(); // 重點，counter 是一個 function: addCount
console.log(counter()); // => 執行 addCount 內容，回傳 1
console.log(counter()); // => 執行 addCount 內容，回傳 2
console.log(counter); // => [function: addCount]
```

實際上就是把剛剛的程式碼再包一層 `function` 而已，然後重點在於**回傳「 要執行的 function 內容 」**，這樣就可以把要保護的狀態包起來，又可以用到裡面的 `method`，因為狀態跟 `method` 在**同一個作用域**底下。

這樣一來，外部就存取不到 `count` 這個變數，所以 closure **簡單來說，可以當成是「 在 `function` 裡面回傳 `function` 」**。

> 注意：要執行的時候，記得要再接收到變數加上 `()`，才是 **真的執行 function**，不然就只是剛剛 `return` 的 `function` 內容而已。

#### closure 簡化版本

如果 closure 寫習慣了就可以把上面的程式碼改寫成更簡短的版本。

```javascript
function createCounter() {
  var count = 0;
  return function() { //=> 直接 return 一個匿名函式
    count++;
    return count;
  }
}

var counter = createCounter();
console.log(counter()); // => 1
console.log(counter()); // => 2
```
---

### 結論：所以什麼是 Closure ？

我的理解是，當你有個**想保護起來的狀態，但又會不停地修改該狀態**，就可以使用閉包**回傳指定的修改方法**，所以閉包接收的就是一個 `function` 內容。

然而閉包神奇的地方在於，一般 `function` 執行完之後，裡面的資源就會全部被釋放掉（ 垃圾回收機制 ），而 Closure 可以保存著裡面的狀態，**以上面的例子來說就是 `count` 這個變數，就算函式執行完，變數也沒有被清掉。**

---

這時同學提了個有趣的提問，如果有兩個變數同樣接收了 `createCount()`，回傳的結果會是一樣的嗎？

```javascript
const count1 = createCounter();
const count2 = createCounter();

count1();
console.log(count1()); // => 回傳 2
console.log(count2()); // => 回傳 1 or 3 ???
```

答案是 `1` 喔，因為兩個 `count` 是重新產生一個 `function`，所以並不會彼此干擾。

---

## 什麼時候需要用到 Closure?


大部分的開發者會認識到 Closure，是因為寫出類似以下的陷阱程式：

```javascript
var arr = [];
for (var i = 0; i < 5; i +=1) {
  arr[i] = function() {
    console.log(i);
  }
}

arr[0](); // 5
arr[1](); // 5
```

例子很簡單，就是把一個 `function` 存在一組陣列 `arr` 裡面，會輸出迴圈裡的數字，如果不是很清楚 JavaScript 的人可能會覺得是按照迴圈的數字輸出，而當我們執行時，會發現都是輸出 `5`，這是為什麼呢？

很簡單，因爲 `arr` 裡面存的每一個值都是內容為 `console.log(i)` 的 `function`，**而真正開始執行的時候 `arr[0]()`，迴圈已經跑完了**，此時的 `i` 已經變成 `5`，所以當然怎麼輸出都是 `5`。

那要怎麼修改上述例子，達到預期中的輸出呢？ 方法有兩種：

### 1. 用 `let` 宣告，產生區塊作用域

第一種比較簡單，用 `let` 宣告變數 `i` 就解決了，因為 `let` 宣告的變數作用域是限制在 block `{ }` 裡面，所以 `for` 迴圈本身也形成一塊作用域，等於每執行一次迴圈，都是一個新的 `i`，所以不會互相影響到。


```javascript
var arr = [];
for (let i = 0; i < 5; i +=1) {
  arr[i] = function() {
    console.log(i);
  }
}

arr[0](); // 0
arr[1](); // 1
```

### 2. 用 Closure 閉包

第二種方式是本章的重點，使用閉包，把要執行輸出的 function return 回去。

```javascript
var arr = [];
for (var i = 0; i < 5; i +=1) {
  arr[i] = function() {
    return log(i); // => return function log
  }
}

function log(n) {
  console.log(n);
}

arr[0](); // 0
arr[1](); // 1
```

也可以在進行簡化，用立即函式 IIFE ：

```javascript
var arr = [];
for (var i = 0; i < 5; i +=1) {
  arr[i] = function() {
    return (function(n) { // => return IIFE
      console.log(n);
    })(i); 
  }
}

arr[0](); // 0
arr[1](); // 1
```

---

### Closure 實際應用（ 一 ）： 拿來避免重複的運算


如果有某種運算是花費大量的時間且又會不停重複，就可以用閉包來做優化。

利用 Closure 寫一個簡單的 Cached 程式，如果有`某輸入`曾經丟入過 `complex()`，那就把`輸出結果`存起來，如果下次有同樣的輸入，就直接存過的拿值、無需重新運算。


```javascript
function complex(num) { // => 假設 complex 要花費很多時間
  console.log('caculating');
  return num * num * num;
}

function Cache(func) {
  let ans = {};
  return function (num) {
    if (ans[num]) {
      return ans[num]; // => ans 有存過，直接回傳
    }
    return ans[num] = func(num); // => 沒存過，丟入 ans 並回傳
  }
}

const CachedComplex = Cache(complex);
console.log(CachedComplex(10)); // 'caculating'，1000 => 跑運算結果
console.log(CachedComplex(10)); // 1000 => 直接拿值
console.log(CachedComplex(10)); // 1000 => 直接拿值
```

---

### Closure 實際應用（ 二 ）： 封裝變數

假設我們有兩個函式 `add` & `deduct`，都會修改到全域變數 `money`，會寫出以下程式：

```javascript
var money = 100;

function add(num) {
  money += 1;
}

function deduct(num) {
  if (num > 10) {
    num -= 10;
  }
  else {
    money -= num;
  }
}

add(1);
deduct(30);

console.log(money); // 101
```

##### 此例潛在的問題：全域變數污染

範例運作良好，程式跑的結果也跟我們想像中一樣，但此例子中的問題是什麼？
1. 有一個全域變數 `money`
2. 如果跟其他同事合作，**中間有可能不小心直接修改到 `money` 的值，例：`money = 1000`，這樣會導致輸出結果不如預期**。

##### 解法：用閉包 Closure 封裝變數

- 把 `money` 放進 `createWallet` 的函式裡，在執行 `createWallet` 丟進初始化的值。
- return 出需要用到的方法 `add`, `deduct`, `getMoney`
- 用一個 `wallet` 變數接住 `createWallet()` 的回傳值  ( 其實就是有 3 個 `method` 的 `Object` )
- **所以 wallet 保留住 `money` 的狀態，也僅能透過調用 `add`, `deduct`, `getMoney` 來存取 `money` 這個變數**，外部是無法使用的，此舉就稱為「 封裝 」。

```javascript
function createWallet(init) {
  var money = init;
  return {
    add(num) {
      money += num;
    },
    deduct(num) {
      if (num > 10) {
        num -= 10;
      }
      else {
        money -= num;
      }
    },
    getMoney() {
      return money;
    }
  }
}

var wallet = createWallet(100);

wallet.add(1);
wallet.deduct(30);

console.log(wallet.getMoney());
```