---
title: "[第九週] SQL - 內建函式、JOIN、排序"
layout: post_project
description: ""
category: project
tags: [PHP, SQL, project_week9]
file_name: 2019-06-12-project_w9_SQL_JOIN
---

## 正規化 Normalization

分成多個 table，不同 table 存不同資料，減少每層的 table 複雜度，這樣的寫法比較有彈性

關連式資料庫：每個 table 都有關係，但切開來會比較好維護及查詢。

### 資料庫 table 的命名規則
- 小寫
- 會加 s 表明是複數
- 用底線 _ 分開字詞（不是駝峰式命名）
 
## 內建函式
#### `COUNT` 數量

計算 comment 這個 table 有幾個 id

```sql
SELECT COUNT(id) FROM 'comment'
```

#### `SUM` 總和

計算 comment 這個 table 的 id 數字總和

```sql
SELECT SUM(id) FROM 'comment'
```

#### `AVG` 平均

計算 comment 這個 table 的 id 數字平均

```sql
SELECT AVG(id) FROM 'comment'
```

#### `CONCAT` 合併欄位（ 字串連接 ）

合併 comment 這個 table 的 id & name 的值

```sql
SELECT CONCAT(id, name) FROM 'comment'
```


#### `between` 用在數字、日期上（ 條件篩選 ） 
找出在某個區間的資料

```sql
where number between 2 & 5
```

#### `in` 有點像陣列的 indexOf

選出 id 等於 `1 || 2 || 3` 的欄位

```sql
SELECT * FROM 'comments' WHERE id in (1, 2, 3)
``` 

#### `like` 用在字串上（ 比對字元 ）

- `%h` : h 開頭的字串
- `%h%` : 任何包含 h 的字串

選出包含 `o` 字串的所有欄位
```sql
SELECT * FROM 'comments' WHERE content like '%o%'
```

---

## SQL Join 合併 table

用下圖解釋比較清楚：

![](https://live.staticflickr.com/8346/8190148857_78d0f88cef_b.jpg){:width="600"}

(image from [mattimattila.fi](https://mattimattila.fi/sql_liitokset_visuaalisesti.html))

- inner join 交集
- left join 左邊區塊 + 交集
    - 保存左邊，右邊有資料就放進來
- right join 右邊區塊 + 交集
    - 保存右邊，左邊有資料就放進來
- full outer join 連集


---

### 排序 `ORDER BY`

升冪 ascending （小 => 大） 

```sql
SELECT * FROM users ORDER BY time ASC
```

降冪 descending（大 => 小） 
```sql
SELECT * FROM users ORDER BY time DESC
```

---

### 分頁

回傳前 30 筆資料
```sql
SELECT * FROM users LIMIT 30
```

回傳 61~90 筆資料 （ 從 60 開始、回傳 30 筆資料 ）
```sql
SELECT * FROM users LIMIT 30 OFFSET 60
SELECT * FROM users LIMIT 60, 30 // 跟上面一樣（OFFSET 60, LIMIT 30 的簡寫）
```

---

### 改名

用 `as` 關鍵字，後面接 `<new-name>`，用來縮減字詞還實用的
```sql
SELECT created_at as time, nickname as name FROM users

SELECT c.id, c.content, u.nickname as name
FROM comments as c, users as u
WHERE c.user_id = u.id

```

---

### 疑難雜症解惑

- 如果中文變成???

`mysqli_set_charset($conn, "utf8")`

- 覺得字串拼接太麻煩

```php
<?php
$sql = "select from users where name='$name'"
?>
```

或者是用 `sprintf`


