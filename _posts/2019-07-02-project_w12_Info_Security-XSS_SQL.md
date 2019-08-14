---
title: "[第十二週] 資訊安全 - 常見攻擊：XSS、SQL Injection"
layout: post_project
description: "駭客可以在輸入資料時，用一些奇怪的方式（惡意字串）竄改 SQL 語法，以偷取、假冒別人資料或刪除資料庫，例如以下輸入就可以成功登入別人帳號"
category: project
tags: [XSS, SQL Injection, project_week12]
file_name: 2019-07-02-project_w12_Info_Security-XSS_SQL
---


# SQL Injection 


以下是填帳號密碼的欄位，接收到開發者預期的**正常輸入**，再根據使用者的輸入去撈資料。

[ 正常輸入 ]
- 帳號： 123
- 密碼： 456

```sql
// 接收輸入的 SQL
SELECT * FROM users WHERE user='123' AND pwd='456';
```

---
而 SQL Injection 的本質就是把「 輸入的惡意資料」 變成「 程式的一部分 」

意思是駭客可以在輸入資料時，用一些奇怪的方式（惡意字串）竄改 SQL 語法，以偷取、假冒別人資料或刪除資料庫，例如以下輸入就可以成功登入別人帳號：

[ SQL Injection ]
- 帳號： ' or 1=1 --
- 密碼： (甚至不用輸入)

```sql
// 接收輸入的 SQL
SELECT * FROM users HWERE user='' or 1=1 --' AND pwd =''; // => 永遠成立
```

- SQL 語法中的 `--` ： 代表註解，所以後面的條件就被忽略
- 因為條件多了 `or 1=1`，且 `pwd` 也因為註解被忽略掉，所以 `WHERE` 條件永遠成立，就可以成功登入帳號了。
- 照理來說會登入**第一個**篩選到的帳號

---
### 解決方法： Prepare Statement
Prepare Statement 做的就是幫我們處理 SQL 跳脫這件事

步驟：
- 把要放的參數都改成問號
- 其實就是 SQL query 換種寫法，拿到資料後的操作是差不多
- 要注意的是 `bind_param()` 的第一個參數，有幾個參數就要寫幾個字元，字元依照參數的資料類型而定
    - `string` => `s`
    - `int` => `i`

```php
<?php
$stat = $conn->prepare("SELECT * FROM users WHERE username=? and pwd=?"); // => 把參數換成 ?
$stat->bind_param('ss', $username, $password); // => 替換成準備好的參數
$stat->execute(); // => 執行 query 語法
$result = $stat->getResult(); // => 執行結果
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc(); // => 撈資料
}
?>
```

---
# XSS ( Cross-Site-Scripting )

> 在別人網站執行 Javascript 

跟 SQL Injection 一樣，本質也是讓使用者「 輸入的資料」 變成「 程式的一部分 」

利用 input 欄位可以輸入內容的特性，只要使用者輸入特別的 JS 語法，且網頁有 **輸出此內容** 的時候，就可以竄改網頁或竊取資料。


XSS 漏洞分為幾種類型：

#### 儲存型 XSS ( Stored )
- 網址列看不出問題
- 最常見的例子就是網站留言板或是訊息，因為使用者可以留任何訊息。
- 存在 DB 裡，所以每個使用者打開都會看到被修改的內容
- 殺傷力最大

```html
<input type="text" placeholder="輸入內容"> // 輸入欄位
<script>alert("XSS攻擊測試");</script> // 輸入惡意碼

<p>文字文字文字</p> // => 正常輸出
<p><script>alert("XSS攻擊測試");</script></p> // => 不正常輸出，且每個使用者都會中標
```

---
#### 反射型 XSS ( Reflected )
- 把惡意程式藏在網址列裡，放在 GET 參數傳遞
- 必須誘導使用者點到假連結才有用
    - 但網址列看起來會很可疑，可用短網址或[特殊編碼](https://meyerweb.com/eric/tools/dencoder/)魚目混珠

如果網頁是在網址上用參數判斷狀態：`login.php?status='登入失敗'`，且輸出錯誤訊息的方式是 **直接把 value 印在網頁上**，那麼只要把 value 換成 js 語法就可以攻擊成功

```php
// 網頁程式
if (isset($_GET['status']) && !empty($_GET['status'])) {
    echo $_GET['status'];
}

// 網址列
http://www.example.com?status='新增成功'  => 印出新增成功
http://www.example.com?status=<script>alert(1)</script>  => 執行惡意程式
``` 

---      
#### DOM 型 XSS
- 可能是頁面上有使用到 `.html()` 或 `.innerHTML()` 的語法 
- 所以就可以直接放 JS 語法
- 跟前兩種 XSS 不一樣，此漏洞要在前端檢查
- 最好都改成 `.innerText()`=> 只會輸出純文字



---

### 所以要是能注入 JS 能幹嘛？

最常見的是偷 cookie 資料：
- 通常會放在一張隱藏圖片 `<img>` (因為不受同源政策影響)，然後在 `src` 裡面發送一個 **帶著 cookie 資訊的 reqeust，傳送到自己的 Server**，這樣就可以拿到點此網址的使用者 cookie 了

```javascript
<img src="" onerror="sendRequest('document.cookie')">
// src 讀取失敗，執行 onerror
// sendRequest() => 只是拿來說明可以送 request 到自己 Server，單純舉例用
```

---

#### 要在哪裡檢查？輸入 or 輸出
檢查有無惡意碼的時機，可以分為在「輸入之前」跟「輸出之前」，一般建議是**無論如何、輸出端的檢查一定要做！**

#### 輸入驗證：

因為 XSS 有太多漏洞可以鑽： HTML, JavaScript, CSS, XML, URL，要很完整對輸入做防範非常困難。

例如刪除所有 `<script>`、 `onerror` 及其他可以執行 JS 的字串，但設黑名單也不是一個理想的方式，因為有太多種變形可以換，白名單是比較推薦的作法，只是要寫的完整也是非常麻煩。

#### 輸出驗證：
## 使用跳脫字元 escape
- 如果「 輸出內容 」使用者可以操作，絕對不能輸出原碼（ 未經處理 ）
- 把內容轉譯成純文字，而不是程式碼

```php
// php 跳脫字元的內建函式 htmlspecialchars
echo htmlspecialchars($str, ENT_QUOTES, 'utf-8')
```

```html
// 輸出時需要 encoding
& --> &amp;
< --> &lt;
> --> &gt;
" --> &quot;
' --> &#x27;     
/ --> &#x2F;
```

參考資料：
- [XSS攻擊的深入探討與防護之道](https://www.qa-knowhow.com/?p=2992)
- [XSS攻擊的原理與防範措施](https://beautyofprogram.com/2018/10/xss%E6%94%BB%E6%93%8A%E7%9A%84%E5%8E%9F%E7%90%86%E8%88%87%E9%98%B2%E7%AF%84%E6%8E%AA%E6%96%BD/)
- [駭客如何用XSS讀取 Cookie?](https://www.qa-knowhow.com/?p=2951) => 列出許多 XSS 輸入碼，有興趣可以看看

---

## 常見疑問
### 應該在哪裡做 escape ?
- 通常是輸出資料的時候才要 escape，而存在資料庫裡的應該還要是明碼（除了密碼）

### 哪些地方要做 prepare statement ?
- 有使用者可以操作的地方都需要，而如果都是自己可以控制的，照理來說不需要
- 但還是建議每個地方都用，但全都加上是一個好習慣，理由是：
  - 統一標準
  - 需求會改，有可能現在不用，未來還是有可能改成使用者會 輸入的欄位
