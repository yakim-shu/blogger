---
title: "[第十七週] JavaScript 進階：打好基礎的第一步，從了解什麼是 EC 開始"
layout: post_project
description: ""
category: project
tags: [EC, VO, scope, project_week17]
file_name: 2019-08-05-project_w17_advancedJS_01_EC
---

### 破除誤解： JavaScript 是直譯語言，代表不會經過編譯嗎？


#### 什麼是直譯語言 (Interpreted language)

以下取自 [編譯語言 VS 直譯語言](https://medium.com/@totoroLiu/%E7%B7%A8%E8%AD%AF%E8%AA%9E%E8%A8%80-vs-%E7%9B%B4%E8%AD%AF%E8%AA%9E%E8%A8%80-5f34e6bae051) 的說明：
> 一種程式語言的類型，不同於編譯語言，直譯語言在**執行時會一行一行的動態將程式碼直譯 ( interpret ) 為機器碼**，並執行。

> 直譯式語言則是必須依賴一個執行環境 ( Execution Context )，語言可用的功能由這個執行環境提供，例如 JavaScript 只能使用瀏覽器提供的功能，它無法獨立執行 ( 看起來像獨立執行，實際上卻是系統自動在背後建立執行環境，如 HTML Application )。

從前認為 JS 就是不需要編譯的語言，像在上 CS50 時使用 C 語言，所以檔案 `a.c` 必須先編譯成 `a.h`，最後真正執行的是 `a.h` 的檔案，然而寫 JavaScript 並沒有經過 **開發者主動編譯** 這個動作，我以為程式就是一行一行的跑的，可以說是深信不疑。

但之前學到 Hoisting 的行為層面，其表現明顯「 違反逐行執行 」的觀念，但就只有想說大概是中間有什麼黑魔法在運作吧，並沒有懷疑 JavaScript 是直譯語言這件事，也沒有想過要去深究原因，只能說 「 對於不知道的事，連不知道的事實都不知道呢。 」

由此可推理出，JS 既然不是逐行執行，那是怎麼跑的呢？ **編譯 => 執行**。

而本章的重點擺在**編譯階段做了哪些事情**。


參考資料：
- [秒懂！JavaSript 執行環境與堆疊](https://medium.com/%E9%AD%94%E9%AC%BC%E8%97%8F%E5%9C%A8%E7%A8%8B%E5%BC%8F%E7%B4%B0%E7%AF%80%E8%A3%A1/%E6%B7%BA%E8%AB%87-javascript-%E5%9F%B7%E8%A1%8C%E7%92%B0%E5%A2%83-2976b3eaf248)
- [編譯語言 VS 直譯語言](https://medium.com/@totoroLiu/%E7%B7%A8%E8%AD%AF%E8%AA%9E%E8%A8%80-vs-%E7%9B%B4%E8%AD%AF%E8%AA%9E%E8%A8%80-5f34e6bae051)

---

## Execution Context 執行環境

JavaScript 所有的程式碼都要在 `執行環境 Execution Context` 中執行，可以**想像成一個虛擬的空間，任何在裡面運行的程式碼所需要的資源，都會被丟進來**。

而當我們要準備運行 JavaScript 程式碼時，第一個 `Execution Context` 就是全域物件的 `Execution Context`，簡稱 `Global EC`。


## Execution Context 的內容有哪些？

你可以想像成 JavaScript 的物件，裡面的內容會有 `VO` & `scopeChain` & `this`：

```javascript
EC: {
    Variable Object (VO): {
        arguments: {
            ... 
        },
        FD: <reference to function>
        <variable>: <value>,
    },
    scopeChain: [...],
    this: ...
}
```

### ☞ VO / AO: Variable Object
紀錄物件的 `arguments` & `variable` & `function`。

只有在 globalEC 裡的稱為 VO，而 functionEC 裡會叫 AO，其差異就是創建 AO 時會初始化一欄 `arguments: <ArgO>`
##### 1. 函式參數 `function arguments`
- 如果沒有參數丟進來，則會是 `undefined`
    
##### 2. 函式宣告 `function declaration` ( FD ) 
- 優先級最高
- 所有函式宣告丟進來，值設為 `記憶體位置`
- 如果有重複的名稱 => 會覆蓋過去（ 後蓋前，意思是同名的 `arugments` 也會被蓋過去 ）

##### 3. 變數宣告 `variable` : var
- 優先級最低
- 所有變數宣告丟進來，初始值為 `undefined`
- 如果名稱重複 => 則忽略 ( 無法覆蓋 `arugments` & `FD` )
  
> 注意： VO 的概念在 **ES5 之後被 lexical environments 模型取代**
      
### ☞ ScopeChain
- 每一層的 `Child_EC.VO` 會相加上一層的 `Parent_EC.VO`，所以透過 scopeChain 會用陣列紀錄該 EC 的所有 scope ，也正因為如此，function 才可以拿到外部的變數
  
----

### 特別補充： 全域物件 Global Object

根據 [ECMA-262-3 in detail. Chapter 2. Variable object.](http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/) 的說明指出：

再進入任何 EC 前，global object 就會被創建，而 global object 的 VO 其實就是指向自己 (`global`)。

global object 裡面包含了這些東西：

```javascript
global = {
  Math: <...>,
  String: <...>
  ...
  ...
  window: global
};
```

而原來 window 也是指向 global，然而我們不能直接寫 `global` 去進行操作，那要怎麼存取呢？

在網頁的環境下，可以直接寫 `window` or `this`（ 全域狀態 ）

所以以下是完全等價的：

```javascript
String(10); // means global.String(10);
 
// with prefixes
window.a = 10; // === global.window.a = 10 === global.a = 10;
this.b = 20; // global.b = 20;
```


---

### 特別補充： 函式宣告 VS 函式表達式

還記得在創建 EC 的時候，會把函式宣告 `function declaration` 丟進 VO/AO 嗎？

這邊要注意的一點是，函式表達式並不會被丟進來。

**`函式宣告 Function Declaration` 跟 `函式表達式 Function Expression` 是不一樣的，只有宣告會被丟進 VO/AO**

以下取自 [ECMA-262-3 in detail. Chapter 2. Variable object.](http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/) 的範例：

```javascript
function test(a, b) {
  var c = 10;
  function d() {}
  var e = function _e() {};
  (function x() {});
}
  
test(10); // call
```

這將是 test 的 AO :

```javascript
AO(test) = {
  a: 10,
  b: undefined,
  c: undefined,
  d: <reference to FunctionDeclaration "d">
  e: undefined
};
```

有沒有發現 function `_e` & `x` 並沒有在 AO！

因為注意看這**兩個 function 都不是函式宣告 FunctionDeclaration**，而是函式表達式 FunctionExpression

但其實 `_e` 還是存在記憶體裡面，唯一的原因是因為他被存在變數 `e` 上面，但是 `x` 從頭到尾都不存在任何 `AO/VO`，所以我們不管在哪個位置試圖要 call function `x` 都會拋出錯誤，「 沒有被指向任何變數 」的**函式表達式**，呼叫的時機只有在創建的當下以及遞迴。


緊接著再看一個好玩的例子，在永遠不會執行的 else 區塊，VO 裡面會有 `b` 嗎？ 

答案是有的，在編譯階段，`b` 還是會被丟進 VO 、並初始化成 `undefined`，所以當你執行以下這段範例時不會拋出錯誤。

```javascript
if (true) {
  var a = 1;
} else {
  var b = 2;
}
 
alert(a); // 1
alert(b); // undefined, but not "b is not defined"
```
