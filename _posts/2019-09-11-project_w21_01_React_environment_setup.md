---
title: "[第二十一週] React 環境建置：手把手步驟 & 超級懶人包"
layout: post_project
description: ""
category: project
tags: [React, environment, project_week21]
file_name: 2019-09-11-project_w21_01_React_environment_setup
---

## 前置作業

因為 React 會用到 webpack、babel 等等，所以環境建置會麻煩一些，但還是有些懶人建置指令一次完成，在文章最下面會介紹。

但其實不管怎樣，跑一次流程都會讓你對 React 的運作更清楚，所以自己跑過建置環境還是必要的步驟。

---

## 1. 用 webpack 打包 js
因為寫 React 會用到 `import` 語法，所以需要 webpack 來幫我們做 bundle

- 先到目標資料夾，初始化 npm
- 安裝 `webpack`、`webpack-cli`

```shell
npm init
yarn add webpack webpack-cli
```

- 新稱一個 `utils.js` 測試，export 出一個 log function

```javascript
export function log(str) {
  console.log(str);
}
```

- 新稱一個 `index.js` import 剛剛的 log function

```javascript
import { log } from './utils';
log('hello world');
```

- 新增一個 `webpack.config.js` webpack 設定檔
    - `module.exports`： 可以當成 export 出一個 json 格式的設定檔
    - `entry` : 進入點

```javascript
const path = require('path');

module.exports = {
  entry : './src/index.js',
  output : {
    path: path.join(__dirname, '/dist'),
    filename : 'bundle.js'
  }
}
```

- 打開 `package.json`，修改 script，新增 `start` 跟 `build` 任務

