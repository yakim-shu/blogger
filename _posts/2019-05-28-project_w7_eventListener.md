---
title: "[第七週] DOM - 事件傳遞機制：捕獲與冒泡、事件代理"
layout: post_project
description: ""
category: project
image: 
tags: [eventListener, bubbling, project_week7]
file_name: 2019-05-28-project_w7_eventListener
---

事件是會被傳送上去的，最簡單的範例如下：
```html
<div class="outer">
    outer
    <div class="inner">
      inner
      <a href="#" class="btn">btn</a>
    </div>
</div>
<script>
addEvent('.outer');
addEvent('.inner');
addEvent('.btn');

function addEvent(className) {
  document.querySelector(className)
  .addEventListener('click', function () {
    console.log(className);
  })
};
</script>
```
頁面有三個元素，由外至內分別是：`outer`、`inner`、`btn`，三個元素都分別監聽 `click` 事件。

但奇怪的事情來了，當我點擊最內層元素 `btn`，不只 `btn` 的 `click` 事件被觸發，連上兩層的元素都被觸發了，這種詭異的現象其實是「 冒泡事件 」。

![螢幕快照 2019-05-27 下午6.47.07](https://i.imgur.com/qBPcmlZ.jpg)

---

## 捕獲與冒泡

再繼續深入下去，根據 [DOM 的事件傳遞機制：捕獲與冒泡](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/) 指出，事件傳遞機制實際上分成 3 階段：
```
// PhaseType
const unsigned short      CAPTURING_PHASE                = 1; 捕獲
const unsigned short      AT_TARGET                      = 2; 元素本身
const unsigned short      BUBBLING_PHASE                 = 3; 冒泡
```

![](https://blog.techbridge.cc/img/huli/event/eventflow.png)

關於這些事件的傳遞順序，只要記住兩個原則就好：
- 先捕獲，再冒泡
- 當事件傳到 target 本身，沒有分捕獲跟冒泡

### 改變事件的傳遞方式

要改變事件的傳遞方式，可以在 `.addEventListener` 方法加上第 3 個參數，為 boolean 值。

- `true => 捕獲 ; false => 冒泡`
- 不加參數的話，預設值是 `false`

改寫剛剛的範例，假如把剛剛的三個元素都加上兩種傳遞模式：
```javascript
function addEvent(className) {
  document.querySelector(className)
  .addEventListener('click', function (e) {
    // e.eventPhase` 為事件階段 (1) 捕獲、(2) 自身、(3) 冒泡
    console.log(className, '冒泡', e.eventPhase); 
  }, false)
  document.querySelector(className)
    .addEventListener('click', function (e) {
      console.log(className, '捕獲', e.eventPhase);
    }, true)
};
```

點擊最內層的 `btn` 會看到順序如下 `(1) > (2) > (3)`：

```javascript
.outer 捕獲 1
.inner 捕獲 1
.btn 冒泡 2 // 看底下說明
.btn 捕獲 2 // 看底下說明
.inner 冒泡 3
.outer 冒泡 3
```

> 注意：

- 中間 `btn` 會「 先冒泡再捕獲 」是因為冒泡的綁定寫在更前面。
- 中文註釋這樣寫可能會造成混淆，因為其實是在階段 `(2) 自身`、非冒泡也非捕獲

---

## 阻止事件傳遞 `e.stopPropagation()`

`e.stopPropagation()` 就是阻止事件的傳遞，換句話說，就是不向上（ 或下 ）級傳遞。

> 請注意，根據不同的指定傳遞方式，結果也會有所不同

以剛剛的例子來看，為了方便觀看，綁定的順序改成先「 捕獲 」 再 「 冒泡 」，也把中文去掉以免造成混淆。

### ☞ 阻止冒泡

在冒泡的流程上加了 `e.stopPropagation()`：

```javascript
function addEvent(className) {
  document.querySelector(className)
    .addEventListener('click', function (e) {
      console.log(className);
    }, true)
  document.querySelector(className)
  .addEventListener('click', function (e) {
    console.log(className);
    e.stopPropagation(); // 阻止冒泡事件傳遞
  }, false)
};
```
同樣的點擊 `btn`，會輸出以下，可以看得出來捕獲事件 `(1)(2)` 繼續傳遞，但冒泡 `(3)` 被阻止了：
```javascript
.outer 1
.inner 1
.btn 2
.btn 2
```

### ☞ 阻止捕獲

接下來，換阻止捕獲事件看看：

```javascript
function addEvent(className) {
  document.querySelector(className)
    .addEventListener('click', function (e) {
      console.log(className, '捕獲');
      e.stopPropagation(); // 阻止捕獲事件傳遞
    }, true)
  document.querySelector(className)
  .addEventListener('click', function (e) {
    console.log(className, '冒泡');
  }, false)
};
```

點擊 `btn` 卻是輸出以下結果：

```javascript
.outer 1
```

發現只傳到 `.outer` 就停止了！

其實也蠻合理的，還記得剛剛的捕獲、冒泡流程圖嗎？
1. 捕獲 `CAPTURING_PHASE`
2. 元素本身 `AT_TARGET`
3. 冒泡 `BUBBLING_PHASE`

事件傳遞是照以上順序的，而 `e.stopPropagation` 會阻止後續的傳遞，所以當 **`.outer` 的捕獲階段 `(1)` 就被阻止**的話，根本傳不到 `btn` ，所以 **`btn` 的 `(2)(3)` 當然就沒有執行下去。**

那如果是點擊 `outer` 元素呢？ 則會輸出：
```javascript
.outer 2
.outer 2
```

因為 `.outer` 上級並沒有元素可以傳遞，所以也沒有捕獲階段 `(1)`，兩次都輸出都是在 `(2): target phase`。

由此可以觀察到一點有趣的事情。

> 如果該元素是最上層的元素，事件傳遞方式指定為捕獲，那底下所有的元素事件傳遞都會被停止。

---

### 實際應用： 停止頁面上所有元素的預設動作

換句話說，如果我想要阻止頁面上所有的 `click` 事件（ 包括 `<a>` 預設的動作 ），可以在 `window` 以捕獲方式做阻止：

```javascript
window.addEventListener('click', function (e) {
  e.preventDefault(); // 停止預設功能
  e.stopPropagation(); // 停止後續傳遞
}, true) // 指定為捕獲

