---
title: "[第七週] 瀏覽器資料儲存 - Cookie、LocalStorage、SessionStorage"
layout: post_project
description: "有個很重要的觀念是，以上的「 瀏覽器儲存空間 」的資料都是 「 分網站 」儲存的，不同 domain 無法共享這些儲存在 Client 端的資料。"
category: project
image: 
tags: [LocalStorage, Cookie, SeesionStorage, project_week7]
file_name: 2019-06-03-project_w7_storage
---

## Cookie

是個小型文字檔 ( 限制 4k )

當瀏覽器發送 Reqeust 時，會自動帶到 Server 端。

而 Server 的 response header 會有一個欄位 `Set-Cookie` 裡面放著資料，只要瀏覽器看到這個資料，就會把 Cookie 寫進來。

為了讓 Server 可以辨識使用者身份，所有 request 都會自動把該 domain 的 Cookie 帶上去。

```javascript
// 第一次登入
A -> 登入 -> Server

// 第二次帶著 Cookie (通行證) ，就不用再登入一次
A -> Server
```

例如： 廣告追蹤、身份驗證、購物車。

可以在 chrome 開發人員工具裡面的 `Application` 看到該 domain 的資料（ 包括 Cookie、Storage ）

---
## LocalStorage

最推薦也最簡單的資料儲存方法。

- 形式：key → value
    - `key` 跟 `value` 必須是字串，如果是 `<object>` 或 `<number>` 會自動轉換成 `<string>`
    - 存取 JSON 格式要先經過轉換。
- 生命週期：沒有過期時間，除非手動刪除 
    - 意思是說關掉分頁、關掉瀏覽器再打開也都還在。
- 不像 Cookie 會隨著 Request 送到 Server 端，Storage 只作用在 Client 端瀏覽器
    - 適合儲存較複雜且不敏感的資料 ( 喜好設定、顏色樣式等等 )
    
#### 操作 API
- 設置資料：`localStorage.setItem(key, value)`
- 取得資料：`let storageValue = localStorage.getItem(key, value)`
- 清除資料：`localStorage.removeItem(key)`
- 清除全部資料：`localStorage.clear()`

---
## SessionStorage

跟 LocalStorage 只差在生命週期的不同。

- 生命週期：
    只儲存在瀏覽器**同一個分頁還開啟著**的狀態下，把分頁關掉、到另一個分頁就沒有作用了。

因為不同分頁是不能共享 SessionStorage，所以通常拿來儲存更短期的資訊。

---
## 結論

有個很重要的觀念是，以上的「 瀏覽器儲存空間 」的資料都是 「 分網站 」儲存的，**不同 domain 無法共享這些儲存在 Client 端的資料**。



參考資料：
- [[Html 5] WebStorage - localStorage和sessionStorage 基本使用方法](https://dotblogs.com.tw/shadow/2014/07/10/145879)
- [JS30-Day15-LocalStorage](https://ithelp.ithome.com.tw/articles/10195522)