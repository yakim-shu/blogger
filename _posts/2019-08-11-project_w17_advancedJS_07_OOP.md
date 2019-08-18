---
title: "[第十七週] JavaScript 進階：物件導向 (new、super、封裝)"
layout: post_project
description: ""
category: project
tags: [prototype, prototypeChain, project_week17]
file_name: 2019-08-11-project_w17_advancedJS_07_OOP
---

## 物件導向的寫法： ES5 VS ES6

在 ES6 之後，開始有了 Class 這個語法糖可以使用，可以比較看看兩種不同版本的物件導向寫法：

#### ES6 : Class

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(this.name, ' Hello');
  }
}
const happy = new Dog('happy');
happy.sayHello();

const white = new Dog('white');
white.sayHello();
```

#### ES5 : 建構函式 + prototype

```javascript
function Dog(name) {
  this.name = name;
}

Dog.prototype.sayHello = function() {
  console.log(this.name, ' Hello');
}

const happy = new Dog('happy');
happy.sayHello();

const white = new Dog('white');
white.sayHello();
```

---- 

## new 關鍵字 : 模擬 new 在背後幫你做的事

在 ES5 時，**通常是把 function 當成 constructor 來用，稱為「 建構函式 」**，而共用的 method 則會放在 prototype 新增，可以避免在每個 instance 重複新增內容一樣的 method。

而關鍵在於 `new` 關鍵字，用 `new` 出來的 function， JS 會在背後幫你做好一連串的機制。

下例是 ES5 的 OOP 寫法：

```javascript
function Dog(name) {
    this.name = name;
}
Dog.prototype.sayHi = function() {
  console.log(this.name, 'Hi');
}

const white = new Dog('white');
```

用上面這個例子，來描述關鍵字 `new` 在背後到底做了什麼？

**接下來以一個自己寫的 function `newDog('yello')`，來模擬 `new Dog('yello')` 在做的事**：

```javascript
const yello = newDog('yello');

white.sayHi();
yello.sayHi();
```

首先目的是通過自己寫的 `newDog` 產生一個 instance `yello`，希望這個 instance 會有 跟用 `new` 出來的 instance `white` 有一樣的屬性跟方法。

所以目標放在如何寫出 `newDog()` 來模擬 `new` 這個關鍵字背後做的事情。

```javascript
function newDog(name) {
  const obj = {};                 // 1.
  Dog.call(obj, name);            // 2.
  obj.__proto__ = Dog.prototype;  // 3.
  return obj;                     // 4.
}

/* 
new 背後做的事：
1. 產生一個新 object => obj
2. 把 Dog 當作 constructor，將 this 指向 obj，同時把參數 name 丟進去
    => 因為 Dog 只有一個參數所以用 call，如果是兩個以上的參數就可以用 apply 放陣列
3. 將 obj.__proto__ 指向 Dog.prototype
4. 回傳 obj
*/

```


--- 

## 繼承 : super

在 ES6 使用繼承時，當**要改寫 子類別的 constructor 時，記得要先用 `super()` 父類的 constructor**，不然會跳出錯誤：`Must call super constructor in derived class before accessing 'this' or returning from derived constructor`。

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    console.log(this.name, 'Hi');
  }
}

class TaiwansDog extends Dog {
  constructor(name) {
    super(name); // => 記得要先 super()
    this.sayHi();
  }
}

const white = new Dog('white');
const black = new TaiwansDog('black');
```

---

## 練習封裝： 實現 private property 的幾種嘗試

之前這篇筆記：[什麼是閉包 Closure 與實際應用](https://yakimhsu.com/project/project_w17_advancedJS_03_Clousure.html) 裡，Closure 最後一個 `wallet` 範例是說明 Closure 可以達到封裝的目的，讓我想到之前看 YT 上的 [JavaScript OOP 教學](https://www.youtube.com/watch?v=PFmuCDHHpwk)。

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

console.log(money); // 91
```

在 ES5 要實現 Class 中的私有屬性，也是在 `function`（ 建構函式 ）內部宣告一個變數，之後再利用用 `getter` & `setter` 去存取 private property。

那後來又想著，**那到底要如何在 ES6 中的 Class 實現 private 屬性呢？**

查了一下 [Private properties in JavaScript ES6 classes](https://stackoverflow.com/questions/22156326/private-properties-in-javascript-es6-classes?page=1&tab=votes#tab-top)，第 2, 3 個回答得到不少啟發，尤其是第 3 個列舉了不同方法跟說明，認識到 `Symbols` 類型，雖然不是真正意義上的 private，但還是滿酷的！

想說練習一下，直接把 `wallet` 範例把多餘的 `method` 刪掉，改成以下三種：
- ES5 - function 模擬 Class 版 ( real private )
- ES6 - constructor 版 ( real private )
- ES6 - Symbols 版 ( half private )


### 1. ES5 - function 模擬 Class 版 ( real private )

很標準的 ES5 封裝方法。

```javascript
function Wallet(init) {
  let money = init;

  this.getMoney = function() {
    return money;
  }
};

const wallet = new Wallet(100);
console.log(wallet.getMoney());
console.log(wallet.money); // => 存取不到，真正的 private
```

### 2. ES6 - constructor 版 ( real private )

需要存取 private property 的 method 都只能放在 constructor 裡面，是可以正常運作，但感覺不太對勁

```javascript
class Wallet {
  constructor(num) {
    var _money = num;
    this.getMoney = () => _money;
    this.setMoney = (newNum) => _money = newNum;
  }
};

const wallet = new Wallet(100);
console.log(wallet.getMoney());
console.log(wallet._money); // => 存取不到，真正的 private
```

### 3. ES6 - Symbols 版 ( half private )

就是看著範例用，沒有深究，只覺得新語法很酷

```javascript
var money = Symbol();
class Wallet {
  constructor(num) {
    this[money] = num;
  }
  
  getMoney() {
    return this[money];
  }

};

const wallet = new Wallet(100);
console.log(wallet.getMoney());
console.log(wallet[money]); // => 還是存取得到，不是真正的 private
```

---

### 結論

所以在 ES6 實現 private property 最方便的寫法是什麼？

> 沒有。

對，真的就是沒有。

而如果 ES7 的支援性越來越高，要實現 private property 就有更方便的 `#` 關鍵字可以用，但現在 ES6 還沒有看到通用的方法，最簡單的方法其實可以再 private property or method 的名稱加上 `_` 前綴，同事們可以很簡單看出這是個私有屬性，沒事不要去動它。