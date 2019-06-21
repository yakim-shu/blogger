---
title: "[第八週] 交換資料 - 同源政策、CORS、JSONP"
layout: post_project
description: "只要瀏覽器發的 request 位置 跟 Server 端的 位置是不同源（不同網域 ）， request 就會被瀏覽器擋掉。
值得注意的一點是：「 **Request 還是有發出去，而且瀏覽器也確實有收到 Response** 」，重點是因為 **瀏覽器** 才會有同源政策，主要是因為安全性的考量，不把 Response 傳回給你的 JavaScript。"
category: project
tags: [CORS, JSONP, Ajax, project_week8]
file_name: 2019-06-06-project_w8_CORS_JSONP
---

延續上一章節，Ajax 看似很美好，可以不用換頁就可以更新資料，但緊接來的問題是什麼？接下來要介紹的是瀏覽起的同源政策。

---

## 同源政策 Same Origin Policy
只要瀏覽器發的 request 位置 跟 Server 端的 位置是不同源（不同網域 ）， request 就會被瀏覽器擋掉。
  
相同 domain 就是同源，但同網域的 http & https 也是不同源喔。詳細定義請看 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/Security/Same-origin_policy) 的說明

值得注意的一點是：「 **Request 還是有發出去，而且瀏覽器也確實有收到 Response** 」，重點是因為 **瀏覽器** 才會有同源政策，主要是因為安全性的考量，不把 Response 傳回給你的 JavaScript。

---
## CORS 跨來源資源共用

那要怎麼破除在瀏覽器的限制呢，很簡單。

只要 **Server 端在 response 的 header** 加上一行，就可以允許不同網域的 request 存取

```javascript
access-control-allow-origin: *
``` 

除了這個 Header 以外，其實還有其他的可以用，例如說 `Access-Control-Allow-Headers` 跟 `Access-Control-Allow-Methods`，就可以定義接受哪些 Request Header 以及接受哪些 Method。

總結一下，如果你想要發起跨來源 HTTP 請求並且順利收到回應的話，需要確保 Server 端有加上 `Access-Control-Allow-Origin`，不然 Response 會被瀏覽器給擋下來並且顯示出錯誤訊息。

---
### 簡單請求
是一個防護機制，如果是**非簡單請求**（ `非 GET || 帶 Header 參數` ）的 request，都會發送一個「 Preflight Request 」，就會知道這個 API 有沒有提供 CORS，檢查是否有權限存取。

因為非簡單請求可能會帶有一些使用者資料，因此這機制的用意是，先用一個 **OPTIONS** 的 Preflight Request 去確認之後真正的 Request 能不能送出，如果不行的話，真正的 request 就會被擋下來，以防資料庫被亂搞。



#### 瀏覽器住海邊嗎，為什麼管這麼寬？

主要是為了安全性。

很重要的一點是，同源政策只跟「 瀏覽器 」有關，所以用 Node.js 並沒有這層限制。

---
## 第三種方式： JSONP

全名為 JSON with Padding

有鑒於 Ajax 一定會受到同源政策影響，所以有個另類的方式拿到 Response 資料，就是 JSONP。

網頁上有些標籤不受同源政策的限制，像是圖片 `<img>` ，因為沒有安全性的問題，所以可以存取跨來源的圖片。

還有像是引入 JS 的標籤 `<script>` 也可以引入其他 domain 的 js 進來，
等我們拿到 `src` 的內容 (response) ，再進行解析！

所以簡單來說 JSONP 就是 **利用 `src` 不受同源限制的特性，直接載入一隻帶參數的js，當作是發 request，用一個 `function` 包裝起來。**

#### 用 `<script>` 標籤發送 Request
實務上在操作 JSONP 的時候，Server 通常會提供一個 callback 的參數讓 client 端帶過去。

```javascript
<script src="https://api.twitch.tv/kraken/games/top?client_id=xxx&callback=receiveData&limit=1"></script>
<script>
  function receiveData (response) {
    console.log(response);
  }
</script>
```

利用 JSONP，也可以存取跨來源的資料。但 JSONP 的缺點就是你要帶的那些參數「 永遠都只能用附加在網址上的方式（GET）帶過去，沒辦法用 POST 」。

很重要的一點是，要使用 JSONP 傳送資料，也要 **Server 端有提供 JSONP 的方法**（ 意指用 callback function 包起來 ）才行，不然回傳的 Response 就只是字串而已，沒有辦法取得資料。

所以如果能用 CORS 的話，還是應該優先考慮 CORS。

參考資料：
- [輕鬆理解 Ajax 與跨來源請求](https://blog.techbridge.cc/2017/05/20/api-ajax-cors-and-jsonp/) 

---
#### 補充： 單向傳送資料的延伸應用（email 與追蹤）

在網頁裡塞一張隱藏圖片，如果使用者看過這網頁，Server 就會收到此圖的 request，進而知道使用者看過這網頁了。

---
#### 補充： Client Side Redering

用 JavaScript 動態新增的頁面，意思是 `按右鍵` > `檢查原始碼`的時候，看不到資料。

壞處是搜尋引擎沒有辦法爬內容，會以為是空網頁，不利於 SEO。

