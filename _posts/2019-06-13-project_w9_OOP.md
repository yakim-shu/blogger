---
title: "[第九週] OOP - 物件導向基礎概念"
layout: post_project
description: "Class 有點像設計圖的概念，你要建的房子是基於設計圖當基底去建造的。
把需要的屬性都宣告出來，要實體化出來時再 new 一個物件(Instance)出來。"
category: project
tags: [OOP, project_week9]
file_name: 2019-06-13-project_w9_OOP
---

## 類別 Class & 實體 Instance

有點像設計圖的概念，你要建的房子是基於設計圖當基底去建造的。

把需要的屬性都宣告出來，要實體化出來時再 new 一個物件出來。

```php
<?php
Class Dog { // => 「 類別 」
    function hello() { // => method 方法
        echo 'hello, I am a dog.';
    }
}
$dog = new Dog(); // => $dog 為 Dog 類別的「 實體 」
$dog->hello();
?>
```

## Public & Private

延續上一個範例

Public 
```php
<?php
Class Dog {
    public function hello() { 
        echo 'hello, I am a dog.';
    }
}
$dog = new Dog();
$dog->hello(); // => 可以成功呼叫
?>
```

Private 

```php
<?php
Class Dog {
    private function hello() { 
        echo 'hello, I am a dog.';
    }
}
$dog = new Dog();
$dog->hello(); // => 會出錯，外圍不能呼叫
?>
```
把 method 改成 private 之後，實體化的物件就不能呼叫了，想必此時會產生個疑問？

有什麼情況下要把 method 改成外圍不能使用的呢？這就要談到封裝的概念了。

## 封裝 Encapsulation

意思是說把細節藏起來，所以對外圍可以使用的接口，會變得非常單純。

所以是說把**常用的方法打開，但方法底層是怎麼實作的，使用者不需要關心。**

```php
<?
Class Dog {
    public function hello() { 
      echo $this->getHelloSentence(); // => 對外接口
    }
    private function getHelloSentence() { // => 藏起來的細節
      return 'hello, I am a dog.';
    }
}
$dog = new Dog();
$dog->hello();
?>
```

而封裝的概念其實早在使用 Ajax 的時候就有接觸過了。

仔細看 `XMLHttpRequest` 就是瀏覽器提供的 Class，而我們發的 request 就是實體化 `XMLHttpRequest` 這個設計圖，其中 `.open()`、`.onload()`、`.send()` 就是 Class 提供的 public 方法。

對於我們來說，只需要把參數傳進 method ，不用去了解 `XMLHttpRequest` 這個 Class 是怎麼實作的，這個概念也就是封裝。

```javascript
let request = new XMLHttpRequest();
request.open('GET', url, true);
request.onload = () => {
 ...
} 
request.send()
```


所以在實務上，要實作封裝的想法，可以先從 「 使用者要怎麼呼叫 method 會比較方便 」的角度來思考。

所以假如以之前的作業「 計算機 」的範例，要實作 Class 之前，可以先想好使用者會呼叫哪些 method ，如以下：

```javascript
Class Calculator {
    ...
}

let calculator = new Calculator();
calculator.input(1);
calculator.input('+');
calculator.input(3);
const result = calculator.getResult();
console.log(result);
```

---

## getter & setter

有時候 property 不想讓外部無意間被改到、或不能讓外部隨意更改，所以會設置 getter & setter 來透過 「特定的方法 」做存取。

這樣跟直接用 public 屬性更改的差異是，因為是 function 存取，所以可以有一些 **條件限制**，例如傳進來的值如果不是所預期的，就把它擋住、不能存取。


```php
<?php
Class Dog {
  private $name = 'default';
  public function hello() { 
    echo $this->getHelloSentence();
  }
  private function getHelloSentence() {
    return 'hello, I am '. $this->name . '<br>';
  }

  // setter 
  public function setName($name) {
    if ($name === 'Fuck') return // => 如果是髒話就不能設置
    $this->name = $name;
  }
  // getter
  public function getName() {
    return $this->name;
  }
}
$dog1 = new Dog();
$dog1->setName('Leo');
$dog1->hello(); // => hello, I am Leo

$dog2 = new Dog();
$dog2->setName('Amy');
$dog2->hello(); // => hello, I am Amy
?>
```



