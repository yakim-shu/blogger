---
title: "[第八週] 交換資料 - 表單、Ajax、XMLHttpRequest"
layout: post_project
description: "用 node.js 呼叫 API 與在網頁上呼叫的根本差異是什麼？最主要的差別在 Node.js 是直接向 Server 發送 request。
而瀏覽器上的 JS 則是透過 「 瀏覽器 」去發送 request 給 Server，Server 也要透過「 瀏覽器 」接收 response 回來，所以會有許多限制。"
category: project
tags: [Ajax, Form, XMLHttpRequest, project_week8]
file_name: 2019-06-06-project_w8_Ajax_Form
---

### 用 node.js 呼叫 API 與在網頁上呼叫的根本差異是什麼？

最主要的差別在 Node.js 是直接向 Server 發送 request。

而瀏覽器上的 JS 則是透過 「 瀏覽器 」去發送 request 給 Server，Server 也要透過「 瀏覽器 」接收 response 回來，所以會有許多限制。

## 傳送資料的第一種方式：表單 form

為最基本、最陽春的方式。

流程：
- 瀏覽器發送一個 **帶上參數** 的 request 到**一個新的頁面**
- 然後將 Server 回傳的 「 response 渲染在頁面上 」

```html
<form method="POST" action="https://google.com">
    ...
</form>
```

所以其實跟 JS 並沒有太大關係，是瀏覽器提供的一種發送 request 的方法。

- GET 方法：參數就會直接被放在 URL 上面
- POST 方法： 參數會放在 request 的 body 裡面

但這種傳送資料的方式有個缺點，就是**一定會換頁**，所以就要來介紹另一種傳送資料的方式： Ajax

---

## 傳送資料的第二種方式：Ajax

全名為 Asynchronous JavaScript and XML

透過 JS 來傳送資料，就可以解決表單的換頁問題

流程：
- 瀏覽器發送一個 **帶上參數** 的 request 到 **一個新的頁面**
- 然後將 Server 回傳的 **「 response 傳給頁面上的 JS 」**

以下內容大部分取自文章： [輕鬆理解 Ajax 與跨來源請求](https://blog.techbridge.cc/2017/05/20/api-ajax-cors-and-jsonp/) 

任何非同步交換資料的方式，重點在於 Asynchronous 這個單字，非同步。


在講什麼是非同步之前，就要先來提一下什麼是同步。你原本寫的 JavaScript 就幾乎都是同步執行的。意思是他執行到某一行的時候，會等這行執行完畢，才執行到下一行，確保執行順序。

非同步是什麼意思呢？就是執行完之後就不管它了，不等結果回來就繼續執行下一行：

```javascript
// 非同步例子
// 假設有個發送 Request 的函式叫做 sendRequest
var result = sendRequest('https://api.twitch.tv/kraken/games/top?client_id=xxx');
  
// 上面 Request 發送完之後就執行到這一行，所以 result 不會有東西
// 因為 Response 根本沒有回來
console.log(result);
```

這邊需要特別注意的是 **「非同步的 Function 不能直接透過 return 把結果傳回來」**，

為什麼？因為像上面這個例子，它發送 Request 之後就會執行到下一行了，這個時候根本就還沒有 Response，是要回傳什麼？

以下是我的理解：

用 `return` 試圖返回 非同步 function 就等於是 ：
>「 剛點完餐就立刻問老闆： 『 阿我的餐勒？』」 

 當然不會有東西啊，阿不然剛剛拿到的號碼牌（ callback function ） 是拿假的嗎？

當非同步的操作完成時（ 拿到號碼牌 ），就可以呼叫這個 Function，並且把資料帶進來。

---



### XMLHttpRequest
是一個 JavaScript 提供的 API，主是要拿來發送 Request 與接收 response

#### 實例物件: `new XMLHttpRequest`
- 一個瀏覽器定義好的物件，要使用得 `new` 出實例

```javascript
const request = new XMLHttpRequest;
```

#### 設定 Request ： `request.open()`
- 設定 `Method` & `URL` & `同步 | 非同步`  & `使用者` & `密碼`
- 前兩項為必填、後三項可選
    
```javascript
request.open(<method>, <url>, <async:b>, <user:s>, <password:s>)
```

#### 設定 Request header : `request.setRequestHeader()`
- 設定 Request 表頭

```javascript
request.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded'); // => 設定 Content-Type
request.setRequestHeader('Client-ID', token); // => 設定 client-ID
```

#### 監聽 load 事件 : `request.onload`
- 當瀏覽器拿到 response 時，會觸發 `onload` 事件綁定的 callback function
    - 如果是用 `addEventListener` 方法監聽，事件名稱是 `load`

```javascript
request.onload = function() { 
    // => callback
}
```

#### 發送 Request： `request.send()`
```javascript
const request = new XMLHttpRequest;
request.onload = function() {
  if (request.status >= 200 && request.status <= 4000) {
    console.log(request.status);
    console.log(request.responseText);
  } else {
    console.log(request.state, request.responseText);
  }
}

request.onerror = function() {
  console.log("error");
}

request.open("GET", "https://reqres.in/api/users");
request.send();
```

參考資料：
- [XMLHttpRequest — JavaScript 發送 HTTP 請求 (I)](https://notfalse.net/29/xmlhttprequest)
