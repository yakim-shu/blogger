---
title: "[第六週] 基礎 SEO 標籤 - meta、og、JSON-LD"
layout: post_project
description: ""
category: project
image: https://i.imgur.com/qON6YOo.jpg
tags: [HTML, SEO, project_week6]
file_name: 2019-05-23-project_w6_HTML_SEO
---

###  SEO 很重要，但還是內容為王

最實在的 SEO 的技巧是什麼，就是把 **「 內容做好 」**，聽起來就是屁話、但的確是最有效的方法。而底下介紹的 meta 比較像是基本功，meta 是**幫助機器看懂你的網頁**，並不會呈現在網頁內容中喔。

> 「 meta 下好很重要，但有品質的內容才是關鍵 」。

---
## `<title>` 標題

- `<title> ... <title>`
- 建議中文保持在 30 個字元、英文 60 個字元以內。

網頁標題非常重要，根據 [網站 SEO優化《第一篇》Title Tag 的重要性與寫法](https://medium.com/@jeffreywang1183/seo-title-tag-f5d5e4f52c12) 的說明，比較理想的標題可以用以下格式： `主要關鍵字 — 次要關鍵字 | 廠牌名稱`

（ 回頭看自己標題完全是錯誤示範... ）

---

### 實際來看看 `<title>` 會怎麼呈現

要注意 `<head>` 裡面放的 `<title>` ，跟網頁內容 `<body>` 的文章標題 `<h1>` 可不一樣（ 雖然很有可能會設定成一樣 ）

這個 `<title>` tag 會顯示在**瀏覽器標籤**上，而更重要的，也是 **Google 搜尋列表**上顯示的標題。

舉 [SEO Secrets: Reverse-Engineering Google’s Algorithm](https://medium.freecodecamp.org/seo-secrets-reverse-engineering-googles-algorithm-92fad4f5a39) 這個網頁為例：

`<title>` 在網頁的位置

![ title - 網頁快照](https://i.imgur.com/Lji6J9N.jpg)

`<title>` 在 Google 搜尋結果頁的位置

![title - 搜尋結果頁](https://i.imgur.com/CKdWSXQ.jpg)

( 藍色字為 `<title>` 會出現的地方 )

---

### `<meta>`description 摘要

- `<meta name="description" content="...">`
- 建議中文保持在 80 個字元、英文 160 個字元以內。

description 就是摘要，也就是搜尋列表標題底下的文字。（ 上圖黃色字部分 ）

查資料說摘要的內容並沒有直接影響到排名，但 description 還是相當重要，請盡量把重點放在越前面的地方越好，但也不要亂塞關鍵字，還是要讓語句保持通順。

好的 description 可以讓使用者更快速了解、此網頁內容是不是他需要的，**找到「 更精準 」的 TA ，提高點擊率、降低跳出率，才是讓你的排名提高的關鍵。**

( 如果網頁內容裡的關鍵字很多，不知道該怎麼下，其實也可以「 不要下 」，讓搜尋引擎去爬你的文章，如果有出現使用者的關鍵字，就會把那段標出來，出現在 description 上 )

---

### `<meta>` keywords 關鍵字（ 建議不要用 ）

- `<meta name="keywords" content="...">`

Google 在 2000 年就提過這個 keywords tag 已經沒在用，原因是在早年已被濫用，雖然加了 keywords 應該是沒有直接傷害，但也不會有幫助。

參考資料：
- [Meta Title Description 怎麼寫？別小看標題、摘要，SEO老司機就是你！](https://www.awoo.com.tw/blog/2018/06/meta-title-description/)
- [網站 SEO優化《第一篇》Title Tag 的重要性與寫法 — SEO優化](https://medium.com/@jeffreywang1183/seo-title-tag-f5d5e4f52c12)
- [網站 SEO優化《第二篇》Meta Description 的寫法 — SEO優化](https://medium.com/@jeffreywang1183/meta-description-17f65e1f47a6)

---

### `<meta>` og 社群相關 tag

og 全名為： Open Graph Protocol

og 適用於台灣人最常用的 facebook 跟 Line，像 Twitter 跟其他社群媒體其實是不一樣的 code，但使用率不高就算了哈哈哈

其實就是給社群媒體分享的 meta ，跟上面的內容很像，一樣是有 title, description，比較特別且重要的是 image 屬性，也可以直接設定分享圖的寬高等等。

如果想知道分享到社群的預覽樣貌，可以利用 [Facebook debugger](https://developers.facebook.com/tools/debug/sharing/) 進行偵錯，把網址貼上去，他就會說明 Facebook 抓取到的內容有哪些，呈現出來的分享結果會是怎樣。

（ 注意： 當你發現改了 og 資訊，但在 Facebook debugger 還是沒有變，**很有可能是被 cache 了**，這時請手動按網址底下的按鈕  `Fetch new scrape information` ）

```html
<head>
    <meta property="og:type" content="type" />  
    <meta property="og:title" content="site_title" />
    <meta property="og:description" content="site_description" />
    <meta property="og:image" content="site_image" />
    <meta property="og:url" content="site_url" />
    <meta property="og:site_name" content="site_name" />
</head>
```

> 補充 og:image 圖片尺寸

- 大小不要超過 8 m
- 尺寸要大於 200px * 200px，否則連圖都抓不到
- 想要在高解析度裝置也不失真，建議寬度至少為 1200
- 如果圖片為**長方形**
  - 建議最小尺寸 600px * 315px（ 或同比例 `1.91` : `1` ）
- 如果圖片為**正方形**
  - 建議最小尺寸 600px * 600px （ 或同比例 `1` : `1` ）

( 有興趣了解更多的話， Facebook 官方文件：[網站與行動應用程式的分享最佳作法](https://developers.facebook.com/docs/sharing/best-practices?locale=zh_TW#) 說得很清楚，該如何優化你的 og tag )

參考資料：
- [如何使用 Open Graph, Twitter Card 自訂網頁在 Facebook, Google, Twitter… 的顯示內容](http://blog.shihshih.com/social-meta-tag/)
- [網站與社群連結常用的 SEO 中繼標籤](https://blog.toright.com/posts/5661/%E7%B6%B2%E7%AB%99%E8%88%87%E7%A4%BE%E7%BE%A4%E9%80%A3%E7%B5%90%E5%B8%B8%E7%94%A8%E7%9A%84-seo-%E4%B8%AD%E7%B9%BC%E6%A8%99%E7%B1%A4.html)
- [Facebook - 網站與行動應用程式的分享最佳作法](https://developers.facebook.com/docs/sharing/best-practices?locale=zh_TW#)

---

### JSON - LD

全名為 JSON for Linking Data

其實跟 meta 有點像，目的是用一個固定格式呈現網頁內容，讓機器更快速且精準的判讀內容。
在搜尋列表中出現更多的資訊，進而吸引使用者點擊。

以下為為呈現在搜尋結果的樣子 

![JSON - LD - 搜尋列表結果](https://i.imgur.com/qON6YOo.jpg)


有興趣可以看 [官方文件說明](https://developers.google.com/search/docs/data-types/product)，想要測試可以使用 Google 提供的 [結構化資料測試工具](https://search.google.com/structured-data/testing-tool)。

( 但比較適用於商業網站，blog 好像沒有其必要，所以就沒有再深入研究了。 ）

---

### robots.txt

是一個 txt 檔案，目的是當搜尋引擎檢索你的網站時，告訴爬蟲哪些網頁需要檢索，哪些則不用。

（ 下列內容取自 [robots.txt用途與使用範例教學，釐清SEO收錄觀念！](https://www.awoo.com.tw/blog/2018/03/robotstxt-crawl/) ）


- `User-agent` => 定義下述規則對哪些搜尋引擎生效，即是對象。
- `Disallow` => 指定哪些目錄或檔案類型不想被檢索，需指名路徑，否則將會被忽略。
- `Allow` => 指定哪些目錄或檔案類型可能被檢索，需指名路徑，否則將會被忽略。
- `Sitemap` => 指定網站內的sitemap檔案放置位置，需使用絕對路徑。


但**有哪些情境是不希望被網頁被收錄**的呢？
- 測試階段：未正式公開、但又需要上線做測試的版本
- 網站管理者後台

所以如果你不需要測試，也沒有後台，那很簡單、就全部允許檢索就好了。

```
User-agent: *
Disallow:
sitemap: <網站 sitemap.xml>
```

想要測試網站的 robots.txt 語法正確與否，可以到 [Google Search Console](https://search.google.com/search-console/about?hl=zh-tw) 進行檢測。

---

### sitemap.xml

是一個 xml 檔案，也就是網站的地圖，讓搜尋引擎知道此網域底下，有哪些網頁需要檢索。

同樣的，想要測試網站的 sitemap.xml 語法正確與否，也可以到 [Google Search Console](https://search.google.com/search-console/about?hl=zh-tw) 進行檢測。