---

## Constructor 建構子


意思是在實體化的時候，就直接把「 初始參數 」帶進去，所以改寫上個例子，在 new 出實體化的時候，name 就被傳進各個 Instance
```php
<?php
Class Dog {
  private $name = 'default';
  
  // 初始化
  public function __construct($name) { // => 建構子
    $this->name = $name;
  }
  public function hello() { 
    echo $this->getHelloSentence();
  }
  private function getHelloSentence() {
    return 'hello, I am '. $this->name . '<br>';
  }
}
$dog1 = new Dog('Leo');
$dog1->hello(); // => hello, I am Leo

$dog2 = new Dog('Amy');
$dog2->hello(); // => hello, I am Amy
?>
```

--- 

## Javascript 中的 Class

在 Javascript 中其實並沒有 Class 的概念，之所以可以用這關鍵字，是因為底層用了其他方法來實現

延續剛剛計算機的概念。

```javascript
Class Calculator {
    constructor() {
        this.text = '';
    }
    input(str) {
        this.text = str;
    }
    getResult() {
        return eval(this.text);
    }
}

let calculator = new Calculator();
calculator.input(1);
calculator.input('+');
calculator.input(3);
const result = calculator.getResult();
console.log(result);
```

---

## PHP 中的 Class

SQL 跟 Server 的連線是個練習 Class 的好方法，影片在重看一次，然後試試看有沒有辦法改寫

包成 Class 的好處是 **在 new 的同時，constructor 也放了一個 `init()` 做一些初始化的工作。**

mySQL 用法有很多種，可以像 `$conn->query('...')` 也可以 `mysqli_query($this->$conn, '....')`


Huli 是推薦這種跟 Class 有關的寫法
`->` 後面不用加 `$`

找了一些網路上的範例，看到比較推薦的是寫兩個檔案：

1. `DB_config.php` : 放 server & user & db 資料
2. `DB_conn.php` : 連線、取資料等初始化動作

#### DB_config.php

```php
<?php
$servername = 'localhost';
$user = 'root';
$password = '';
$dbname = 'jobs';
?>
```

#### DB_conn.php

```php
<?php
require_once('./DB_config.php');

class Db {
  public function __construct($servername, $user, $password, $dbname) {
    $this->server = $servername;
    $this->user = $user;
    $this->pass = $password;
    $this->db = $dbname;
    $this->init();
  }
  private function init() {
    $this->conn = mysqli_connect($this->server, $this->user, $this->pass, $this->db);
    if ($this->conn->connect_error) {
			die('Failed: ' . $this->conn->connect_error);
		}
    mysqli_query($this->conn, "SET NAMES utf8");
  }

  public function query($str) {
    $this->result = mysqli_query($this->conn, $str);
  }

  // 拿到所有資料
  public function fetch_array() {
    while ($row = mysqli_fetch_array($this->result, MYSQLI_ASSOC)) {
      printf("ID: %s  title: %s", $row["id"], $row["title"] . "<br>");
    }
  } 
}

$db = new Db($servername, $user, $password, $dbname);
?>
```

#### 使用資料時，new 出一個 DB Class

```php
require_once('./DB_config.php');
require_once('./DB_conn.php');

$sql = "SELECT * FROM jobs";
$db->query($sql);
echo $db->fetch_array(); // 印出所有 id & title
```


##### `mysqli_fetch_array` 存取欄位陣列
- `MYSQLI_ASSOC` 用欄位名稱選擇（ `$row['id'], $row['title']` ）
- `MYSQL_NUM` 用陣列位置選擇 （ `$row[0], $row[1]` ）
- 如果沒有輸入選擇方式的話，預設是選擇 `MYSQL_BOTH`
    - 兩者皆可用（ `$row[0], $row['id']` ）

