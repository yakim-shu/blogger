---
title: "[第十七週] JavaScript 進階：從 EC 來理解 Hoisting"
layout: post_project
description: ""
category: project
tags: [EC, VO, Hoisting, project_week17]
file_name: 2019-08-06-project_w17_advancedJS_02_Hoisting
---

## 什麼是 Hoisting？

要解釋 Hoisting 最簡單的例子如下，結果會輸出 `undefined`：

```javascript
console.log(a); // => undefined
var a = 5;
```

咦很奇怪吧？ 

明明變數 `a` 在第二行才進行宣告，怎麼不是拋出個 error 訊息: `a is not defined`，卻是輸出 `undefined` 呢？

這種奇怪的現象稱為 **`變數` / `函式` 提升 Hoisting**。

想要最簡單理解的話，可以想像成 `變數` 或 `function` 的宣告提升到作用域的最上方，然而只有理解到這層面的話，只會記得 Hoisting 就是個奇怪的現象。

但如果從原理開始了解，就不會再有 Hoisting 是個神秘黑魔法的想法，變成：「哦，就只是因為某某機制導致的結果麻！」

所以本篇著重於介紹 Hoisting 的原理、以及在 Hoisting 當中發生名稱衝突的優先順序。

---
## Hoisting 原理
當了解 EC 的編譯及執行這兩個階段，就不難理解為什麼會有 Hoisting 的現象。

#### → Phase 1 編譯 : 初始 EC 內容

Hoisting 的重點就是因為有 EC 編譯階段，**會先把 `變數` 跟 `function` 丟進 EC 的 VO 裡面**
- 變數會初始化成 `undefined`
- 而 function 則指向 `內容的記憶體位置`

```javascript
global EC: {
    VO: {
        a: undefined,
    }
}
```

#### → Phase 2 執行 : 根據 EC 的資源執行 function
所以在執行階段時，當我們要查詢變數 `a`，因為 EC 的 VO 裡面已有宣告 `a` 的紀錄，才會造成輸出結果為 `undefined`。

#### 結論： Hoisting 不是把變數拉上去的黑魔法，而是一個現象
這樣「 **看似把 `變數` & `function` 的宣告提到（ 作用域 ）最上面的動作，稱為 Hoisting** 」，然而我們要是了解 JS 執行的過程，就可以很清楚解釋 Hoisting 的原理。

---

### 同樣名稱的`arguments` & `Function` & `var`，如何判斷最終提升的結果？

#### ☞ Function VS var

`function` 提升的**優先度**會比`變數`高，跟宣告順序沒關係：

（當然後面宣告的 function 會蓋過前面的 function）
```javascript
function test() {
    console.log(a);
    function a() {};
    var a = '1';
}

test(); // => [Function: a]，是 Function 而非 undefined
```

#### ☞ arguments VS var

```javascript
function test(a) {
    console.log(a);
    var a = 456;
}
test(123); // => 123
```

`test()` 裡面已經有 `a` 變數了，所以變數 `a => undifined` 的提升 Hoisting 等於沒有用，但如果是直接賦值，等於是在執行階段改變了 `a` 的值，則當然會影響到：

```javascript
function test(a) {
    var a = 456;
    console.log(a);
}
test(123); // => 456
```

#### ☞ Function VS arguments

```javascript
function test(a) {
    console.log(a);
    function a() {
        return 'a function';
    }
}
test(123); // => [Function: a]
```

#### 小補充： 那 `let` & `const` 呢？

其實這兩種變數宣告方式也是有 Hoisting 的，只是方式不太一樣，`let` & `const` 同樣會把變數提上去，**但不會設成 `undefined`，且在賦值之前無法存取**，這範圍稱為 Temporal Dead Zone (TDZ)

### 結論

還記得上一篇：[打好基礎的第一步，從了解什麼是 EC 開始](https://yakimhsu.com/project/project_w17_advancedJS_01_EC.html) 提過 VO 初始內容的優先順序及覆蓋方式，所以也就能夠推斷出，當發生名稱衝突時、Hoisting 的優先度（ 由高至低 ）：

1. 函式宣告 `function declaration`
2. 函式參數 `function arguments`
3. 變數宣告 `variable`


---

### Hoisting 的目的

function 提升是有其必要的，可以解決這種 `A call B` & `B call A` 的互相呼叫 function 的問題。

以下範例取自 [JavaScript系列文章：变量提升和函数提升](https://www.cnblogs.com/liuhe688/p/5891273.html)：

```javascript
// 验证偶数
function isEven(n) {
    if (n === 0) {
        return true;
    }
    return isOdd(n - 1);
}

// 验证奇数
function isOdd(n) {
    if (n === 0) {
        return false;
    }
    return isEven(n - 1);
}

console.log(isEven(2)); // true
```

---

### ☞ 名詞小補充： LHS 賦值 & RHS 查詢值 

以下內容完全取自：[我知道你懂 hoisting，可是你了解到多深？](https://github.com/aszx87410/blog/issues/34)

```javascript
var a = 10
console.log(a)
```

上面這兩行有個差異，第一行的時候我們只需要知道「a 的記憶體位置在哪裡」就好，我們不關心它的值是什麼。

而第二行則是「我們只關心它的值是什麼，把值給我就好」，所以儘管兩行裡面都有a，但你可以看出來他們所要做的事情是不一樣的。

第一行的 `a` 我們叫它 LHS（Left hand side）引用，第二行叫它 RHS（Right hand side）引用，這邊的 left 跟 right 指的是相對於等號的左右邊，但用這種方式理解的話其實不夠精確，因此像下面這樣記就好：

- **LHS**：請幫我去查這個變數的位置在哪裡，因為我要對它**賦值**。
- **RHS**：請幫我查詢這個變數的值是什麼，因為我要**用這個值**。


---

## 小測驗

以下有個小測驗，可以來檢驗對 Hoisting 的熟悉程度：

```javascript
var a = 1;
function test(){
  console.log('1.', a);
  var a = 7;
  console.log('2.', a);
  a++;
  var a;
  inner();
  console.log('4.', a);
  function inner(){
    console.log('3.', a);
    a = 30;
    b = 200;
  }
}
test();
console.log('5.', a);
a = 70;
console.log('6.', a);
console.log('7.', b);
```


輸出結果如下：

```javascript
1. undefined
2. 7
3. 8
4. 30
5. 1
6. 70
7. 200
```

參考資料：
- [我知道你懂 hoisting，可是你了解到多深？](https://github.com/aszx87410/blog/issues/34)