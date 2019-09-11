---
title: "[第二十一週] React 基礎：如何寫 CSS"
layout: post_project
description: ""
category: project
tags: [React, CSS, project_week21]
file_name: 2019-09-11-project_w21_04_React_basic_CSS
---

在 React 中有很多種方式可以寫 CSS，各門派可說是各有自己的看法，建議是可以多嘗試再來決定自己喜歡哪一套寫法：

1. inline-style 
  - 之前提過可以用 `style={}` 來寫 inline style
2. HTML 的 CSS link
  - 就是一般傳統寫法，引入一支外部 CSS
3. 使用 webpack 打包
4. styled-components

本篇要介紹的就是用 **(3) webpack 打包 CSS 跟 (4) styled-components 的做法。**

---
##  webpack 打包 CSS

- 安裝 `style-loader`、`css-loader`，目的是幫助我們將 CSS 載入進來

```shell
yarn add style-loader css-loader
```

- 修改 `webpack.config.js`，新增一條 `rules` 規則

```javascript
{
    test: /\.css$/,
    // => 順序很重要、要搞清楚，webpack 會由後往前跑 loader
    loader: ['style-loader', 'css-loader'], 
}
```

- 在 `src/` 資料夾底下新增一個 CSS 檔案，隨便寫個樣式
- 回到 `app.js` 裡面把 CSS 檔案 import 進來

```javascript
// app.js
import './style.css';
```

- 重新運行 `npm run start`，成功的話就會發現 CSS 被 load 進來囉！

那這樣用 webpack 打包 CSS 的好處是什麼？

這樣 load 進來的 CSS 也能有即時更新的功能，開發時更加方便。

而且這樣的話你可以一個 Component 就用一個 CSS，如果要用其他的 CSS 相關套件，例如 postcss, sass, css-minify... 只要去載入相對應的 loader 就 OK 囉！

---

## styled-components

styled-components 是一個 library，算是目前一個滿主流的用法，用法很特別，有興趣了解更多可以看 [官網教學](https://www.styled-components.com/)。

- 安裝 `styled-components`

```shell
yarn add styled-components
```

- 把 style import 進來

```javascript
// app.js
import styled from 'styled-components';
```

- 開始用 `styled-components` 寫 CSS

```javascript
const Header = styled.h1` // => 定義一個 h1 樣式，名為 Header
  color: green;
  font-size: 50px;
`

const Title = (props) => {
  return (
    <Header onClick={() => { // => 編譯的時候，<Header> 就會變 <h1>
      props.changeCounter(Math.random())
    }}>{props.text}</Header>
  )
}
```

背後的原理就是他會隨機幫你產生個亂數 class，並把剛剛的 style 加上去。

![螢幕快照 2019-09-11 下午6.25.51](https://i.imgur.com/azdTFqL.jpg)

而 `styled-components` 真正厲害的點是可以根據 `props` 的不同，去改變 CSS。

但這部分我還沒有研究，之後會再找時間了解怎麼寫！