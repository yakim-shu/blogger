---
title: "[第十一週] 資訊安全 - 網站辨識身份機制：Session"
layout: post_project
description: "Http 協議是**無狀態協議**，他是一個 Request -> 處理 -> Response。
意指每次發 Request 的動作都是各自獨立的，並不會紀錄之前執行過的動作。每次請求都不會紀錄上次執行的動作，所以 Session 機制就是讓我們可以判斷使用者是否已經登入過。
Session 機制算是一種**概念**，所以不同程式語言、有不同的實現方法。"
category: project
tags: [Session, project_week11]
file_name: 2019-06-28-project_w11_Info_Security-Session
---

## 網站是怎麼辨識使用者身份的？

Http 協議是**無狀態協議**，他是一個 Request -> 處理 -> Response。

意指每次發 Request 的動作都是各自獨立的，並不會紀錄之前執行過的動作。每次請求都不會紀錄上次執行的動作，所以 Session 機制就是讓我們可以判斷使用者是否已經登入過。

## Session 機制能幹嘛？

Session 機制算是一種**概念**，所以不同程式語言、有不同的實現方法。

簡單來說就是新訪客造訪網站時，會建立一個新 Session（ 會話 ），Server 端就可以對應 Session 表找到此 Session ID，來辨識這是以前造訪過的訪客跟訪客資料。

而要實作 Session 機制可以有兩種作法：

---

## ☞ 利用 cookie 儲存 Session ID 在瀏覽器上：
- 儲存位置：Client side	 
- 發送一個「 帶有 Session ID 的 Cookie 」在瀏覽器裡，而以後同一網域、同一個瀏覽器發送的每個 Requset 都會帶上此 Cookie。
- 要注意的是 Cookie 對於 HttpOnly 與 Secure 的設定。
- 存在的風險：
  - 基於「 存在 Client 端的資料都是不安全的 」，所以要是網頁受到攻擊，使用者的 Cookie 被偷走，網站認證、不認人。
  - 有心者就可以拿著使用者的識別證登入網站，所以要怎麼設計 session id 是很重要的，**千萬不可以存使用者帳號或 user_id**，或任何可以輕易猜出來別人的 session id。

#### 實作步驟：

1. 資料庫: 建立 `user_certificate` 的 table
2. 當使用者成功登入時，比對 `user_certificate` 有無此 user
  - 有 => 代表之前登入過，取 `id`
  - 沒有 => 代表第一次登入，建立一個 亂數 `id`，同時並設一個帶著 `id` 的 `cookie`

---

## ☞ 儲存 Session ID 在伺服器上
- 儲存位置：Server side (通常放在 Server 記憶體或 DB)
- 新增一個 Session ID 在伺服器裡，而因為是存在伺服器裡，是個比較安全的做法。


#### PHP 內建的 session 機制:

PHP 已經包裝好了，所以可以直接用語言提供的 Session 方法

```php
<?php
// 啟動 Session，並新增一個 user_id
session_start(); // => 記得放在最上面
$_SESSION['user_id'] = $user_id;

// login.php 查看
echo $_SESSION['user_id'];

// logout.php 刪除
session_destroy();
?>
```

參考資料：
- [php 中 session_id () 函式詳細介紹](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/215642/)
- [PHP - PHP session 使用入門](https://blog.xuite.net/gotrans/life/13801477-PHP+-+PHP+session+%E4%BD%BF%E7%94%A8%E5%85%A5%E9%96%80)
- [PHP Session 使用介紹，啟用與清除 session](https://www.webtech.tw/info.php?tid=33) => 列表完整 php Session 用法

---

## 結論

有一點很重要的是，要搞清楚 Cookie 在其中扮演的角色，只是因為 Cookie 可以儲存在瀏覽器裡，剛好方便實作此 Session 機制。

所以就算沒有 Cookie，我們也是可以把 Session ID 存在 Server 或 DB 裡面。

---
參考資料：
- [SessionID.cookie,Session 傻傻分不清楚？？](https://dotblogs.com.tw/daniel/2017/04/08/110915)
- [HTTP Session 攻擊與防護](https://devco.re/blog/2014/06/03/http-session-protection/) => 圖文並茂、講得超級清楚
- [sessionid 如何产生？由谁产生？保存在哪里？](https://www.cnblogs.com/woshimrf/p/sessionId.html)
- [Session 的資訊安全設計原則、測試、個案與實作](https://www.qa-knowhow.com/?p=2737) => 也解釋的非常清楚