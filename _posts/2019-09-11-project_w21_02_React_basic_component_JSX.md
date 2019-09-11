---
title: "[第二十一週] React 基礎：Component、JSX 語法、事件機制"
layout: post_project
description: ""
category: project
tags: [React, component, JSX, project_week21]
file_name: 2019-09-11-project_w21_02_React_basic_component_JSX
---

## 在 React 中所有元素皆為 Component
React 基本元素就是 Component，一整個網頁就是由不同的 Component 組成。

```javascript
ReactDom.render(<App />, document.getElementById('root')); // => 將 APP render 到 #root 元素
```

像之前的範例，`App` 就是一個 Component，會渲染到 `#root` 這個 DOM 物件裡面。

```javascript
import React, {Component} from 'react';

class App extends Component {
  render() { // => 每個 Component 都要有 render function
    return (
      <h1>hello React</h1>
    )
  }
}

export default App;
```

**而 `render` function 是 Component 很重要的一環，每一個 Component 都一定要有 `render()`**，沒有加上 `render()` 的話會報錯誤訊息。

---

## Component `<App />`

`<Component />` 這樣是一個 Component 的既定寫法，有點像 HTML 的 tag 寫法。

所以也可以在 `render()` 裡面放另一個 Component，像**以下範例就是在 `<App />` render 裡面放 `<Title />` 跟 `<Text />`  的 Component。**

（ 要注意定義 Class 的時候一定要 `extends Component`，不然會報錯 ） 

```javascript
class Title extends Component {
  render() {
    return <h2>title</h2>
  }
}

class Text extends Component {
  render() {
    return <p>text</p>
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Title /> // => Component
        <Text /> // => Component
      </div>
    )
  }
}
```

---

## JSX

而像這樣 `<Component />` 有點像 HTML 的語法叫做 JSX，有點像是 HTML 跟 JavaScript 的混合體。

所以寫 JSX 就像是在寫 HTML 般很好上手，但還是有些微小差異：
- **`<div class="">` 的寫法要寫成 `<div className="">`**，不然會報錯
- 用**大括號 `{ }` 包起來的內部可以放 JavaScript 語法**
- inline style 裡面是放 object
    - 且在 React 裡面寫 inline-style，**用 `-` 連接的 CSS 屬性名稱都會轉成駝峰式命名：**
        - `font-size` => `fontSize`

```javascript
class App extends Component {
  render() {
    const name = 'title';
    const style = {
      color: 'grey',
      fontSize: '30px', // => Camel-Case
    }
    return (
      <div className={name + '1'} style={style} data-id='1'> // => className
        <Title />
        <Text />
      </div>
    )
  }
}
```

---

## React 的事件機制

傳統寫網頁的時候，想要對按鈕添加事件監聽會像以下範例：

```javascript
function sayHi() {
  alert('hi!');
}
document.querySelector('.button').addEventListener('click', sayHi);
```

但因為寫 React 是把 DOM 跟 JavaScript 寫在一起，所以我們可以直接事件監聽寫在 DOM 上面：

```javascript
class Title extends Component {
  render() {
    function sayHi() {
      alert('hi!');
    }
    return <h2 onClick={sayHi}>title</h2>
  }
}

/* 或者把上面範例改寫，簡化成箭頭函式 */
class Title extends Component {
  render() {
    return <h2 onClick={() => alert('hi')}>title</h2>
  }
}
```


---

## 結論
- 每個 Component 都一定要有 `render` function
- JSX 乍看跟 HTML 語法很像，但還是有些微小差異
  - `class` => `className`
  - 大括號： `{` 可以放 Javascript 語法 `}`
  - `font-size` => `fontSize`
- 事件直接加在 DOM 上面： `onClick`