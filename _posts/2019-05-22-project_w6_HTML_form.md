---
title: "[第六週] HTML - 表單 form 介紹"
layout: post_project
description: ""
category: project
image: https://i.imgur.com/W8Hk65w.jpgㄋ
tags: [HTML, form, project_week6]
file_name: 2019-05-22-project_w6_HTML_form
---

相信大家都有填表單的經驗，最常見的應該就是 [google form](https://www.google.com.tw/intl/zh-TW/forms/about/)，今天要來簡單實作個表單的 HTML 結構。

## 不可或缺的標籤 `<form>`

所有 HTML 表單 都是以 `<form>` 元素开始，以下為 `<form>` 的屬性值：
- `action` 表單傳送的網址
- `method` 傳送方法：`GET` or `POST`
  - Get 方法 （ 默認 ）
    - 表单提交是被动的（ 比如搜索引擎查询 ），并且没有敏感信息
    - 傳送的參數下在 url，都是可見的：`action_page.php?firstname=Mickey&lastname=Mouse`
    - 而參數的 `key` 則為表單的 `name`
    - GET 最适合少量数据的提交。浏览器会设定容量限制
  - POST 方法  
    - 表单正在更新数据，或者包含敏感信息（例如密码）
    - POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的

---

### 單行輸入 `<input>`、說明文字 `<label>`

`<input>` 沒有關閉標籤，其中輸入類型 `type` 為最重要的屬性類型，以下是幾種常見的 `type` 類型：

##### [ 文字輸入 ]

```html
<form>
    <input type="text" name="name">
    <input type="email" name="email">
    <input type="password" name="password">
    <input type="search" name="search">
    <input type="tel" name="phone">
    <input type="url" name="url">
<form>
```
- `text` 純文字
- `password` 顯示會以符號代替文字
  - 可以使用 `maxlength` and `minlength` 來控制輸入字數
- `email` 會自動做簡單驗證
- `search` 搜尋框
- `tel` 電話
- `url` 網址

##### [ 選擇框 ]

```html
<form>
    <input type="date" name="date">
    <input type="color" name="color">
    <input type="file" name="upload-file">
    <input type="number" name="sigle-number">
    <input type="range" min="0" max="5" name="range-number">
    <!-- 單選 -->
    <div>
        性別：
        <input type="radio" name="gender" id="male" checked>
        <label for="male">男性</label>
        <input type="radio" name="gender" id="female">
        <label for="female">女性</label>
    </div>
    <!-- 複選 -->
    <div>
        興趣：
        <input type="checkbox" name="interest" id="sport">
        <label for="sport">運動</label>
        <input type="checkbox" name="interest" id="reading">
        <label for="reading">閱讀</label>
    </div>
<form>
```

- `date` 日期，有些瀏覽器會出現簡單的日曆工具
- `color` 選擇顏色
- `number` 整數數字
- `file` 上傳檔案
- `range` 一個範圍的數字
- `radio` 單選框
  - 要設定同樣的 `name`
  - 搭配標籤 `<label>` 做選取文字
- `checkbox` 複選框
  - 要設定同樣的 `name`
  - 搭配標籤 `<label>` 做選取文字

##### [ 按鈕 ]

```html
<form>
    <input type="button" value="按了沒用">
    <input type="submit" value="按了會傳送表單">
    <input type="reset" value="按了會重置表單">
<form>
```
- `button` 無動作按鈕，按下去不會發生任何事，但當你要使用 JavaScript 來操作時非常好用。
- `submit` 提交按鈕，按下會送出 `form` 裡所有的資訊
  - 按鈕文字在 `value` 更改，或者改用 `<button>` 標籤
- `reset` 重置表單內容（ 盡量別用，使用者體驗不佳 ）

---

### 多行輸入 `<textarea>` 
跟 `<input>` 標籤不同的是，需要有關閉標籤 `</textarea>`。
- `rows` 行數
- `cols` 列數

```html
<form>
    <textarea name="message" cols="30" rows="10"></textarea>
</form>
```
---

### 下拉選單 `<select>`、`<option>`、`<optgroup>`
  - `option` 單個選項內容
    - 可加上 `selected` 屬性，表示為預設選項。
  - `optgroup` 分組選項內容，但本身並不可選
    - `label` 標籤是強制的，會顯示在選項中

```html
<form>
    <select id="myAddress" name="myAddress">
      <option>台北市</option>
      <option>新北市</option>
      <optgroup label="彰化縣">
        <option selected>彰化市</option>
        <option>花壇鄉</option>
        <option>鹿港鎮</option>
        <option>員林鎮</option>
      </optgroup>
    </select>
</form>
```

---

## 表單屬性

某些屬性是可以共用的，但並不是所有 input 的類型都可以擁有這些屬性，以下列出幾個最常見的屬性：

  - `name` 如想要正確的提交表單，要記得每個輸入項目都要帶有 `name` 屬性。
  - `value` 預設內容
    - `<input>` 沒有關閉標籤，所以內容是寫在 `value` 屬性值上面，但通常會用 `placeholder` 代替。
  - `disabled` 禁止作用
  - `required` 表示為必填項目
  - `plceholder` 表單內的提示文字，當使用者開始輸入文字就會消失
    - 根據 [MDN - Labels_and_placeholders](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#Labels_and_placeholders) 的解釋，盡量以 `<label>` 標籤取代
     `placeholder`，或者兩者以一起用，以防止使用者輸入到一半忘記表單的用途。
  - `maxlength` 控制輸入框的最大字數，適用於 `text` & `password`
  - `checked` 預設選項，適用於 `radio` & `checkbox`
  - `selected` 預設選項，適用於 `<option>`

-----

## 表單範例

> 除了加個背景色，內容沒有加上 CSS ，就是很醜的預設樣式。

<form style="background: #eee; padding:20px;">
  <div>
    <label for="name">姓名：</label> 
    <input type="text" id="name" value="我是 value 文字">
  </div>
  <div>
    <label for="phone">電話：</label> 
    <input type="tel" id="phone">
  </div>
  <div>
    <label for="email">Email：</label>
    <input type="email" id="email">
  </div>
  <div>
    <label for="date">日期：</label>
    <input type="date" id="date">
  </div>
  <div>
    <label for="password">密碼：</label>
    <input type="password" id="password">
  </div>
  <div>
    <label for="color">顏色：</label>
    <input type="color" id="color">
  </div>
  <div>
    <label for="upload-file">上傳檔案：</label>
    <input type="file" id="upload-file">
  </div>
  <div>
    <label for="sige-number">選個整數：</label>
    <input type="number" id="sigle-number">
  </div>
  <div>
    <label for="range-number">選個範圍 0 - 5：</label>
    <input type="range" min="0" max="5" id="range-number">
  </div>
    性別：
    <input type="radio" name="gender" id="male" checked>
    <label for="male">男性</label>
    <input type="radio" name="gender" id="female">
    <label for="female">女性</label>
  <div>
    興趣：
    <input type="checkbox" name="interest" id="sport">
    <label for="sport">運動</label>
    <input type="checkbox" name="interest" id="reading">
    <label for="reading">閱讀</label>
  </div>
  <div>
    <label for="message">給我們的建議：</label>
    <textarea name="message" id="message" cols="30" rows="10" placeholder="我是 placeholder 文字，開始輸入會消失"></textarea>
  </div>
  <div>
    <label for="myAddress">地址</label>
    <select id="myAddress" name="myAddress">
      <option>台北市</option>
      <option>新北市</option>
      <optgroup label="彰化縣">
        <option selected>彰化市</option>
        <option>花壇鄉</option>
        <option>鹿港鎮</option>
        <option>員林鎮</option>
      </optgroup>
    </select>
  </div>
  <div>
    <button type="submit">送出表單</button>
  </div>
</form>

----

### 範例程式碼

```html
<form>
  <div>
    <label for="name">姓名：</label> 
    <input type="text" id="name" value="我是 value 文字">
  </div>
  <div>
    <label for="phone">電話：</label> 
    <input type="tel" id="phone">
  </div>
  <div>
    <label for="email">Email：</label>
    <input type="email" id="email">
  </div>
  <div>
    <label for="date">日期：</label>
    <input type="date" id="date">
  </div>
  <div>
    <label for="password">密碼：</label>
    <input type="password" id="password">
  </div>
  <div>
    <label for="color">顏色：</label>
    <input type="color" id="color">
  </div>
  <div>
    <label for="upload-file">上傳檔案：</label>
    <input type="file" id="upload-file">
  </div>
  <div>
    <label for="sige-number">選個整數：</label>
    <input type="number" id="sigle-number">
  </div>
  <div>
    <label for="range-number">選個範圍 0 - 5：</label>
    <input type="range" min="0" max="5" id="range-number">
  </div>
    性別：
    <input type="radio" name="gender" id="male" checked>
    <label for="male">男性</label>
    <input type="radio" name="gender" id="female">
    <label for="female">女性</label>
  <div>
    興趣：
    <input type="checkbox" name="interest" id="sport">
    <label for="sport">運動</label>
    <input type="checkbox" name="interest" id="reading">
    <label for="reading">閱讀</label>
  </div>
  <div>
    <label for="message">給我們的建議：</label>
    <textarea name="message" id="message" cols="30" rows="10" placeholder="我是 placeholder 文字，開始輸入會消失"></textarea>
  </div>
  <div>
    <label for="myAddress">地址</label>
    <select id="myAddress" name="myAddress">
      <option>台北市</option>
      <option>新北市</option>
      <optgroup label="彰化縣">
        <option selected>彰化市</option>
        <option>花壇鄉</option>
        <option>鹿港鎮</option>
        <option>員林鎮</option>
      </optgroup>
    </select>
  </div>
  <div>
    <button type="submit">送出表單</button>
  </div>
</form>
```

參考資料：
- [HTML forms guide](https://developer.mozilla.org/zh-TW/docs/Learn/HTML/Forms)
- [创建我的第一个表单](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Your_first_HTML_form)
- [如何建構 HTML 表單](https://developer.mozilla.org/zh-TW/docs/Learn/HTML/Forms/How_to_structure_an_HTML_form)
- [HTML 表单](http://www.w3school.com.cn/html/html_forms.asp)
  
