---
title: "[第十週] week 7 複習 - DOM、Event"
layout: post_project
description: "事件代理就是利用 DOM 事件的冒泡特性，將子層元素的監聽事件綁定在父層元素上，主要有兩點好處：1. 如果有很多子元素，就不用一一綁定監聽事件。2. 動態新增進來的子元素，因為也是會冒泡到上層，就不怕沒綁定到監聽"
category: project
image: 
tags: [DOM, event, project_week10]
file_name: 2019-06-17-project_w10_review_DOM&EventListener
---

#### 你知道 JavaScript 跑在網頁上跟跑在 Node.js 上差在哪裡

兩個是不同的執行環境，所以能夠使用的語法也會有些微差異。

瀏覽器也只是一種軟體，能做的事相對比較少一點，像是 Node.js 引入模組的 `require` 語法在瀏覽器上就無法使用。

---

#### 你知道 DOM 是什麼

DOM 物件是「 瀏覽器 」幫 HTML 的「 節點 」轉換成 「 JS 可以操作的物件 」，等於是 HTML  跟 JS 之間溝通的橋樑。

---
#### 你知道如何用 JavaScript 操控 DOM 物件

使用 `element = document.querySelector(<element-name>)` 選取到 DOM 物件

##### 新增元素、文字節點
- `document.createElement("div");` 新增元素節點
- `document.createTextNode("123")` 新增文字節點

##### 加入、移除元素
- `.appendChild()` 在最後面加入元素
- `.removeChild()` 移除該元素

##### 改變內容
- `.innerText()` 裡面的文字內容
- `.innerHTML()` 裡面的文字及標籤內容
- `.outerHTML()` 自身與裡面的文字及標籤內容

---
#### 你知道怎麼用 JavaScript 更改元素的 style
- `.style.background = 'red'` 直接操縱樣式
- `.classList.add('<class-name>')` 新增class
- `.classList.delete('<class-name>')` 刪減class
- `.classList.toggle('<class-name>')` toggle class
- `.classList.contains('<class-name>')` 是否包含該 class

---
#### 你知道如何幫一個按鈕加上 event listener
```javascript
element.addEventLister('<event-name>', function(){
    // do something
})
```

常用 event :
- `click` 點擊
    - `e.target` 點擊到的元素
    - `e.screenX` 滑鼠離視窗左邊的距離 
    - `e.screenY` 滑鼠離視窗上邊的距離 
- `keydown` 按下按鍵
    - `e.key` 按鍵號碼
    
---
#### 你知道捕獲與冒泡是什麼

是 DOM 事件傳遞的順序，先捕獲 -> 自身 -> 冒泡。

我們可以夠過傳參數「 改變事件監聽的時機 」，但傳遞方式是保持不變的。

---
#### 你知道什麼是事件代理（delegation）

事件代理就是利用 DOM 事件的冒泡特性，將子層元素的監聽事件綁定在父層元素上，主要有兩點好處：
1. 如果有很多子元素，就不用一一綁定監聽事件
2. 動態新增進來的子元素，因為也是會冒泡到上層，就不怕沒綁定到監聽


---
#### 你知道 preventDefault 與 stopPropagation 的差異

##### preventDefault 阻止預設行為

`e.preventDefault()` 是阻止瀏覽器上的**特定元素**在**該事件預設的行為**，以下是比較常用的情況：

- `<form>` 的 `submit` - 阻止送出表單
- `<a>` 的 `click` 事件 - 阻止跳網址
- `<input>` 的 `keypress` 事件 - 阻止輸入按鍵

---

##### stopPropagation 阻止事件繼續傳遞

`e.stopPropagation()` 就是阻止事件的傳遞，換句話說，就是**不向上（ 或下 ）級**傳遞。

要改變「 在哪個階段加上監聽器 」，可以在 `.addEventListener` 方法加上第 3 個參數，為 boolean 值。

> 注意：**「 事件的傳遞方式 」並沒有改變，改變的是「 在哪個階段做監聽？ 」**因為不管在哪裡監聽它，都一樣是以先捕獲再冒泡，事件的傳遞方式是不會變的。

```javascript
.addEventListener('click', function (e) {
    // do something
}, <boolean>) // => 改變監聽階段
```
- `true => 捕獲 ; false => 冒泡`
- 不加參數的話，預設值是 `false`

---

假設畫面上有兩個元素（ 父子 ），我點了內層的子元素會觸發以下：

```
父元素 -> 捕獲
子元素 -> 自身
父元素 -> 冒泡
```

如果事件監聽的階段是在「 監聽冒泡 」（ 參數為 `false` ），加上 `e.stopPropagation()` 停止事件傳遞，點擊內層的子元素會觸發以下，會發現冒泡被阻止了：

```
父元素 -> 捕獲
子元素 -> 自身
```


如果事件監聽的階段是在「 監聽捕獲 」（ 參數為 `true` ），加上 `e.stopPropagation()` 停止事件傳遞，點擊內層的子元素會觸發以下，會發現子元素的自身跟冒泡都被阻止了：

```
父元素 -> 捕獲
```

因為當 **`父元素` 的捕獲階段 `(1)` 就被阻止**的話，根本傳不到 `子元素` ，所以 **`子元素` 的 `(2)(3)` 當然就沒有執行下去。**

