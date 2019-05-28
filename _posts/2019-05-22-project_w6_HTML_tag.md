---
title: "[第六週] HTML - tag 基礎標籤介紹"
layout: post_project
description: ""
category: project
image: https://i.imgur.com/yI8OjLW.jpg
tags: [HTML, project_week6]
file_name: 2019-05-22-project_w6_HTML_tag
---

HTML 全名為 HyperText Markup Language ，中文翻為「 超文本標記語言 」

要了解 HTML 標籤之前，你要先知道常聽到的 「 HTML 5 」 只是 HTML 最新的版本。

而 HTML 5 有新增很多功能，語意化標籤、Web Sockets、離線緩存... 但在此章節會討論的只有標籤部分。

對 HTML 5 有興趣了解更多的可以看 [MDN - HTML5](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML) 的介紹。

以下開始介紹最基本的 HTML 標籤。（ 包括舊標籤 與 HTML5 才出現的標籤 ）

---

## 必要的基本標籤

- `<!DOCTYPE HTML>`
    - HTML5 的宣告方式，比以往的宣告方式更簡單，對於未支援 HTML5 的瀏覽器會忽略 HTML5 的特性。
    - 是一個宣告，所以沒有關閉標籤
- `<html>`
    - 網頁的最根部元素
- `<head>`
    - 網頁的資訊，包含 CSS & JS 檔案的網址，內容不會顯示在頁面上
- `<body>`
    - 網頁的主要內容，所有在畫面看得到的元素都放在裡面

---

## 常見的 <head> 內容

```html
<head>
    <!-- 在瀏覽器 tab 標籤上的標題 -->
    <title>網頁標題</title>
    <!-- 聲明該頁面的編碼方式 -->
    <meta charset="UTF-8"> 
    <!-- 行動裝置的顯示方式，initial-scale=1.0 代表不縮放 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 在瀏覽器 tab 標籤上的小圖示 -->
    <link rel="icon" href="favicon.ico">
    <!-- 連接 main.css -->
    <link href="main.css" rel="stylesheet">
    <!-- 連接 common.js -->
    <script src="common.js"></script>
</head>
```

其實 CSS 跟 js 的引入不一定要放在 `<head>` 標籤裡，放在 `<body>` 其實也可以，**只是放在 `<head>` 是比較常見的作法。**


---

## 常見的 <body> 標籤

[ 大區塊 ]

- `<header>` 網頁標頭，經常包含 logo、標題、導航列
- `<main>` 主要內容
- `<footer>` 網頁結尾，經常包含版權資訊、聯絡方式
- `<article>` 主要文章
- `<section>` 段落
- `<nav>` 導航列
- `<aside>` 跟主要內文沒有太直接關係的側欄

[ 內容 ]
- `<div>` : 分組用 ( block )
- `<ul>` 有序清單 & `<ol>` 無序清單
    - 底下清單都是 `<li>` 
    - 去除清單預設樣式 CSS：`list-style: none`

[ 圖片相關 ]
- `<img>` 圖片
    - `src`  圖片網址
    - `title` 鼠標停留在圖片上的提示文字
    - `alt` 圖片連結失效的替代文字
- `<figcaption>` 圖片說明文字

[ 文字 ]
- `<h1>` ~ `<h6>` 網頁標題
    - 數字越小越重要
