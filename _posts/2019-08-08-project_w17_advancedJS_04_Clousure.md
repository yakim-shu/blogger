---
title: "[第十七週] JavaScript 進階：從作用域鍊 ScopeChain 來理解 Closure 原理"
layout: post_project
description: ""
category: project
tags: [Closure, project_week17]
file_name: 2019-08-08-project_w17_advancedJS_04_Clousure
---

延續上一篇：[[第十七週] 面試愛考題：什麼是閉包 Closure 與實際應用](https://yakimhsu.com/project/project_w17_advancedJS_03_Clousure.html)

以下是一個 closure 範例：

```javascript
function createCounter() {
  var count = 0;
    return function addCounter() { //=> return addCounter
    count++;
    return count;
  }
  addCounter;
}

const counter = createCounter();
const total = counter();
console.log(total);
```

從上個章節我們知道 Closure 最神奇的地方就是，可以儲存狀態，意指幫某些變數的作用域保存起來，但照理來說，`createCounter()` 執行完畢後，應該就會被 JS 的垃圾回收機制回收掉啊，而到底是怎麼做到的呢？

首先我們來一步步拆解 Call Stack 中的步驟：

```javascript
1. globalEC 編譯
2. globalEC 執行
3. createCounterEC 編譯
4. createCounterEC 執行
    - createCounterEC 結束，pop 拋出 stack
5. counterEC 編譯 
    - counter scopeChain 仍保有 createCounterEC scope
6. counterEC 執行
```

而重點在於 `(3.) createCounter() 編譯階段` 時，裡面的 `addCounter` 默默被加上了一個屬性： `[[scope]]`。

---

### 幕後功臣： `[[scope]]` 屬性

每個 `function` 都有一個 `[[scope]]` 屬性（ 無法直接存取 ），裡面放上父層 `createCounter()` 的 scopeChain 的值，如此一來，`addCounter` 才可以存取到 `createCounter` 的作用域。

> 說簡單一點，當內層函式存取了外部函式的變數，就會產生 Closure

```javascript
// Call Stack 階段： (3) createCounterEC 編譯

addCounter.[[scope]] = [createCounterEC.AO, globalEC.VO]; // => 作用域被保存下來了

createCounterEC: {
    AO: {
        count: 0,
        addCounter: 0x22,
    }
    scopeChain: [createCounterEC.AO, globalEC.VO], // => 複製到 addCounter.[[scope]] 
    this: ...
}

counter.[[scope]] = [globalEC.VO]; // => 作用域被保存下來了

globalEC: {
    VO: {
        counter: 0x11,
        total: undefined,
    }
    scopeChain: [globalEC.VO], // => 複製到 counter.[[scope]]
    this: ...
}
```

所以這就是為什麼 `createCounterEC` 的 scopeChain 能有 `globalEC.VO`，就是**因為背後的 `[[scope]]` 屬性把他們串連起來，且再加上自身的 AO 所形成的 scopeChain**，才會有最後的 `[createCounterEC.VO, globalEC.VO]` 結果。 

> 注意：而 `createCounterEC.VO` 只會在主程式 `global` 結束之後，才會被真的釋放掉，所以要小心 **closure 要避免存放太大的資料**。


```javascript
// Call Stack 階段： (5) counter 編譯階段

createCounterEC.AO: {
    count: 0,
    ...
}

counterEC: { // => 其實就是 addCounter
    AO: {
        ...
    }
    // => 所以儘管 createCounterEC 已經結束 pop off 不在 call stack 裡面，但作用域仍然會被保存下來
    scopeChain: [counterEC.AO, createCounterEC.AO, globalEC.VO], 
    this: ...
}

globalEC: {
    VO: {
        counter: 0x11,
        total: undefined,
    }
    scopeChain: [globalEC.VO], 
    this: ...
}
```

---

## 作用域鍊 Scope Chain

所以如果在某 `function` 的 `AO` 裡找不到該變數，就會透過 `[[scope]]` 屬性去往上一層查找，而最後就像一條鍊子把每層都牽引住，這就被稱為作用域鍊 Scope Chain。

### 小小總結

所以閉包其實就是在編譯階段的時候，如果遇到 `function` 宣告，會自動幫 `function` 加上一個 `[[scope]]` 的屬性，而 **scopeChain 的構成就是參考這屬性的值、再加上自己的 `EC.AO`**。

當理解作用域鍊之後，便能夠很清楚知道閉包的原理，就是因為 **scopeChain 被保存下來了**，所以就算外部 `function` 在 Call Stack 中已經拿掉 Pop off，仍然可以保留外部 `function` 的狀態。

---


### 小補充： 由 Scope Chain 的形成機制來思考，在哪裡呼叫 function 會有影響嗎？

看看以下例子：

```javascript
var a = 'global';

function change() {
    var a = 1;
    test();
}

function test() {
    console.log(a);
}

change(); 
```

這例子很容易讓人誤以為答案是 `1`，但其實結果是 `global`。

理由很簡單，還記得 Scope Chain 是怎麼組成的嗎？ **`test()` 的 `[[scope]]` 屬性早在宣告 function 的時候就已經決定了，意思是說不管在哪裡呼叫 `test()`，都不會改變已成形的 Scope Chain。**

( 而以上例子，都是說明 ES6 之前的情況，變數的作用域的基本範圍就是 `function`。 )

---

## ES6 之後的作用域範圍： let & const 

在 ES6 之前的作用域是基於 `function`，而 ES6 之後的 let & const 宣告的作用域是基於區塊 `block {}`。

所以像是常用的 **`if else`、`for 迴圈` 都是一個 block，如果用 `let` 或 `const` 宣告變數，就會不同於 `var` 宣告，有一個區塊作用域**

Scope 的不同：
- `var`: function scope
- `let & const`: block scope