```javascript
"scripts": {
    "start": "webpack --mode development",
    "build": "webpack --mode production",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

- 運行 `npm run start`
- 應該就可以看到 `build/` 資料夾下多了打包好的 `bundle.js` 的檔案

```shell
npm run start
```

- 將這隻 `bundle.js` 引入至 `index.html`

```html
<script src="./dist/bundle.js"></script>
```
- 打開 `index.html` 查看 console 就可以知道已經打包成功囉！

---
## 2. babel 轉成 ES5

因為許多瀏覽器還是不支援 ES6 語法，所以需要 babel 轉成 ES5 語法，而且需要 babel 轉化 JSX 語法

- 先安裝 `@babel/core`、`@babel/preset-env`
- `babel-loader` 給 webpack 用的

```shell
yarn add @babel/core babel-loader @babel/preset-env
```

- 新增一個 babel 的設定檔 `.babelrc`，裡面指定用哪一個設定檔去打包

```javascript
{
  "presets" : ["@babel/preset-env"], // 符合大部分現代瀏覽器的
}
```

- 編輯 `webpack.config.js`，設定 `loader`
    - test: 用哪一種檔案
        - `$` 代表是用哪一種副檔名結尾
    - exclude: 排除
        - 因為 `node_modules/` 資料夾底下的應該都轉譯過了，這樣要花很久時間，所以先排除掉

設定完之後，當 webpack 在打包的時候看到 `.js` 結尾的檔案時，就會用 `babel-loader` 去把它轉化成 ES5 再 load 進來

```javascript
module.exports = {
  entry : './src/index.js',
  output : {
    path: path.join(__dirname, '/dist'),
    filename : 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/, 
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
}
```

- 之後一樣運行 `npm run start`，應該就可以看成 `bundle.js` 成功轉成 ES5 語法了！

---

## 3. 加入 React

- 安裝 react

```shell
yarn add react react-dom @babel/preset-react
```

- 更改 `.babelrc` 的 babel 規則，加入 `@babel/preset-react`

```javascript
{
  "presets" : ["@babel/preset-env", "@babel/preset-react"],
}
```

- `webpack.config.js` 設定不用改，新增一個 `App.js` 的檔案
- 寫一個簡單的 Component: `App`
    - 每個 Component 裡面都會有 `render` 這個 function，`return` 要輸出的內容

```javascript
import React, {Component} from 'react';

class App extends Component { // => Component
  render() {
    return (
      <h1>hello React</h1>
    )
  }
}

export default App; // 記得 export 出去
```

- 打開 `index.js`，把剛剛的測試刪掉

底下這段的意思是當成是執行到這段時，會找到 `#root` 這個元素，將 `App` 這個 Component render 在這個 `#root` 的 DOM 裡面。

```javascript
import React from 'react';
import ReactDom from 'react-dom';
import App from './App';

ReactDom.render(<App />, document.getElementById('root')); // => 將 APP render 到 #root 元素
```

- 一樣 `npm run start`，就會看到網頁上有 hello React

---

## 4. webpack dev server

以上就算環境建置完成了，但還是有美中不足的地方，接下來的步驟是讓開發環境更友善一點：

### ☞ 第一個優化： 檔名加入 hash 防止被 Cache

為了避免打包出來的 `bundle.js` 被瀏覽器 cache 住，webpack 有提供一個貼心功能 `hash`，將打包出來的 `bundle.js` 的檔名加入 hash 防止 cache

- 編輯 `webpack.config.js`，設定 `filename`

```javascript
filename: 'bundle.[hash].js'
```

所以打包出來的檔名會像 `bundle.a3jk23423adsf397s.js`。

但這樣就會碰到另一個問題了，index.html 不就要同時更新的 js 檔名嗎？

- 所以為了防止此問題，先安裝個 plugin: `html-webpack-plugin`，此 plugin 可以自動幫我們生成 html 的檔案

```shell
yarn add html-webpack-plugin
```

- 修改 `webpack.config.js`，新增一條 plugin 規則
- 將 js 檔都丟進 `src` 的資料夾裡
- 原本的 index.html 裡面的引入 script 就可以刪掉了

```javascript
const path = require('path');
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry : './src/index.js',
  output : {
    path: path.join(__dirname, '/dist'),
    filename : 'bundle.[hash].js'
  },
  module: {
    rules: [
      {
        test: /\.js$/, 
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  },
  plugins: [
    new htmlWebpackPlugin({
      template: './index.html' // => 以主目錄的 index 為範本
    })
  ]
}
```

- 再次運行 `npm run start`，會發現 `dist` 資料多了一個 `index.html`
- 打開 `dist/` 資料夾底下的 `index.html`，就會發現 webpack 幫我們加上有 hash 過後的 js 檔案了，非常聰明


---

### ☞ 第二個優化： webpack-dev-server 網頁自動更新

像這樣每做一個小修改就要重新 build bundle.js 也太麻煩了吧，所以現在要實現的功能是每次存檔就會自動更新網頁，不用再手動下指令。

所以這次的工具叫做 `webpack-dev-server`

- 安裝 `webpack-dev-server`

```shell
yarn add webpack-dev-server
```

- 接下來只要更改 `package.json` 就好了，修改 script 的 `start` 指令

```
  "scripts": {
    "start": "webpack-dev-server --mode development --open --hot",
    "build": "webpack --mode production",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

- 運行 `npm run start`，會自動打開 `localhost:8081/index.html` ，而所有的 js 檔案一但有修改並且儲存，就會即時反應出來！是不是方便多了～

---

## 環境建置超級懶人包

嫌以上步驟都太麻煩嗎？

其實 React 有提供一個很方便的指令，可以把上述步驟一鍵完成 ( 包括 hot reload )，只要照官網輸入：



```shell
npx create-react-app my-app
cd my-app
npm start
```

成功之後應該就會自動跑出 `localhost:3000` 的網頁囉！

其實這指令的背後跟我們剛剛做的事情是一樣的，只是他用一個指令把剛剛的步驟都包裝起來，但缺點是有可能安裝了許多你不需要的東西，功能比剛剛自己建置的還多，所以還是要看個人需求。