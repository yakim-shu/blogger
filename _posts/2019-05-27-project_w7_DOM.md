---
title: "[第七週] DOM - 操作 DOM 介面"
layout: post_project
description: ""
category: project
image: 
tags: [DOM, project_week7]
file_name: 2019-05-27-project_w7_DOM
---

JavaScript 在網頁上的操作不外乎以下三種，而這章會談到改變介面跟事件監聽的部分。
- 介面：如何改變介面
- 事件：如何監聽事件並作出反應
- 資料：如何跟伺服器交換資料

## 執行 JS - Node.js、瀏覽器的差別

兩個是不同的執行環境，所以能夠使用的語法也會有些微差異。

瀏覽器能做的是相對比較少一點，像是 Node.js 引入模組的 `require` 語法在瀏覽器上就無法使用。

## DOM 文件物件模型

DOM 全名為 Document Object Model

就是把 HTML 的**標記**（ Document ） 轉換成**物件**( Object )。

JS 可以操作物件，但不能直接操作頁面上的標籤，所以 DOM 就是瀏覽器幫 object 轉換成 => HTML 對應的標記，進而讓 JS 可以改變到頁面的元素。

> 所以 DOM 可以說是 JS 跟 HTML 溝通的橋樑。

---

## 選取 DOM 元素
以下是幾種 JavaScript 選取 DOM 元素的方法，最推薦用 `.querySelector()`。

- Tag
- Class Name
- ID （ 注意：Element 沒有 s，因為 ID 元素只能有一個 ）
- CSS 選擇器 
  - `querySelector`、`querySelectorAll` 
  - 最方便，就像寫 CSS 來選擇元素。
  - id 用 `#`、class 用 `.`、 tag 用 `<tag 本身>`、關係選擇器 `> + ~`
  - 但 `querySelector` 只會回傳第一個元素，如果想要選取到所有匹配到的元素，可以改用 `querySelectorAll`。

```javascript
const header = document.getElementsByTagName("header"); // tag
const cotainerBody = document.get​Elements​ByClass​Name("cotainerBody"); // class
const container = document.getElementById("container"); // id
const header_line_first = document.querySelector("#header p"); // 匹配到的第一個 #header p
const header_line_all = document.querySelectorAll("#header p"); // 所有匹配到的 #header p
```

> 注意：script 要放在元素後面，不然 JS 無法對元素操作

---

## 改變 CSS

有一種做法是直接改 Dom 上面的 `style` 屬性：

```javascript
element.style.background = 'red';
element.style.paddingTop = '10px'; 
element.style["padding-top"] = '10px';
```

但這種寫法很不彈性又非常累贅，所以一般來說都是寫好另外的 class，事件觸發時，再幫元素加上該狀態的 class：
```html
<style>
.active { 
  background: red;
}
</style>
element.classList.add('active')
```

### 增加、刪減 className

- 增加 : `.classList.add("active")`
- 移除 : `.classList.remove("active")`
- 開關 : `.classList.toggle("active")`
    - 沒有此 class → 新增、有此 class → 移除

### 抓取、改變文字或標籤內容
#### `.innerText` 
- 只抓取標籤裡的內容、不包含子標籤
- 只能寫入純文字
- `element.innerText = ` **`"content"`**

#### `.innerHTML` 
- 標籤裡的所有內容、包含子標籤 ( 如果裡面沒有子標籤，抓取內容就跟 `innerText` 一樣 ）
- 跟 `innerText` 不同的是，可以寫入標籤
- `element.innerHTML = ` **`"<div>content</div>"`** 

#### `.outerHTML` 
- 改變標籤內容、包括標籤本身
- `element.outerHTML =` **`"<new-element>...<div>content</div>...<new-element>"`**

### 加入、移除元素 

- `.appendChild()` 在最後面加入元素
- `.removeChild()` 移除該元素

``` javascript
const container = document.querySelector("#container");
const header_line_first = document.querySelector("#header p");
const newElement = document.createElement("div");
const newTextElement = document.createTextNode("123")
container.appendChild(header_line_first); // 加入其他位置的節點
container.appendChild(newElement);        // 加入新節點
container.appendChild(newTextElement);    // 加入文字節點
```

--- 

## 事件監聽 Event Listener

簡單來說就是指定畫面的某一元素，我們會監測當此元素發生 **「什麼事」** 、會再進行後續的處理，而我們可以要偵測指定「 事件 」是什麼、當它發生的這動作稱為 **「觸發」**。

最常見的應該是 click 點擊事件：

```javascript
element.addEventListener('click', function(){
    // ... callback => 當按鈕點擊時、要做的動作
});
```

所以上面這段程式碼可以解釋為：
- `element` 綁定一個 `click` 事件
- 當滑鼠點擊到 `element` ，即觸發了`element` 的 `click` 事件
- 會進行 callback function

### 回呼函式 Callback function
白話文就是先註冊一個事件，程式不會停下來等，繼續去跑其他地方，等事件被觸發了才執行 callback function 的內容。

### 事件資訊 `function(e)`

event 資訊會放在 callback function 裡面的**第一個參數**，通常都是取名 `event` 或簡寫 `e`，可以當成是一個「 物件 」，裡面有各種此事件的參數值，例如：
- `click` 點擊
    - `e.target` 點擊到的元素
    - `e.screenX` 滑鼠離視窗左邊的距離 
    - `e.screenY` 滑鼠離視窗上邊的距離 
- `keydown` 按下按鍵
    - `e.key` 按鍵號碼

    
### 表單事件處理 `onSubmit` 

當表單 `form` 中的 `submit` 按鈕 `click` 的時候，會有個預設行為，就是「 送出表單 」，預設是 GET 方法，並把參數帶入原網址送出，而 `submit` 事件是在**表單送出前會觸發**，用處是可以拿來驗證表單內容。

例如 `密碼` 跟 `確認密碼` 輸入的值不一樣的話，可以用 `e.preventDefault()` 來阻止表單送出。

```html
<body>
  <form class="form" action="">
    密碼：<input type="password" name="password1">
    確認密碼：<input type="password" name="password2">
    <button type="submit">送出表單</button>
  </form>
  <script>
    const form = document.querySelector('.form');
    form.addEventListener('submit', function(e){
      const pw1 = document.querySelector('input[name="password1"]');
      const pw2 = document.querySelector('input[name="password2"]');
      if (pw1.value !== pw2.value) e.preventDefault(); // 密碼不同、不送出表單
    });
  </script>
</body>
```

### 阻止預設動作 `e.preventDefault()`

阻止瀏覽器上的**特定元素**在**該事件預設的行為**，以下是比較常用的情況：

- `<form>` 的 `submit` - 阻止送出表單
- `<a>` 的 `click` 事件 - 阻止跳網址
- `<input>` 的 `keypress` 事件 - 阻止輸入按鍵
