---
title: "[第十七週] JavaScript 進階：為什麼要有原形鍊 Prototype Chain？"
layout: post_project
description: ""
category: project
tags: [prototype, prototypeChain, project_week17]
file_name: 2019-08-10-project_w17_advancedJS_06_prototypeChain
---

### 沒有 Prototype 會有什麼問題？

在 ES6 之前，JavaScript 是沒有 Class 的用法（ ES6 的 Class 也只能算是語法糖 ），所以要用 prototype 來實現共用方法，而且 Prototype 也可以拿來解決一些問題，看以下例子：

```javascript
function Person(name) {
  this.name = name;
  this.getName = function(){
    return this.name;
  }
}

const nick = new Person('nick');
const peter = new Person('peter');
console.log(nick.getNmae === peter.getName); // => false
```

可以看得出來儘管兩個 instance 都是回傳 `function(){ return this.name; }` 內容，但**因為 instance 的記憶體位置不同，會被視為不同的方法。**


### Prototype 解決了什麼？

所以為了要解決以上狀況，就可以用 prototype 來實現

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.getName = function() {
  return this.name;
}

const nick = new Person('nick');
const peter = new Person('peter');
console.log(nick.getName === peter.getName); // => true
```
#### prototype 共用方法

而為什麼 prototype 就可以讓 method 相等呢？

JS 的運作原理是當我們執行 `nick.getName` 的時候，會先去看 `nick` 這個 instance 有沒有 `getName` 這個 method，以上例子在宣告 `Person` 這個 Class 是沒有的。

那下一步呢，就是看 `Person` 的原型 `prototype` 裡面找，就發現有 `getName` 這個方法，而 `nick` 跟 `peter` **這兩個 instance 是共用 Class 的 prototype 方法**，所以才會回傳 `true`。

```javascript
console.log(nick.getName === Person.prototype.getName); // => true
```

---

## Prototype 原型鏈的原理

推薦先閱讀這兩篇文章：[Javascript继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)、[从设计初衷解释 JavaScript 原型链](https://www.jianshu.com/p/a97863b59ef7)

可以先從為什麼這樣設計 JavaScript 出發，當初設計語言並沒有 Class，所以只能用建構函式 function（ 可以當作 `constructor` ），所以就可以知道 prototype 的出現是為了達到所有 instance 都能共享 `property` & `method`。

JavaScript 一開始設計的時候，所有皆物件 Object，所以需要一個機制來連接物件，也進而達成繼承功能，實現 Instance 的方法是透過 **new 出一個建構函式**。

```javascript
function Person(name) {
    this.name = name;
    this.sayHi = function() {
        console.log('Object', this.name, 'Hi'); // => new 出幾份 instance 就會有幾份，佔用記憶體空間
    }
}

Person.prototype.sayHi = function(){
    console.log('Object', this.name, 'Hi'); // => 用 prototype 實現共享 property & method
}
```

這是怎麼達成這機制的呢？

---

## `.__protp__` : 隱藏的指路人

當我們創建一個 instance 的時候，JS 會自動幫你加上一個 `.__proto__` 的屬性，作用是告訴程式，當我們在 Class 找不到 method 的時候，要去哪裡找，算是一個 **指引** 的概念

```javascript
console.log(nick.__proto__ === Person.prototype); // => true;
console.log(nick.__proto__.getName === Person.prototype.getName); // => true;
```

**而 `.__protp__` 這個指引的屬性可以省略**，所以以下才會成立

```javascript
console.log(nick.getName === Person.prototype.getName); // => true
```


**在使用 new 的同時，instance (peter) 會自動加上 `.__proto__`，並且指向建構函式 (Person) 的 `.prototype`。**

```javascript
peter.__proto__ 指向 => Person.prototype 
nick.__proto__ 指向 => Person.prototype 
```

而**建構函式 `Person` 其實也是 Object `new` 出來的啊**，所以 Person.prototype 也是會有 `.__proto__`，同樣指向 `Object.prototype`

```javascript
=> Person.__proto__ === Person.prototype
=> Person.__proto__.__proto__ === Object.prototype
=> Person.prototype.__proto__ === Object.prototype
=> Object.prototype === null

