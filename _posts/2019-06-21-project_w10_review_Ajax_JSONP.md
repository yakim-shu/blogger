---
title: "[第十週] week 8 複習 - Ajax、JSONP"
layout: post_project
description: "事件代理就是利用 DOM 事件的冒泡特性，將子層元素的監聽事件綁定在父層元素上，主要有兩點好處：1. 如果有很多子元素，就不用一一綁定監聽事件。2. 動態新增進來的子元素，因為也是會冒泡到上層，就不怕沒綁定到監聽"
category: project
image: 
tags: [Ajax, JSONP, project_week10]
file_name: 2019-06-21-project_w10_review_Ajax_JSONP
---

#### 你知道什麼是 API

全名為 Application Programming Interface，中文翻譯為「應用程式介面」

> 簡單來說就是方便溝通、交換資料的管道。

以網頁用的 Web API 來舉例，就像是對方定義好了插座的規格，我們**只要照著規格、把形狀合適的插頭插上去，就可以得到資料**，而你不會知道資料是怎麼來的，就像你不必關心電是怎麼傳過來的。

---
#### 你知道什麼是 Ajax

全名為 Asynchronous JavaScript and XML

透過 JS 用 「 非同步 」的方法來傳送資料，就可以解決傳統的表單換頁問題。

當發送一個 request 之後，以下是同步與非同步（ 異步 ）的差別：

- 非同步：
    - 頁面的其他程式並不會停下來，等拿到 response 才繼續執行 callback
- 同步：
    - 在拿到 response 之前，使用者不能對該頁面做任何的存取，頁面會被阻塞。

Ajax 流程：
- 瀏覽器發送一個 **帶上參數** 的 request 到 **一個新的頁面**
- 然後將 Server 回傳的 **「 response 傳給頁面上的 JS 」**

---
#### 你知道從網頁前端呼叫 API 與在自己電腦上寫程式呼叫的差異
在 node.js 環境下發 request，並沒有瀏覽器這層限制，跨域問題只存在於透過瀏覽器發送的 request。

---
#### 你知道什麼是同源政策（Same-origin policy）
只要瀏覽器發的 request 位置 跟 Server 端的 位置是不同源的（不同 domain ）， request 就會被瀏覽器擋掉。
  
相同 domain 就是同源，值得注意的是 http & https 是分開的：

詳細定義請看 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy) 的說明

---
#### 你知道如何存取跨網域的資源（CORS）
只要 **Server 端在 response 的 header** 加上一行，就可以允許不同網域的 request 存取

```javascript
access-control-allow-origin: *
``` 

除了這個 Header 以外，其實還有其他的可以用，例如說 `Access-Control-Allow-Headers` 跟 `Access-Control-Allow-Methods`，就可以定義接受哪些 Request Header 以及接受哪些 Method。

---
#### 你知道什麼是 JSON
> 比起 XML 更輕量，**為現代最普遍、常用的資料格式**。

- 全名為 JavaScript Object Notation
- 儘管來源自 JavaScript 的子集，但其實廣泛運用在其實程式語言，幾乎所有與網頁開發相關的語言都有JSON函式庫。
- 乍看跟 JavaScript 的物件很像，但 `key` 值必須要用雙引號 `"key"` 包起來
- JSON 字串可以包含 **陣列 ( 用中括號 `[ ]` )** 以及 **物件 ( 用大括號 `{ }` )**

---
#### 你知道什麼是 JSONP 及其原理
簡單來說 JSONP 就是 **利用 `src` 不受同源限制的特性，直接載入一隻帶參數的js，當作是發 request，用一個 `function` 包裝起來。**

實務上在操作 JSONP 的時候，Server 通常會提供一個 callback 的參數讓 client 端帶過去。
但 JSONP 的缺點就是你要帶的那些參數「 永遠都只能用附加在網址上的方式（GET）帶過去，沒辦法用 POST 」。

很重要的一點是，要使用 JSONP 傳送資料，也要 **Server 端有提供 JSONP 的方法**（ 意指用 callback function 包起來 ）才行，不然回傳的 Response 就只是字串而已，沒有辦法取得資料。


---
#### 你知道 Fetch 的基本使用方法

- `fetch()` 會使用 ES6 的 Promise 作回應
  - `.then()` 作為下一步
  - `.catch()` 作為錯誤回應 (404, 500…)

fetch 後方會接 then()，這是 Promise 的特性，資料取得後可在 then 裡面接收。return response.json(); 的資料則會傳到下一個 then()。

[From MDN - Using Fetch](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API/Using_Fetch)

`fetch()` 第二個參數是選用的，可以傳送一個 init Object 來設定 request。

```javascript
postData('http://example.com/answer', {answer: 42})
  .then(data => console.log(data)) // JSON from `response.json()` call
  .catch(error => console.error(error))

function postData(url, data) {
  // Default options are marked with *
  return fetch(url, {
    body: JSON.stringify(data), // must match 'Content-Type' header
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, *omit
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, cors, *same-origin
    redirect: 'follow', // manual, *follow, error
    referrer: 'no-referrer', // *client, no-referrer
  })
  .then(response => response.json()) // 輸出成 json
}
```


[From 鐵人賽: ES6 原生 Fetch 遠端資料方法](https://wcc723.github.io/javascript/2017/12/28/javascript-fetch/)

使用 `fetch()` 做 `POST` 時，由於它並沒有向一些框架一樣包那麼徹底，所以有些地方還是需要做調整，其中 `body` 所送出的資料必須先轉純字串後才能送出，以下範例就是一個簡單的 `POST` 行為。

```javascript
let url = 'https://hexschool-tutorial.herokuapp.com/api/signup';
fetch(url, {
  method: 'POST',
  // headers 加入 json 格式
  headers: {
    'Content-Type': 'application/json'
  },
  // body 將 json 轉字串送出
  body: JSON.stringify({
    email: 'lovef1232e@hexschool.com',
    password: '12345678'
  })
}).then((response) => {
    return response.json(); 
  }).then((jsonData) => {
    console.log(jsonData);
  }).catch((err) => {
    console.log('錯誤:', err);
})
```