---
title: "[第十二週] 資訊安全 - 常見攻擊：CSRF"
layout: post_project
description: "所以假設你點入駭客提供的 A 網站，其實背後已經發了個 B 網站的 request 而渾然不知。
而這漏洞最可怕的是，如果**你曾造訪過 B 網站，且 B 網站的 session 週期還沒過期（換言之，還在登入狀態）**，同時 B 網站的 Server 驗證機制不夠全面，B 網站很可能就會認為此 request 是來自本人**造訪 B網站** 。
結論：如果你在某網站是保持登入狀態，就存在 CSRF 的風險，其手法是欺騙瀏覽器、讓網站以為是使用者本人的操作。"
category: project
tags: [CSRF, project_week12]
file_name: 2019-07-03-project_w12_Info_Security-CSRF
---

# CSRF 原理 ( Cross Site Request Forgery ) 

其實跟 XSS 有點像，CSRF 的原理是，駭客在其他網頁塞入一個「 目標 domain 」 的連結，偷偷發 request（ 利用隱藏圖片或隱藏表單 ）。

所以假設你點入駭客提供的 A 網站，其實背後已經發了個 B 網站的 request 而渾然不知。

而這漏洞最可怕的是，如果**你曾造訪過 B 網站，且 B 網站的 session 週期還沒過期（換言之，還在登入狀態）**，同時 B 網站的 Server 驗證機制不夠全面，B 網站很可能就會認為此 request 是來自本人**造訪 B網站** 。

結論：如果你在某網站是保持登入狀態，就存在 CSRF 的風險，其手法是欺騙瀏覽器、讓網站以為是使用者本人的操作。

聽起來還是不知道可怕在哪，只要試想把 B 網站換成「 我們常用的網路銀行 」，有可能你默默就把錢轉出去也沒發現，就知道 CSRF 是很危險的。

---

## 防範方法

以下內容完全就是看 Huli 大的好文 [讓我們來談談 CSRF](https://blog.techbridge.cc/2017/02/25/csrf-introduction/) 筆記內容，原來有這麼多方法，其實還滿有趣的，簡直就是開發者跟駭客之間角力戰的歷史過程。



#### 不推薦做法

☞ 比對 request 參數 & session_id （不安全）
- 漏洞：典型的 CSRF 漏洞，有可能從別的 domain 發出惡意 request，如果我是登入狀態，一樣會被攻擊

☞ 傳送方式從 GET 改為 POST（不安全）
- 漏洞： 開一個隱藏 iframe，把 form 藏在裡面，一載入自動 submit

☞ 把 API 改成只接收 JSON（不安全）
- 漏洞： 把 `content type` 改成 `text/plain`，一樣可以用 form 傳送 JSON 
- 雖然 Server 可以擋 `text/plain`，但如果 Server 的 `Access-Control-Allow-Origin: *`，代表別的 domain 的 request 都可以發過來 

☞ 檢查 request 的 header Referer，擋掉不合法 domain（不安全）
- 漏洞：url 變形方法很多，且 user 有可能不會帶上 Referer，不是很保險

---
#### 推薦做法
##### ☞ 加上圖形驗證碼、簡訊驗證碼
- 安全但麻煩，通常有金流操作的網站才會用

### ☞ 加上 CSRF token
- 產生： Server
- 儲存： Server 

在 form 裡新增一個 `name='csrftoken' value='<亂碼>'`，比對 Server 端的 session
- 漏洞：攻擊者可以先發一個 request 取得 csrftoken

#### ☞ Double Submit Cookie
- 產生： Server
- 儲存： Client
 
一樣在表單放 CSRF token，但這次參照值不是存在 Server 裡，而是存在 cookie 裡

這方法利用的是 cookie 只會從相同 domain 帶上來，攻擊者無法從不同 domain 帶上此 cookie

- 漏洞： 攻擊者如果掌握了你底下任何一個 subdomain，就可以幫你來寫 cookie

#### ☞ Client 端生成的 Double Submit Cookie
- 產生： Client
- 儲存： Client

之前提的 Double Submit Cookie，是由 Server 產生、存在 Client 的 Cookie

但由於 SPA 在拿取 CSRF token 會有困難，所以可以改成 Client 端生成，此 cookie 只是要確保攻擊者無法取得、沒有包含任何敏感資訊，所以不避擔心安全性考量
- 某些 library（如：axios），只要設定好 cookie 的值，會幫你自動在 request 的 header 填上 cookie 值，就不用每個表單都要手動加。

---

### ☞ 瀏覽器端的防禦：SameSite cookie ( 最推薦 )

其原理就是**幫 Cookie 再加上一層驗證**，不允許跨站請求。

意思是除了在 B 網站這個 domain 發出的請求，其他 domain（如 A 網站）的發出的 request 都不會帶上此 Cookie，等於是張更安全的通行證。

只要在設置 Cookie 加上：

```php
Set-Cookie: key=value; path=/; domain=example.org; HttpOnly; SameSite=Lax
```

( [PHP 版本要 7.3 以上才支援](https://wiki.php.net/rfc/same-site-cookie) )

SameSite 有兩種模式：`Strict` 、`Lax`
#### `strict` 嚴格
- `<a href="">, <form>, new XMLHttpRequest`... 等標籤，只要不同 domain 都不會帶上此 Cookie
    
#### `Lax` 寬鬆
- 上述標籤可以帶上 cookie
- 除了 `Get` 方法，其他的 `POST`、`DELETE`、`PUT`... 都不會帶上 Cookie 
- 意思是 Lax 模式之下就沒辦法擋掉 `GET` 形式的 CSRF


所以某些網站會準備兩種 cookie，一般瀏覽網頁時、帶上**沒有 `samesite` 的 Cookie**。

當使用者有敏感操作（如：購買、帳戶...等），會帶上 `SameSite=strict` 的 Cookie，所以如果從外部網站發 B 網站的 request 就會要求重新登入，讓攻擊者無法 CSRF（ 常用購物網站的人一定超有感 ）

---
參考資料：
- [讓我們來談談 CSRF](https://blog.techbridge.cc/2017/02/25/csrf-introduction/)