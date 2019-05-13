---
title: "[第四週] 網路基礎 - HTTP、Request、Response"
layout: post
description: "HTTP 就是一套網路傳輸協定，而今天要學的就是這套協定的內容是什麼，以及如何實作一個簡單的 Client 與 Server 端。要了解全球通訊網的基礎，才有辦法依照標準來實作網站。"
category: project
tags: [HTTP, Request, Response, project_week4]
file_name: 2019-05-06-project_w4_Network_http
---

## HTTP 是什麼？

全文為 HyperText Transfer Protocol，中文翻為「 超文本傳輸協定 」。

HTTP 就是一套網路傳輸協定，而今天要學的就是這套協定的內容是什麼，以及如何實作一個簡單的 Client 與 Server 端。要了解全球通訊網的基礎，才有辦法依照標準來實作網站。

### Client & Server

一般來說傳輸資料的兩端會分為 **客戶端 ( Client )** 跟 **伺服器端 ( Server )**

- Client： 以網頁來說就是你的瀏覽器、電腦，主要會發送 **「 請求 request 」**到 Server 端
- Server： 收到 request 開始處理資料，完成後會回傳 **「回應 response 」**到 Client 端  

---

### 你是怎麼看到網頁的畫面？

有沒有想過網頁上的資訊是怎麼來的，其實就是一大堆的 request 跟 response。

1. 瀏覽器發送 HTTP request 到 Server
    - request 包含 head 跟 body
2. Server 回傳 response 到瀏覽器
    - response 包含 head 跟 body
3. 瀏覽器進行解析 html、css、js、圖片檔案...等等，**渲染成可讀性高的網頁內容**

想要看到網頁上進行了哪些 request，可以打開瀏覽器的開發人員工具，切到 Network 標籤底下，再重新整理網頁，就可以看到 resquest 跟回傳的 response 詳細資訊。

> 快速開啟 safari 的開發人員工具 ： `alt` + `cmd` + `I`

### DNS

全文為 Domain Name System