// 底下的事件傳遞全都被阻止了
function addEvent(className) {
  document.querySelector(className)
    .addEventListener('click', function (e) {
      console.log(className, '捕獲', e.eventPhase);
    }, true)
  document.querySelector(className)
  .addEventListener('click', function (e) {
    console.log(className, '冒泡', e.eventPhase);
  }, false)
};
```

---

### `e.stopImmediatePropagation()` 阻止後續相同事件

一個元素可以綁定多個事件，例如以剛剛的例子，我在 `btn` 上面又多綁了 2 個 `click`，分別輸出 `第一個 click` & `第二個 click`，全部不加參數、以預設的冒泡方式傳遞：

```javascript
document.querySelector('.btn')
.addEventListener('click', function (e) {
  console.log('第一個 click', e.eventPhase);
  e.stopImmediatePropagation();
});

document.querySelector('.btn')
.addEventListener('click', function (e) {
  console.log('第二個 click', e.eventPhase);
});
```

會輸出以下，可以看得出來傳遞順序是 `btn` 上面綁的三個 eventLister `(2)` > 冒泡到上層 `(3)`。

```javascript
.btn 2
第一個 click 2
第二個 click 2
.inner 3
.outer 3
```

那如果我在第一個 `click` 加上 `e.stopProgatation` 呢？ 
```javascript
document.querySelector('.btn')
.addEventListener('click', function (e) {
  console.log('第一個 click', e.eventPhase);
  e.stopPropagation(); // 停止事件傳遞
});
```

會輸出以下，看的出來冒泡 `(3): bubbling` 被阻止了，但兩個 click 都會觸發。
```javascript
.btn 2
第一個 click 2
第二個 click 2
```

原因是這兩個事件都在 `(2): target phase` 到階段， `e.stopPropagation()` 阻止的只有冒泡階段，那如果我想要停止 `(2)` 階段後續的 click 事件呢？

可以改成 `e.stopImmediatePropagation()`，立即停止後續的自身 & 冒泡 `(2)(3)` 階段：
```javascript
document.querySelector('.btn')
.addEventListener('click', function (e) {
  console.log('第一個 click', e.eventPhase);
  e.stopImmediatePropagation(); // 立即停止事件傳遞 (包括 2: target phase)
});
```
就會只剩下：
```javascript
.btn 2
第一個 click 2 // 是加在這裡，所以才會輸出兩個
```

結論： 當元素有**綁定多個同樣的事件時**，可以用 `e.stopImmediatePropagation()` 阻止所有後面的綁定。

---

## 新手易混肴問題

### ☞ 多個元素綁定 EventListener

假如畫面中有 `n` 個按鈕，分別顯示 `1, 2, 3, 4...` 個數字。

而今天要做的功能是「 當我按下按鈕，就顯示該按鈕的數字 」

首先第一步應該會想到用迴圈幫全部按鈕加上事件監聽，所以寫成以下：

```javascript
const btnGroup = document.querySelectorAll('.btn');
for (var i = 1; i <= btnGroup.length; i += 1) {
  btnGroup[i].addEventListener('click', function (e) {
    console.log(i);
  })
}
```

但當我這麼做的時候，會發現不管按哪個按鈕，都是輸出 `btnGroup.length` 的數值，這是為什麼呢？

首先你要知道，接在 `click` 事件後的 **`function(e)` 是一個 callback function**，JS 雖然已經跑完整段程式，但 callback **執行的時機是要等事件觸發**。

所以當我觸發 `click` 事件時，迴圈已經跑完了，而 `i` 是用 `var` 宣告 => **`全域變數`**，所以 `i` 的數值早已變成 `btnGroup.length` 了。

### 改善方法

- 將迴圈初始值 `i` 改成用 `let` 宣告，變成 `區域變數`

```javascript
for (let i = 1; i <= btnGroup.length; i += 1) {
```

拆解開來，可以看成是這樣運作
```javascript
// 跑第一遍迴圈
{
  let i = 1;
  btnGroup[i].addEventListener('click', function (e) {
    console.log(i);
  })
}
// 跑第二遍迴圈
{
  let i = 2;
  btnGroup[i].addEventListener('click', function (e) {
    console.log(i);
  })
}

...
```
> 以上 Demo 感謝 Huli 詳解

- 在元素上新增自定義屬性 `data-value`，直接抓屬性值

```html
<button class="btn" data-value="1">1</button>
<button class="btn" data-value="2">2</button>
<script>
  const btnGroup = document.querySelectorAll('.btn');
  for (let i = 0; i < btnGroup.length; i += 1) {
    btnGroup[i].addEventListener('click', function (e) {
      console.log(e.target.getAttribute('data-value'));
    })
  }
</script>
```

---

### ☞ 動態增加的元素、事件代理 `Event delegation`

接續上一個例子，順利地幫每個按鈕加上 `EventListener` 、大功告成！

咦不對，又發現另外兩個問題：

1. 如果按鈕數量越來越龐大，假如變成 1000 個按鈕好了，難道要迴圈 1000 次幫每個按鈕都加上 `EventListener` 嗎？ 光想就非常沒有效率，尤其是 callback function 的內容其實很相似
2. 如果是**動態新增**的元素呢？（ 例如用 `appendChild` 的元素 ）要怎麼幫它綁定 `EventListener`？

別擔心，以上煩惱都可以用之前學的 **「 事件冒泡 」** 的概念去解決。

還記得事件傳遞預設是會冒泡的吧！改寫一下 HTML 結構，幫所有按鈕用 `.outer` 包起來。
```html
<div class="outer">
  <button class="btn_add" >add</button>
  <button class="btn" data-value="1">1</button>
  <button class="btn" data-value="2">2</button>
</div>
``` 
仔細想想，所有的 `button` 只要觸發 `click` 事件，都會向上冒泡傳遞到上層 `.outer`，觸發 `.outer` 的 `click` 事件，那就直接將 `EventListener` 綁定在上層的 `.outer` 身上就解決啦！

而且因為 `click` 事件是綁在上層的 `.outer` ，所以也不用擔心 `.appendChild()` 新增的子元素沒有綁定到 eventListener。

```javascript
document.querySelector('.outer').addEventListener('click', function (e) {
  console.log(e.target.getAttribute('data-value'));
})
```

> 現在就真的大功告成了 🙌

而像這樣把 `button` 的 `click` 綁定在上層的 `.outer` 元素上，就叫做 **事件代理 Event delegation**。

參考資料：
- [DOM 的事件傳遞機制：捕獲與冒泡](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
  - Huli 寫的，大推！