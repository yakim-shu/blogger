---
title: "[第四週] API 基礎 - RESTful API、JSON、curl 指令"
layout: post
description: "以網頁用的 Web API 來舉例，就像是對方定義好了插座的規格，我們只要照著規格、把形狀合適的插頭插上去，就可以得到我要的資料，而你不會知道資料是怎麼來的，就像你不知道電是怎麼傳過來的"
category: project
tags: [API, RESTful API, project_week4]
file_name: 2019-05-08-project_w4_API
---

# API 是什麼？

全名為 Application Programming Interface，中文翻譯為「應用程式介面」

> 簡單來說就是方便溝通、交換資料的管道。

怎麼說方便溝通呢？ 我自己覺得中國的翻譯：「接口」，比較容易想像。

就像是把家裡的 **插頭 (開發者)** 插進 **插座(API)** 就 **有電(資料交換)**。

而 API 有分很多種：
- 軟硬體廠商的 API
- 有作業系統用的 API ，用於傳輸、寫入、讀取資料...等等電腦上一切的操作
- 自己團隊內部用的 API 
- 而對於網頁來說就是 Web API
  - 是基於 http 協定下運作的 API

以網頁用的 Web API 來舉例，就像是對方定義好了插座的規格，我們只要照著規格、把形狀合適的插頭插上去，就可以得到我要的資料，而你不會知道資料是怎麼來的，就像你不知道電是怎麼傳過來的（ 或許你知道 ）

> 你只是呼叫 function 加上參數，就會得到一個結果，中間的商業邏輯與資料篩選是提供 API 的 Server 端來處理。

#### Web API 實際上最常應用的場景
一般來說，Web API 就是只透過 HTTP 協定的 API，有幾個常見的應用：