之前的章節也有介紹過 DNS，有興趣請看：[[第一週] 搞懂目錄位置 & 網路基礎概論](https://yakimhsu.com/project/project_w1_Networking_Introduction.html)

當瀏覽器發送 request 的時候，其實是發送一個網址，但要怎麼知道網址的 IP 位置呢？就是靠 DNS 啦

以 [HTTP的維基頁面](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)為例，打開瀏覽器的開發人員工具，點開發送的 request ，可以看到有 **url** 跟 **IP 位置** 的資訊。

![螢幕快照 2019-05-06 上午11.27.23](https://i.imgur.com/PApc3n2.jpg)

> 你也可以在 Terminal 查詢網域的 IP 位置，輸入 `nslookup <要查詢的網域名稱>`

---

### 瀏覽器只是一個程式

前面有提過，其實瀏覽器幫我們做的就是：發送一堆 request、接收一堆 response，根據 html 裡面的內容再進行資源下載，而更詳細的 request 發送順序可以參考以下：

（ **注意**：這僅是目前理解的觀念，可能有誤，歡迎大家指正！ ）

1. 一開始先發送 request 到副檔名為 `.html` 的 url
2. DNS 解析成 IP 位置
3. Server 接收到 request、 回傳 response
4. 瀏覽器解析 html 檔案
5. 根據 html 內容，**一但發現有 CSS、JS、圖片檔案，再發送各檔案的 request**（ 如同步驟 1.2.3 ）
6. 發送完包含在 html 所有資源的 request
7. 開始下載資源 ( CSS、JS、圖片檔 )
8. 進行渲染網頁

---

## 實作一個 Client 端

> 其實沒有瀏覽器的情況下，我們也可以利用其他方式發送 `request` 跟 獲得 `response`。

以下是示範實做一個 Client 端發送 request。

利用 Node.js 的 library - [request](https://github.com/request/request) ( Simplified HTTP client )，模擬瀏覽器發送 request，步驟如下：

- 切到專案資料夾下： `npm install request`
- 直接複製官網提供的範本：

```javascript
const request = require('request');
request('http://www.google.com', function (error, response, body) {
  console.error('error:', error);
  console.log('statusCode:', response && response.statusCode);
  console.log('body:', body);
});
```

現在假設我要發送一個 request 到**此 blog 的首頁**，所以把 URL 改成 blog 網址，再進行發送。
```javascript
const request = require('request');
request('https://yakimhsu.com',
  function (error, response, body) {
    console.error('error:', error);
    console.log('statusCode:', response && response.statusCode);
    console.log('head:', response.headers); // 多印 response 的 header
    console.log('body:', body);
});
```

執行後會得到以下的 response 內容：

![螢幕快照 2019-05-06 下午12.28.41](https://i.imgur.com/DjyGoPt.jpg)

---

#### 小補充 ： `process` 模組

`process` 是 Node.js 內建的模組，功能為「 取得指令的參數 」。

- 引入： `const process = require('process')`
- 取得指令參數： `process.argv`
    - 假如我輸入 `node index.js 2`
    - 會印出 `node` 、 `index.js` 、 `2` ( 前面還會帶一些前綴字 )

---
### 開發人員工具

那在瀏覽器是怎麼顯示 response 資訊的呢？ 

我直接在 blog 首頁開啟開發人員工具，可以看到其實瀏覽器接收到的 response，跟我剛剛用 Node.js 收到的 response 是一樣的。

- 上圖的橘色框： 發送 url 為 `https://yakimhsu.com` 的 request，收到的 response header
- 下圖的橘色框： 在瀏覽器輸入 url 為 `https://yakimhsu.com`，瀏覽器會發送一個此網址的 request，橘色框是收到的 response header（ 只是瀏覽器幫你排版得比較好閱讀而已 ）

![螢幕快照 2019-05-06 下午12.53.19](https://i.imgur.com/B1nfc7V.jpg)

#### `request` 跟 `response` 的共通點
都有 header 與 body，分別帶著不同資訊
  - header: 額外資訊
  - body: 主要內容

---

## 主要的 Http Request Method: Get & Post

而發送的 Request **根據不同的用途**，有查詢、新增、修改或者是刪除，稱為「 Request Method 」，是為了讓 Server 能夠清楚辨別 request 的目的，而最常見的 Method 就是 `Get` & `Post`。

### `Get`
單純的跟 server 要一個連結或圖片，通常**網頁都是 Get 的 request 比較多**
- 例如要去某個網址、看某張圖片
- 因為只是要**獲取資訊、沒有 request body**

### `Post`
需要**執行一些動作**時，會傳送 `Post` 的 request
- 例如登入會員 
- **獲取「 指定的 」資訊，放在 request body ( Form data ) 裡面**
  
### 其他的 Http Request Method
- `Put` : 取代掉整個 request 
- `Patch` : 修改部分 request （ 較推薦 ）
- `Delete` : 刪除資源
- `Head`: 只要獲取 request 的 header，不要 body
- `Option` : 可以暸解 server 提供哪些溝通方法  

---

## Http Code 狀態碼
用數字表示 response 的狀態，通常以開頭的數字做判斷。

```   
1xx: 再等等
2xx: 成功了
3xx: 去其他地方
4xx: 你挫賽了（ 客戶端 ）
5xx: 我挫賽了（ 伺服器端 ）
```
#### 常見的 Status Code
#### 1xx : 稍等
- `100` Continue：Server 成功接收、但 Client 還要進行一些處理

#### 2xx : 成功
- `200` OK：成功
- `204` No Content：成功，但沒有回傳的內容（ 例如當你發出 Delete 的 request ）

#### 3xx : 重新導向
- `301` Moved Permanently：資源「 永久 」移到其他位置，再下一次發出 request 時，瀏覽器會直接到新位置
- `302` Found（Moved Temporarily）：資源「 暫時 」移到其他位置
- `304` Not Modified：東西跟之前長一樣，可以從快取拿就好

#### 4xx : Client 端錯誤
- `400` Bad Request：請求語法錯誤、或資源太大...等等
- `401` Unauthorized：未認證，可能需要登入或 Token
- `403` Forbidden：沒有權限
- `404` Not Found：找不到資源

#### 5xx : Server 端錯誤 
- `500` Internal Server Error：伺服器出錯，搶票時很可能發生
- `501` Not Implemented
- `502` Bad Gateway：通常是伺服器的某個服務沒有正確執行

參考資料：
- [常見與不常見的 HTTP Status Code](https://noob.tw/http-status-code/) ( 介紹很多幽默的 Status Code )

---

## 實作一個簡單的 Http Server

在這之前我們實作一個 Client 端，現在要來試試看 Server 端！

同樣也是利用 Node.js 內建 library : `http`

- 建立一個新檔案: `server.js`，輸入下列程式碼：

```javascript
let http = require('http'); // 引用 library: http 
let server = http.createServer(handleRequest);

function handleRequest(req, res) {
  console.log('request url: ', req.url);
  if (req.url === '/') {
    res.writeHead(200, { // 更改 response header
      'info': 'index'
    })
    res.write('index'); // 更改 response body
    res.end();
    return;
  }
  if (req.url === '/redirect') {
    res.writeHead(301, {
      // 'Location': '/category' // 轉址到 /category
      'Location': 'https://google.com' // 轉址到 google.com
    });
    res.end();
    return;
  }
  if (req.url === '/category') {
    res.writeHead(200, {
      'info': 'category'
    })
    res.write('category')
    res.end();
    return;
  }
  res.writeHead(404);
  res.end();
  return;
}

server.listen(5000); // 監聽 5000 這個 port 
```

- 執行 Server : `node server.js`
- Server 就會一直執行到你按 `ctrl + C` 停止

#### 打開瀏覽器
- 輸入：`http://localhost:5000`
    - 會看到畫面有 `index`
- 當網址加上 `'/category'`
    - 會看到畫面有 `category`
- 當網址加上 `'/redirect'`
    - 會跳轉網頁到 `https://google.com`
- 當網址隨意輸入 `'/dakldf'`
    - 一片空白， 404 Not Found
 



