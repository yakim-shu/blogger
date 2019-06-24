---
title: "[第九週] 後端基礎 - PHP、資料庫、SQL 語法"
layout: post_project
description: ""
category: project
tags: [PHP, SQL, project_week9]
file_name: 2019-06-12-project_w9_PHP_SQL
---

## 後端「實際上」到底是什麼?

1. 伺服器 Apache : 需要有一個伺服器來處理 Request 跟 Response
    - Server => 程式，專門處理 request & response 的程式
2. 寫程式 PHP : 需要寫程式來處理
    - PHP => 接收 request 處理成 output，通常是 html
3. 資料庫 MySQL : 需要有資料庫可以儲存資料
    - 資料庫系統 => 程式，專門操作有關資料的程式，提供一些更有效率查找、修改資料的方法。

## 接收 Request 流程

```
1. 接收 request
2. server 轉給 php 處理  （重要）
3. php 處理轉成 html 
4. html 傳給 server 
5. server 回傳 response

request => apache => php => output(html) => apache => response
```

也可以用 cmd 輸出 php 程式碼

```bash
php <file-name>.php
```

---

#### 如果要改 Apache 預設根目錄

打開設定檔，路徑： `apache2/` => `conf/` => `httpd.conf`


php 的網址規則是由 server 決定的，所以你不會再 facebook 上看到 `facebook.com/index.php`，但通常 apache 預設的規則是： php 的網址的結構就是按照資料夾目錄。

---

## 基礎 PHP 語法

### 開始寫 PHP
- 要用 `<?php` `?>` 包起來

```php
<?php
    echo 'hello world';
?>
```
- 不用宣告變數，使用變數要用 `$` 開頭
- 字串連接用 `.` 而不是 `+`

```php
<?php
for ($i = 0; $i < 3; $i++) {
    echo $i . '<br>';
}
?>
```

- 陣列 array
    - 輸出完整陣列
        - `var_dump` : 輸出每一個內容的型態跟值： type, value
        - `print_r` : 比較簡潔、沒有輸出型態： value

```php
<?php
$arr = array(1, 2, 3, 4, 5);
$length = sizeof($arr); // 陣列長度
echo $arr[$length - 1]; // 印出最後一個
var_dump($arr); // 輸出 type, value
print_r($arr); // 輸出 value

/* var_dump 輸出結果 */
array(4) {
  [0]=>
  string(3) "one"
  [1]=>
  int(2)
  [2]=>
  string(5) "three"
  [3]=>
  bool(false)
}

/* print_r 輸出結果 */
Array
(
    [0] => one
    [1] => 2
    [2] => three
    [3] => 
)
?>
```


### 接收 request
- `$_GET['key-name']` 是一個物件，可以取得 get 的值（記得用引號）
- `$_POST['key-name']` 是一個物件，可以取得 post 的值（記得用引號）
- `isset()` 檢查是否有此參數

```php
<?php
$username = $_GET['username'];
if (isset($_GET['username'])) {
  // do something...
}
?>
```


---
## 資料庫系統

其實 excel 也是一種資料庫

- 檔案 => `Database`
- tab => `Table`
- 欄位 => `column`

### 關聯式資料庫 Relational database

用不同的 table 去存取不同類型的內容，但各個 table 之間是有相關性的。

好處是避免不相關的資料互相干擾。

#### 關聯式資料語言 SQL （ Structured Query Language ）

是一種程式語言，專門拿來操作關聯式資料庫，簡單來說，就是用指令去操作資料庫。

一般來說，是比較常用的資料庫系統。

比較有名的是 MySQL, PostgreSQL 
 
#### 非關聯式資料語言 NoSQL （ Not only SQL ）

相較於 SQL 只能儲存單一型態的資料， NoSQL 可以儲存的資料更複雜一些，比較常用於存 log 日誌，優點是比較彈性，如果要新增欄位，不用去更改資料庫設計。

比較有名的是 mongoldb

#### 管理資料庫 phpMyAdmin

輸入此網址： http://localhost:8080/phpmyadmin

為一個用 PHP 寫的 GUI 資料庫管理軟體，其本質就是一個 PHP 檔案，用處是以網頁的介面來管理你的資料庫。

類似的資料庫管理軟體： sequel pro 

---

## Table schema 結構簡介