- `<p>` 網頁內文
    - 假文產生字： [Lorerm ipsum](https://loremipsum.io)
    - 也可以用塊級字體取代： [blokkfont](http://www.blokkfont.com)、[Redacted-Font](https://github.com/christiannaths/Redacted-Font)
- `<span>` 分組用 ( inline )
    - 通常放在 `<p>` 裡面
- `<em>` 強調文字
- `<strong>` 重要文字
- `<blockquote>` 表示引用自其他內容
- `<a>` 超連結 or 錨點
    - 超連結：
        - `href` 填 網址
        - `target` 內連 or 外連
            - `_self` 在同一個標籤頁跳轉
            - `_blank` 另開標籤頁
    - 錨點：
        - `href` 填 `#id`
        - 例如網頁要跳到 `h1`
            - `<h1 id="title">標題</h1>`
            - `<a href="#title">跳到標題</a>`
- `<pre>` 保留格式
    - 因為在瀏覽器解析裡，不管內文有多少空白，都只會顯示成 1 個空白，也不會換行。所以如果要換行可以使用 `<br>>` 標籤
- `<code>` 放程式碼
- `<time>` 日期與時間
- `<hr>` 分隔線
- `<br>` 換行  

[ 嵌入內容 ]
- `<ifram>` 內嵌網頁
  - `src` 填上要內嵌的網址 
  - 但 `server` 端可以設定擋掉被其他網頁嵌入的功能。
- `<embed>` 嵌入一個外部資源，可以是影片、圖片、程序
- `<video>` 一段影片檔
- `<audio>` 一段聲音檔
- `<source>` 可填入不同的 `<video>` or `<audio>` 類型，讓瀏覽器選擇支援度高的媒體播放。
- `<canvas>` 通過 JavaScript 實現繪圖功能，甚至是動畫、遊戲
- `<svg>` 繪製一個向量圖

[ table 表格 ]
- `<thead>` 表格頭部
- `<tbody>` 表格主體
- `<tr>` 橫排
    - 放 `<td>` 表格內容
    - 第一排通常會放 `<th>` 表頭

```html
<table>
    <tr>
        <th>姓名</th>
        <th>年齡</th>
    </tr>
    <tr>
        <td>小花</td>
        <td>20</td>
    </tr>
    <tr>
        <td>小明</td>
        <td>10</td>
    </tr>
</table>
``` 

[ form 表單 ]

表單內容又多又雜，所以我另外整理成一篇：[[第六週] HTML - 表單 from 介紹](https://yakimhsu.com/project/project_w6_HTML_form.html)

---

## 語易化標籤 Semantic Elements

HTML5 新出的這些標籤，最重要的目的就是「 語易化 」，**透過有明確功能的標籤，搜尋引擎及人類肉眼都可以輕易看出這段結構的功能**，以下列出具有語意的標籤：

、`<article>`、`<aside>`、`<details>`、`<figcaption>`、`<figure>`、`<footer>`、`<header>`、`<main>`、`<mark>`、`<nav>`、`<section>`、`<summary>`、`<time>`

參考資料：
- [消滅 Lorem 吧！仿真內容的介面設計](https://medium.com/d-d-mag/消滅-lorem-吧-內容導向的介面設計-fb0ac6d22fd5)
- [ML5 标签列表](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)

---

### ☞ 補充： js 的引入標籤 `<script>`

`<script>` 有些屬性可以添加，可以改變 js 的載入模式。

預設用法：
- 下載後立刻執行 `src` 的內容，在執行完之前會停止繪製頁面，所以**如果 js 檔案很大，畫面會卡住。**

HTML5 新屬性：
- `async`
    - **用非同步模式加載，下載後立刻執行**，但同時也繼續載入頁面與其他 js 檔案。
    - 適合此 js 可以獨立運作，不依賴於其他 js 的情況下使用。
    - 例： `<script async src="common.js"></script>`
- `defer` 
    - **下載完會先等著，等到頁面都解析完成，觸發 `DOMContentLoaded` 事件才執行。**
    - 類似把 js 放在頁尾的情況，適合此 js 在等頁面繪製完才執行。
    - 例： `<script defer src="common.js"></script>`

底下這張圖很清楚說明 js 載入順序的差別，灰色為畫面暫停繪製的部分。

![async vs defer](http://www.growingwiththeweb.com/images/2014/02/26/async-vs-defer-twitter.png)
( image from [Growing with the Web](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html))

參考資料：
- [[HTML5] script 的新增屬性 defer, async](http://n.sfs.tw/content/index/10323)
- [script tag 屬性 async defer 差別](https://blog.xuite.net/vexed/tech/61308318-script+tag+屬性+async+defer+差別)
- [async vs defer attributes](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
