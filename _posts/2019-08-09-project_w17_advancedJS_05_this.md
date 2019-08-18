---
title: "[第十七週] JavaScript 進階：出乎意料的 this"
layout: post_project
description: ""
category: project
tags: [this, project_week17]
file_name: 2019-08-09-project_w17_advancedJS_05_this
---

## this 是什麼？ 為什麼這麼複雜？

首先，可以先看看文章：[淺談 JavaScript 頭號難題 this：絕對不完整，但保證好懂](https://github.com/aszx87410/blog/issues/39)，讀完內文先記得一件事： **「 脫離了物件，this 的值就沒什麼意義 」**。

沒有意義是指 `this` 值常常會出乎你意料，在物件中很單純，除了特意改變 `this` 的值，不然 `this` 就是指向物件本身或 instance。

而接下來要討論的就是 `this` 為什麼這麼多變，又是怎麼改變的。

---

### 非物件導向下的 this

在**非物件導向的環境下**，this 會是預設值，而 `this` 的預設值會依據環境的不同而有所改變。 

`this` 的預設值：
- node.js : `global`
- 瀏覽器 : `window`
- 嚴格模式 : `undefined` ( node.js 跟瀏覽器都一樣 )

**想要把 this 統一的話，其實可以改成嚴格模式**： `'use strict';`，這樣 `this` 就會變成 `undefined`

---


## this 是被 call 的當下決定

this 的值很常被搞混，是因為不清楚 this 的由來，先看一下範例：

```javascript
'use strict';

const obj = {
  sayThis: function() {
    console.log(this);
  }
}

obj.sayThis(); // => { sayThis: [Function: sayThis] } => 就是物件 obj 本身
```

`sayThis()` 由 `obj` 呼叫，所以 `this` 很單純、就是 `obj`。

而如果用一個變數 `say` 去接收 `sayThis` 這個 `function` 本身呢？

```javascript
'use strict';

const obj = {
  sayThis: function() {
    console.log(this);
  }
}

const say = obj.sayThis;
say(); // => 輸出 undefined
say.call('who'); // => 輸出 who
```

咦？ 函式內容不變，this 怎麼不一樣？

注意看 `say` 只是指向 `obj.sayThis` 這個 `function` 地址，但並不是由物件呼叫的，在這裡等於是在全域環境下呼叫一個 `function`，所以 `this` 會是預設值，但因為前面有指定嚴格模式，所以才會輸出 `undefined`。

所以到目前為止，只要記得一件事就好，**this 是在被呼叫的當下決定，跟在哪裡宣告沒有關係**。

----

## 那如何改變 this 值？
#### ☞ 方法一 `call` & `apply` : 呼叫 function

有兩個 method 能夠以指定的 `this` 立刻呼叫 function，`call`、`apply`：

```javascript
'use strict';

function test(a, b, c) {
  console.log('this: ', this);
  console.log(a, b, c);
}

test(1, 2, 3); // this:  undefined
test.call('I am call', 1, 2, 3); // this: I am call
test.apply('I am apply', [1, 2, 3]); // this: I am apply
```

- 第一個輸出：
    - 預設值用嚴格模式下是 `undefined`
- 第二個輸出：
    - `.call(<this>, <arugemt_1, arugemt_2...>)` 改變 this 為字串 `I am call` 
- 第三個輸出：
    - `.apply(<this>, [<arugemt_1, arugemt_2...>])` 改變 this 為字串 `I am apply`，**第二個參數為一個陣列**，裡面放參數

所以看得出來 **`call` & `apply` 的差別，其實就只有參數是不是 array 的形式**而已。



#### 改寫剛剛的例子

```javascript
// 以下兩種寫法相等
obj.sayThis();
obj.stayThis.call(obj);

// 以下兩種寫法也相等（ 瀏覽器中、且非嚴格模式 ）
say();
say.call(window);
```

---

#### ☞ 方法二 `bind` ： 回傳一個指定 this 的 function

那如果我們只想要**固定 `this` 值、沒有要立刻執行**怎麼辦呢？ 這時就可以用 `bind`，用 function `bind` 會回傳固定 `this` 的 function 本身。

這樣就不用擔心因為呼叫當下而改變 `this`，**且之後就算用 `call` or `apply` 也改變不了 `this`**。

```javascript
const obj = {
  sayThis: function() {
    console.log(this);
  }
}

const say = obj.sayThis.bind('hello');
say();           // => 輸出 [String: 'hello']
say.call('who'); // => 還是輸出 [String: 'hello']
```

---

## 還沒完，還是有討厭的例外
#### 例外（ 一 ） 事件監聽中的 `this`

像 DOM 物件綁定某種事件時，`this` 會變成綁定的 DOM 元素本身:

```javascript
document.querySeletor('.btn').addEventListener('click', function(){
    console.log(this); // => btn 這個元素
});
```

---

#### 例外（ 二 ）箭頭函示 arrow function 中的 `this`

之前不是說過 `this` 跟在哪裡定義無關、而是跟在哪裡呼叫有關嗎？

但有個例外喔，就是箭頭函式。

#### example1: 一般例子

```javascript
'use strict';

class Test {
  run() {
    console.log('run: ', this);
    setTimeout(function() {
      console.log('setTimout: ', this);
    }, 1000);
  }
}
const test = new Test(); 
test.run();
// run: Test {}
// setTimout: undefined ( 一秒後 )
```

以上例子看起來很正常，沒有問題，那如果換成 arrow function 呢？ 會發現 setTimout 的 `this` 變成 `Test` 了。

#### example2: 改成箭頭函式 arrow function

```javascript
'use strict';

class Test {
  run() {
    console.log('run: ', this);
    setTimeout(() => {
      console.log('setTimout: ', this);
    }, 1000);
  }
}
const test = new Test();
test.run();
// run: Test {}
// setTimout: Test {} ( 一秒後 )
```

`this` 在 arrow function 中，**跟在哪裡定義有關，此時的 `this` 有點像變數的行為，會依照作用域去抓外部的 `this`**，依據上一層的 `run: this` 是什麼，`setTimout: this` 就會是什麼。

#### example3: 用 bind 改變 this ， arrow function 裡面的 this 也會跟著換

要驗證的話，可以把 `run` function 的 `this` 固定成一個字串 `hello`，所以此處的 `setTimout: this` 也會跟著變成 `hello`。

```javascript
class Test {
  run() {
    console.log('run: ', this);
    setTimeout(() => {
      console.log('setTimout: ', this);
    }, 1000);
  }
}
const test = new Test();
const newTest = test.run.bind('hello'); // => 把 this 固定成 hello
newTest();

// run:  hello
// setTimout: hello ( 一秒後 )
```


---

### 結論 
#### this 到底是什麼？

有很多因素要考慮：
1. 非物件導向底下，要看執行環境有不同預設值：
    - node.js : `global`
    - 瀏覽器 : `window`
    - 嚴格模式 : `undefined` ( node.js 跟瀏覽器都一樣 )
2. 物件導向中， `this` 就是 `new` 出來的 `instance`
3. DOM 事件綁定時， `this` 為綁定的 DOM 物件
4. 單純在物件底下，`this` 跟如何呼叫 function 有關
    - 如果是在物件本身上呼叫，`this` 為物件本身
    - 把 function 抽出來呼叫，`this` 則為預設值 ( `global or window or undefined` )
5. arrow function 中，根據外部的 this 是什麼而決定

####  如何改變 `this`：
1. `bind` : 將 `this` 值固定下來，**回傳 function**
2. `call` or `apply` : 指定 `this`，**傳入參數並執行 function**

---


### 小測驗

```javascript
function log() {
  console.log(this);
}

var a = { name: 'a', log: log };
var b = { name: 'b', log: log };

log(); // => global or window
a.log(); // => a 本身
b.log.apply(a); // => a 本身


const newLog = b.log.bind(b); 
newLog.apply(a); // => b 本身
```