1. Person 是 function 的 instance
    => 可以想像成是 var Person = new Function();
    => Person.__proto__ === Function.prototype
2. function 是 Object 的 instance
    => 可以想像成是 var Function = new Object();
    => Function.prototype.__proto === Object.prototype
    => Person.__proto__.__proto__ === Object.prototype
```

所以可以看得出來， peter (`instance`) 的原型會指向 Person (`constructor`)，而 Person (`constructor`) 的原型又會指向 Object，**這樣鍊型的關係達到了繼承的效果，而又稱為原型鍊**。


---

## Prototype Chain ： 尋找的順序

```javascript
console.log(nick.getName());
```

所以要怎麼找到 `getName` 這個 method 呢？

1. `nick` 
2. `nick.__proto__` 
    - ( = `Person.prototype` )
3. `nick.__proto__.__proto__` 
    - ( = `Person.prototype.__proto__` )
    - ( = `Object.prototype` )
4. `nick.__proto__.__proto__.__proto__` => 最上層，也就是 `null`
    - ( = `Person.prototype.__proto__.__proto__` )
    - ( = `Object.prototype.__proto__` )


```javascript
console.log(nick.getName === Person.prototype.getName); // => true
console.log(nick.__proto__ === Person.prototype); // => true;
console.log(nick.__proto__.__proto__ === Person.prototype.__proto__); // => true;
console.log(nick.__proto__.__proto__ === Object.prototype); // => true;
console.log(Object.prototype.__proto__); // => null;
```

所以 JS 會一直往上找，一但找到該 method 就會停止。

所以如果 `Person` 跟 `Object` 的 prototype 都有 `getName`，真正回傳的會是 `Person`的 `getName`，因為 Person 的尋找優先順序比較高。

所以這樣 **一層一層往上查找，就構成一條原型鍊 prototype chain**，而這條鍊的最頂層就是 `Object.prototype.__proto__` 也就是 `null`。

> 注意： 以上都是了解 JS 底層實作的概念，實務上盡量不要去用到 `.__proto__` 這個屬性。

可以再看下一個例子，確定自己清楚 prototyp 的尋找順序：

```javascript
function Person(name) {
  this.name = name;
  this.sayHi = function () {
    console.log(this.name, 'Hi => method of instance'); // => new 出幾份 instance 就會有幾份，佔用記憶體空間
  }
}

Person.prototype.sayHello = function () {
  console.log(this.name, 'Hello => method of Class'); // => 用 prototype 實現共享 property & method
}

Object.prototype.sayHello = function () {
  console.log(this.name, 'Hello => method of Object'); // => method 跟 Person 重複，所以找到 Person 就停了
}

Object.prototype.sayYo = function () {
  console.log(this.name, 'Yo  => method of Object');
}
const peter = new Person('peter');

peter.sayHi(); // peter Hi => method of instance
peter.sayHello(); // peter Hello => method of Class
peter.sayYo(); // peter Yo  => method of Object

// => 這樣才可以使用到 Object 的 method
Object.sayHello.call(peter); // peter Hello => method of Object
```


---

## 補充
### ☞ `hasOwnProperty`: 確認是否為 instance 自己的方法

```javascript
peter.hasOwnProperty(sayHi); // => true
peter.hasOwnProperty(sayHello); // => false
```

而當我們寫下 `peter.sayHello` 的時候， `.sayHello` 這個 method 並不在 `peter` 這個 instance 上面，就會自動往上一層查找 => `Person.prototype` 

所以如果**想知道該 method 是在 instance 上，還是存在於原型鍊中**，可以用 `hasOwnProperty` 這個方法確認

### ☞ `instanceof`: 確認 A 是否為 B 的 instance

```javascript
peter instanceof Person; // true
Person instanceof Function; // true
Person instanceof Object; // true


// Function 跟 Object 互為對方的 intance，好奇妙
Function instanceof Object; // true 
Object instanceof Function; // true
```

### ☞ `constructor`: 建構函式

每一個 prototype 都有個 `constructor` 屬性，而想當然爾就是指向自己（建構函式）囉。

```javascript
peter.constructor === Person; // true;
Person.prototype.constructor === Person; // true
Person.prototype.hasOwnProperty('constructor'); // true
```