### 資料型態

資料庫結構 Table schema 是在開一個資料庫前，要設定好資料型態、有無預設值、是否為唯一... 等等的設定，而之後的資料都要以符合當初設定的結構，否則無法新增。

- id : `int` 
    - 勾選 ai (auto increment)
        - 保證遞增，但不保證 id 是連續的
    - 設定為 primary index，代表是唯一值
- 短的文字（ 如標題 ）可以用 `varchar`
    - 也可以限定長度（ 字元數 ）
- 長文可以用 `text`
- 日期 `datetime`
    - 預設值改成 `current timestamp`

### varchar 跟 text 的差別？
- `char` ： 長度為 0 ~ 255，當儲存字串不夠 255 的長度時，會用空格補齊剩餘的空間，因此讀取時必須把後面空格去除。
- `varchar`： 可以設定最大長度，適合用在文字量少的欄位，可以有預設值。
- `text`： 不可設定長度，適合用在文字量多的欄位，最大长度为 2 ^ 31 - 1 個字符，不可以有預設值。

查詢速度： 
- char 最快， varchar 次之，text 最慢。
- 由于 varchar 查询速度更快，所以能用 varchar 的时候就不用 text。


---

## 索引 index

可以當成是書本的目錄，資料庫系統會幫你建立某欄位的索引，目的是 **加快搜尋的速度。**


#### Index
- 建立索引需要額外的空間來存資料，所以通常只會幫 **比較常用的欄位** 建立索引。
- Index 也可以是一個複合欄位，例如把 username 跟 password 兩個常用欄位，建立成一個 index。

#### Primary Index (PK) 主鍵
-  一個 table 只能有一個 primary 
- 不能為空值、不能重複，是 table 裡面最主要的欄位
- 通常會加在 id 上面
- 當你設置某欄位為 Primary index 時，該欄位會自動加上 unique index

#### Unique Index 唯一
- 唯一性，如果資料重複的話，系統會報錯

---

## MySQL 基礎語法介紹

### 查詢 `SELECT`

- 某個 column  ： `SELECT` + `<column>`
- 某個 table ： `FROM`  + `<table>`
- 取出某個條件的那列 row ： `WHERE`  + `條件 ( <column> = <value> )`
    - 也可以放兩個條件 

```sql 
where name = 'peter' and id=2 // => &
where name = 'peter' or id=2 // => ||
```

從 users 這個 table 裡面找到是 peter 的那列
並且把 phone 這個欄位的值取出來

```sql
select phone from users
where name = 'peter'
```

把 users 這個 table 裡面的 id & content 欄位取出來
```sql
select id, content from users
```

把 user 這個 table 裡面所有的欄位取出來： *
```sql
select * from users
```

### 刪除 `DELETE`

- 刪除某列 `DELETE`

刪除 users 裡面 name 是 peter 的那列

```sql
delete from users
where name = 'peter'
```

但有時候刪除的做法不同，像是某些比較重要的欄位，通常不會直接刪除，怕造成無法挽回的後果。
所以比較常見的是新稱一個 `is_deleted` 欄位，放一個 boolean 值，如果要刪除他就把此欄位設成 1。

所以篩選資料的時候，where 會將上條件 ： `where is_deleted = 0`


### 更新 `UPDATE` & `SET`

- 某個 table ： `UPDATE` + `<table>`
- 設定某個欄位值 ： `SET` + `<column>`=`<value>` 

更新 user 裡面 name 是 peter 的那列
把 phone 設為 123、age 設為 20

（ `where` 條件很重要，不然會所有欄位都一起更新 ）

```sql
update users set phone = 123, age = 20
where name = 'peter'
```

### 新增 `INSERT INTO` & `VALUES`

- 在某個 table 新增 某欄位 ： `INSERT INTO` + `<table>` + `(<column1>, <column2>)`
- 新增的值為 ： `VALUES` + `(<value1>, <value2>)`

新增一筆紀錄，name: peter, phone: 123
( 兩個括號是對應的關係 )
```sql
insert into users(name, phone)
values ('peter', 123)
```

---

### 設定編碼及時區

```php
<?php
$conn->query("SET NAMES 'UTF8'"); // => 編碼
$conn->query("SET time_zone = '+08:00'"); // => 台灣時區
?>
```