- 會員登入
- 社群嵌入
    - 分享、按讚按鈕、嵌入貼文、留言板、影音
    - 例如：[Facebook Graphic](https://developers.facebook.com/docs/graph-api?locale=zh_TW)
- 資料嵌入
    - 例如：[Yahoo 氣象](https://developer.yahoo.com/weather/)、[Google 地圖](https://developers.google.com/maps/documentation/timezone/start?hl=zh-tw)
    
以網站來說，最常用的應該是登入系統，現在不管哪個網站，在會員登入頁，大部分都會有 Facebook 跟 Google 登入按鈕，**這麼做對開發者、用戶、甚至是供應商，都是有利的狀況**：
- 開發者： 取自大神的登入功能，安全又省力
- 用戶： 幾乎每個人都有帳號，無須花時間填寫會員資料
- API 供應者： 
    - 免費的廣告
    - 獲取使用者操作與使用習慣的資料，再進行分析，以方便投放更精準的廣告
    
#### 一個好的 API 應該要有下列條件
- 完整的 API 文件說明與規範
- 有範例程式，解釋如何運用於應用程式中
- 有教學指引，一步一步帶領開發者學習使用 API
- 合理的命名規則
- 適當的防呆措施、必須預防開發者在錯誤使用時，也不會造成嚴重的傷害
- 一開始就要有清楚的規劃、使用情境，因為一但更新或改動，客戶端都要做相對的審視與修改。

參考資料：
- [What is an API? - Application Programming Interface](https://www.youtube.com/watch?v=B9vPoCOP7oY)
- [什么是API，接口？](https://www.youtube.com/watch?v=Pbx4elFAXR0)
- [API設計和應用程式設計的不同？](https://www.ithome.com.tw/voice/87207)
- [API ? SDK? 傻傻分清楚](https://blog.jyny.tw/2013/01/api-sdk.html)
- [什麼是API?](https://cola.workxplay.net/what-is-an-api/) ( 販賣機例子非常好懂 )

---

## RESTful API
現代 API 有一大部分都是走 RESTful 設計風格。

> 注意： 是一種廣泛流行的 API 設計規範、**不是規定**

### 「 非 」 RESTful API 長怎樣？
簡單設計一個非 RESTful 的 API，功能在於操作文章，可以看得出來 **「 動詞是寫在 URL 」**上。
- `GET` + `/Get_allArticle`： 獲取 所有文章 
- `GET` + `/Get_Article/5`： 獲取 `id 為 5` 的文章
- `POST` + `/Create_Article`： 新增 文章
- `POST` + `/Delete_Article/5`： 刪除 `id 為 5` 的文章

### 改成 RESTful 風格又會長怎樣？
如果內容要做得非常 RESTful ，其實還滿嚴格的，我還沒深入研究。

但如果只是想要淺略了解的話，核心在於 request 要符合 **「 語易化的動詞 Method 」 + 「 名詞 URL 」**的格式，例如：

- `GET` + `/articles`： 獲取 所有文章 
- `GET` + `/articles/5`： 獲取 `id 為 5` 的文章 
- `POST` + `/articles`：  新增  文章
- `DELETE` + `/articles/5`： 刪除 `id 為 5` 的文章
- `PATCH` + `/articles/5`： 修改 `id 為 5` 的文章部分內容
- `PUT` + `/articles/5`： 更新 `id 為 5` 的文章全部內容 ( 換句話說，就是覆蓋 )

> Huli 在課程舉了一個我認為很好理解的例子。

「 就像你寫網頁的時候，你也可以整個 html 檔案全部都用 `div` 標籤，但使用更有語意化的標籤（ 例如`section`、`article` ），可以更好從結構裡看出內容的作用。 」

### CRUD 原則
也就是說，就算你只用 `GET` 跟 `POST` 也可以達到以上功能，但充份善用 HTTP Method 可以讓開發者更快速地了解如何使用此 API，也**符合 CRUD 原則的**、分別是 Create (Post)、Read (Get)、Update (Put)、Delete (Delete)，我們就把這種風格稱為 RESTful API。

#### 參考資料：
- [[不是工程師] 休息(REST)式架構? 寧靜式(RESTful)的Web API是現在的潮流？](https://progressbar.tw/posts/53)
- [[API] (1) - 定義 1 - 什麼是 REST/RESTful ?](https://ithelp.ithome.com.tw/articles/10157431)
- [何謂RESTful API？](https://dotblogs.com.tw/jeffyang/2018/04/21/233001)
  - 推薦給想看更深入定義 RESTful API 的人
- [閒聊 - Web API 是否一定要 RESTful?](https://blog.darkthread.net/blog/is-restful-required/) 
  - 討論了 RESTful 有可能造成哪些問題。雖然我只看得懂一小部分，但也明白了 RESTful 潛在的風險，值得一看！
- [RESTful API 最佳实践](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)
  - 神人、總免不了一拜
  
---

## 資料格式 XML & JSON
常用的資料格式有兩種： XML、JSON

### XML 
> 以前傳資料的格式，現代開發比較少用

- 全名為 Extensible Markup Language
- 跟 HTML 有點像、都是標記語言 Markup Language，特色是「 內容用**前後標籤**包起來 」，因為此格式檔案較大、且不好閱讀，所以現代開發比較少人用 XML。

### JSON
> 比起 XML 更輕量，**為現代最普遍、常用的資料格式**。

- 全名為 JavaScript Object Notation
- 儘管來源自 JavaScript 的子集，但其實廣泛運用在其實程式語言，幾乎所有與網頁開發相關的語言都有JSON函式庫。
- 乍看跟 JavaScript 的物件很像，但 `key` 值必須要用雙引號 `"key"` 包起來
- JSON 字串可以包含 **陣列 ( 用中括號 `[ ]` )** 以及 **物件 ( 用大括號 `{ }` )**
- 如果還是有點模糊，非常推薦點進去這網站：[JSON Editor online](https://jsoneditoronline.org)，左右對照、一目了然！
- 整個 JSON 格式不能使用註解
- `Vaule` 值沒有辦法放 function

### 實際來處理 JSON 格式
所以當我們發出一個 request，收到資料格式為 JSON 的 response 要怎麼處理呢？

一樣用 [reqres.in 的 API](https://reqres.in) 為例，請看以下程式碼：

```javascript
const request = require('request');
request(`https://reqres.in/api/users/5`, function (error, response, body) {
  console.log('原始格式，JSON 格式的字串----------');
  console.log(body);
  console.log('轉成 JS 物件--------------------');
  console.log(JSON.parse(body));
```

如果只是把 `response 的 body` 印出來，會發現是**「 JSON 格式 」的 「 字串 」**，所以用內建的函式 `JSON.parse(<JSON>)` 做處理，就會轉成**「 JS 物件 」**。

![螢幕快照 2019-05-08 上午11.42.58](https://i.imgur.com/T1nNFzp.jpg)

> 反過來說，如果想要把 「 JS 物件 」轉成「 JSON 格式的字串 」，可以用：  `JSON.stringify( <object> )`

#### 參考資料：
- [JSON - 維基百科](https://zh.wikipedia.org/wiki/JSON)
- [瞭解JSON格式](http://j796160836.pixnet.net/blog/post/30530326-瞭解json格式)
- [你不可不知的 JSON 基本介紹](https://blog.wu-boy.com/2011/04/你不可不知的-json-基本介紹/)
- [[筆記] JavaScript中物件(object)和JSON格式的轉換](https://pjchender.blogspot.com/2016/01/javascriptobjectjson.html)

---
  
## 必學指令  `curl`

( 以下內容完全參考至這篇文章： [Linux Curl Command 指令與基本操作入門教學](https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/)，詳細又好懂，感謝！ )

隨便看一個 Web API 文件，一定都會看到上面用 `curl` 示範如何發出正確的 request，因為此做法最簡單、沒有任何門檻，只要在終端機 ( Terminal ) 輸入指令就可以發 request！

雖然很簡單使用，但 curl 可是很強大的，支援發 HTTP request 、下載及上傳檔案的功能，基本格式為：
```
curl [options] [URL...]
```

### 預設是 `GET` 方法
簡單示範一下，用 `curl` 發 request 到 google 首頁
- 最基本的作法就是： `curl http://www.google.com`
- 也可以輸出 response 到 `google.html` 這個檔案： `curl http://www.google.com > google.html`

### 加上參數
- `-I`
  - `--head` 只需要 `response header`
  - `curl -I http://www.google.com`
- `-o`
  - 小寫 `O`
  - `--output` 下載 response 到新檔案，假設為 `google.html`
  - `curl -o google.html http://www.google.com`，其效果等同： `curl http://www.google.com > google.html`
- `-O` 
  - 大寫 `O`
  - `--remote-name` 下載指定網址的檔案內容
  - 例如下載 google 首頁圖片，拿到檔案 `googlelogo_color_272x92dp.png`
      - `curl -O https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png` 
- `-L`
  - 直接跟隨 Status Code 為 `301 / 302` 轉址後的網址
  - `curl -L google.com`
- `-X` 
  - `--request` 使用 HTTP Method 來發出 request，後面要加上 `GET`/`POST`/`PUT`/`DELETE`/`PATCH`
  - `curl -X GET "http://www.example.com/api/resources`
- `-H`
  - `--header` 設定 request 要攜帶的 header
      - `curl -H http://www.example.com "X-TestHeader1:111"`
- `-s` 
  - `--silent` 靜音模式，不輸出任何東西

### 常見 Restful CRUD 指令：
- 取得資料： GET
  - `curl -X GET "http://www.example.com/api/resources`
- 新增 JSON 資料： POST
  - `$ curl -X POST -H "Content-Type: application/json" -d '{"status" : false, "name" : "Jack"}' "http://www.example.com/api/resources"`
- 刪除資料： DELETE
  - `curl -X DELETE "http://www.example.com/api/resources/1`

    
#### 參考資料：
- [linux指令 curl指令详解](https://blog.51cto.com/doiido/1564631)
- [Linux Curl Command 指令與基本操作入門教學](https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/)