參考資料：
- [PHP database connection class](https://stackoverflow.com/questions/3228694/php-database-connection-class)
- [解釋 mysql_fetch_array （還有 num & both）](https://www.php.net/manual/en/function.mysql-fetch-array.php)
- [[PHP] 透過 While 和 mysql_fetch_arry 把所有陣列的資料輸出](https://pjchender.blogspot.com/2015/05/php-while-mysqlfetcharry.html)
- [Super-fast PHP MySQL Database Class](https://codeshack.io/super-fast-php-mysql-database-class/) ( 可以參考別人的 DB Class 怎麼設計的 )



---

## 繼承 Inheritance

### extends 繼承自另一個 Class

某一個物件如果跟新的物件很像，可以用 `extends` 去取得父物件的屬性。

#### super 改變父 Class 的方法

可以基於父 Class 的方法做一些改變與微調。

其他程式語言應該都是用 `super` 這個關鍵字，但在 PHP 是用 `parent::<method>` 語法

### protected 屬性狀態：可以給繼承物件使用的 private

屬性值除了 `private` 跟 `public` 還有 `protected`。

意思是說只有跟繼承的物件都可以用，就像是 private + extends 的子物件。

```php
<?php
Class Animal {
  private $name = 'default';
  
  // 初始化
  public function __construct($name) {
    $this->name = $name;
  }
  public function hello() { 
    echo $this->getHelloSentence();
  }
  private function getHelloSentence() {
    return 'hello, I am '. $this->name . '<br>';
  }
}
Class Dog extends Animal { // => 繼承
  public function run() {
    echo $this->name . 'is running';
  }
  public function sayYoAndHello() {
    echo 'Yo';
    parent::hello(); // => 執行 Animal 的 hello 函式
  }
  // public function hello() { // => overwrite
  // ...
  // }
}
$dog1 = new Dog('Leo');
$dog1->hello(); // => hello, I am Leo

$dog2 = new Dog('Amy');
$dog2->hello(); // => hello, I am Amy
?>
```

### 舉 JavaScript 做繼承示範

新增一個 `chineseCalculator` 的 Class，如果輸入的數字件是中文，就轉化成數字，繼承自原本的 `Calculator` Class
```javascript
Class Calculator {
    constructor() {
        this.text = '';
    }
    input(str) {
        this.text = str;
    }
    getResult() {
        return eval(this.text);
    }
}
Class chineseCalculator extends Calculator {
    input(str) {
        if (str === '三') {
            super.input(3); // => super 語法
        } else {
            super.input(str);
        }
    }
}

let calculator = new Calculator();
calculator.input(1);
calculator.input('+');
calculator.input('三'); // => 會轉化成 input(3)
const result = calculator.getResult();
console.log(result);
```

---

## Static 靜態方法

Class 本身也可以有 function ，意思是給 Class 用的，而不是給 new 出來的 instance 用的。

通常用在：
- 這個函式的內容不會被改變
- 每一個 instance 都會共用的變數
- 可以用來檢查要 new 出來的參數是否合理，如果不合理就擋掉


特性：靜態的變數永遠就只會有一個，如果是跟著 instance ，代表 new 出幾個 instance 就會有幾份變數


```php
<?php
class MentorProgramAPI {
    static $version = 1; // => 靜態
    public $version = 1;
}
$api = new MentorProgramAPI();
// 存取
echo MentorProgramAPI::$version // => 靜態存取，在 Class 上
echo $api->version // => 在 Instance 上
?>
```

--- 

## JavaScript ES6 之前的 OOP 用法

JavaScript 原本是沒有 Class 的（ ES 6 之前 ），所以接下來是要示範在沒有 Class 語法的時候，用 JavaScript 的語法去「 模擬 」 Class 的特性。

用前面的計算機舉例：
- 用 function 定義 Class => 取代 constructor 建構子
- 用 prototype 綁定 method 方法

```javascript
// => 原本的方法
Class Calculator {
    constructor() {
        this.text = '';
    }
    input(str) {
        this.text = str;
    }
    getResult() {
        return eval(this.text);
    }
}
// --------------

// => 使用 function 模擬 Class 用法
function Calculator(name) { // => 等於 constructor 建構子
    this.name = name;
    this.text = '';
}
Calculator.prototype.input = function(str) { // => 添加 method 方法
    this.text = str;
}
Calculator.prototype.getResult = function() {
    return eval(this.text);
}
// --------------

let calculator = new Calculator('name');
calculator.input(1);
calculator.input('+');
calculator.input(3);
const result = calculator.getResult();
console.log(result);
```