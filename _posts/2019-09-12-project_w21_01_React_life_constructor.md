---
title: "[第二十一週] React 生命週期： 建立初期 constructor、componentDidMount"
layout: post_project
description: ""
category: project
tags: [Lifecycle, constructor,constructor、componentDidMount, project_week21]
file_name: 2019-09-12-project_w21_01_React_life_constructor
---

## constructor 的作用
在 React 開發裡面有一個很重要的觀念就是 Lifecycle，可以想像成 Component 的生命週期，包括什麼時候產生、更新、結束等等的階段。

這單元是要介紹生命週期的開始： `constructor`。

`constructor()` 觸發的時間點就是在 **「 Component 執行 `render()` 之前 」**

```javascript
class App extends Component {
  constructor() {
    super();
    console.log('app created');
  }

  render() {
    console.log('call render');
    return (
      <div>
      </div>
    )
  }
}
```

看以上範例，當我們一打開網頁就可以看到主控台先 `app created` 的訊息才有 `call render`。

代表在執行到 `constructor` 那段，元件就已經被 new 出來了，所以才會執行 `console.log('app created')`。



---
#### 小範例： 控制開關
而這邊來示範一個「 用按鈕去控制元素的顯示與否 」的小程式，按下 `button` 之後 `<h1>` 就會不斷得開關，看主控台會發現不停顯示 `Title created`。

```javascript
class Title extends Component {
  constructor() {
    super();
    console.log('Title created');
  }
  render() {
    return <h1>Title</h1>
  }
}

class App extends Component {
  constructor() {
    super();
    this.state = {
      isShow: true
    }
    console.log('app created');
  }

  render() {
    const {isShow} = this.state;
    return (
      <div>
        <button onClick={() => { // => 按下按鈕會觸發 onClick 事件
          this.setState({
            isShow: !this.state.isShow
          })
        }}>Toggle</button>
        {isShow && <Title />} // => 如果是 ture 才會 render <Title>
      </div>
      
    )
  }
}
```

而這是為什麼呢？

因為我們是用 `{isShow && <Title />}` 來決定要不要有 `<Title />` 這個 Component。

當 `isShow` 為 `false`，`<Title />` 等於是從網頁上移除，等到再按一次變成 `true`，**`<Title />` 才會再重新建立一次**，所以才會看到 `constructor` 的 console 訊息不斷出現。

而如果不想要讓整個 Component 移除又重新建立的話，就用 CSS 控制就好了！

```javascript
class Title extends Component {
  constructor() {
    super();
    console.log('Title created');
  }
  render() {
    const {isShow} = this.props;
    return <h1 style={{
      display: isShow ? 'block' : 'none' // => 用 CSS 控制
    }}>Title</h1>
  }
}

class App extends Component {
  constructor() {
    super();
    this.state = {
      isShow: true
    }
    console.log('app created');
  }

  render() {
    const {isShow} = this.state;
    return (
      <div>
        <button onClick={() => {
          this.setState({
            isShow: !this.state.isShow
          })
        }}>Toggle</button>
        <Title isShow={isShow}/> // => 把狀態傳進去
      </div>
      
    )
  }
}
```


如此一來，畫面上看起來是相同的，但主控台就沒有重複出現 `Title created` 的訊息了

---

### 特別注意： constructor 的 props

要特別注意的一點是，如果你在 constructor 要存取 `this.props` 的值，習慣都要加上參數 `props`，包括 `super(props)` 也是，以盡量避免錯誤的產生。

```javascript
class Title extends Component {
  constructor(props) { // => props
    super(props); // => props 
    console.log(this.props); // => 如果前面兩個參數忘記加，這裡會無法存取
    console.log('Title created');
  }
}
```

### 結論
- constructor 就是 Component 在建立時，會自動執行的一個 function。
- 在寫 constructor 時要記得加上 `props` 參數，確保被正確初始化。而如果 Component 沒有什麼特別需要初始化的可以不用寫 constructor
- 當你在 React 要隱藏一個元素的話有兩種做法
    - 用條件判斷要 render 語法（ Component 會被徹底移除 ）
    - 用 CSS 控制（ Component 還在畫面上，只是被隱藏起來 ）


---

# componentDidMount 掛載時期

之前有提到說 Component 在建立的時候，會執行 `constructor`，可是建立歸建立，此時還沒 render 到 DOM 上面去，那 render 到 DOM 的動作稱為「 **Mount 掛載** 」。

所以只有在 `componentDidMount` 這個 function 被觸發以後，Component 才會真正的出現在 DOM 上面。


`ComponentDidMount()` 的時間點就是在 **「 `constructor()` 執行之後、且執行`render()` 之後 」**

```javascript
class App extends Component {
  constructor(props) {
    super(props);
    // => 選不到 button，因為還沒加到 DOM 裡面
    console.log('1. construcotr: ', document.querySelector('.test'));
  }

  componentDidMount() {
    // => 成功選取 button，因為此階段已經加到 Dom 裡面
    console.log('3. mount: ', document.querySelector('.test'));
  }

  render() {
    console.log('2. render');
    return (
      <div>
        <button className='test'></button>
      </div>
    )
  }
}
```

可以看到 console 依序列出以下訊息：

```
1. construcotr:  null
2. render
3. mount:  <button class=​"test">​</button>​
```

所以 `componentDidMount` 實際上要做什麼呢？

最適合做的就是一些初始化相關的事情，而這就會談到另一個 function: `componentWillUnmount`

先把把之前的開關範例拿來示範，button 是控制 Title 的開關：

<div class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="yakim-shu" data-slug-hash="aboGXmZ" data-prefill='{"title":"[React] toggle component width ComponentDidMount","tags":[],"stylesheets":[],"scripts":["https://unpkg.com/react/umd/react.development.js","https://unpkg.com/react-dom/umd/react-dom.development.js"]}'>
  <pre data-lang="html">&lt;div id="root">
    &lt;!-- This element's contents will be replaced with your component. -->
&lt;/div>
</pre>
  <pre data-lang="css" ></pre>
  <pre data-lang="babel">class Title extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      title: 'Titile'
    }
  }

  render() {
    const { title } = this.state;
    return &lt;h1>{title}&lt;/h1>
  }
}

class App extends React.Component {
  constructor() {
    super();
    this.state = {
      isShow: true,
    }
  }

  render() {
    const { isShow } = this.state;
    return (
      &lt;div>
        &lt;button onClick={() => {
          this.setState({
            isShow: !this.state.isShow
          })
        }}>Toggle&lt;/button>
        {isShow && &lt;Title />}
      &lt;/div>

    )
  }
}

ReactDOM.render(
  &lt;App />,
  document.getElementById('root')
);</pre>
</div>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>




然而如果我想要在 Title 出現兩秒後，變換文字 => `new Title`，所以在 `<Title />` 這個 Component 使用 `componentDidMount` 加上一個 `setTimout` 去換文字。

```javascript
class Title extends Component {
  constructor(props) {
    super(props);
    this.state = {
      title: 'Titile'
    }
  }

  // => 加入這段，過兩秒後會變成 new Title
  componentDidMount() {
    setTimeout(() => {
      console.log('changed title!');
      this.setState({
        title: 'new Title'
      })
    }, 2000);
  }

  render() {
    const { title } = this.state;
    return <h1>{title}</h1>
  }
}
```

### 發現問題： 無法對已銷毀的 Component 設定 state

但問題來了！

當網頁一載入進來，我就**立刻把 Title 關掉，也代表 Title 這個 Component 已經被銷毀了，等兩秒時間到，剛剛設定好的 Timer 就找不到 `this.setState` 是哪一個 Component 的 state 了**，所以會看到 console 出現錯誤訊息。

---

### 解決之道： componentWillUnmount （ 銷毀 Component 之前要做的事 ）



所以此時正是 `componentWillUnmount` 登場的時候。

我們幫 Title 這個 Component 再加上 `componentWillUnmount` 來消除剛剛的 Timer，意思就是告訴 React「 當這個 Title 銷毀之前，也幫我剛剛加上去的 **Timer 清掉**。 」

綜合以上，改寫 Title Component：

```javascript
class Title extends Component {
  constructor(props) {
    super(props);
    this.state = {
      title: 'Titile'
    }
  }

  componentDidMount() {
    // => 將 timer 存起來
    this.timer = setTimeout(() => {
      console.log('changed title!');
      this.setState({
        title: 'new Title'
      })
    }, 2000);
  }

  componentWillUnmount() {
    // => 當 Component 要被銷毀時，也要順便清除 timer
    clearTimeout(this.timer);
  }

  render() {
    const { title } = this.state;
    return <h1>{title}</h1>
  }
}
```

如此一來，怎麼開關 Title 都不再有問題。

### codepen 範例
<div class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="yakim-shu" data-slug-hash="QWLrYOR" data-prefill='{"title":"[React] toggle component width componentDidMount &amp; componentWillUnmount","tags":[],"stylesheets":[],"scripts":["https://unpkg.com/react/umd/react.development.js","https://unpkg.com/react-dom/umd/react-dom.development.js"]}'>
  <pre data-lang="html">&lt;div id="root">
    &lt;!-- This element's contents will be replaced with your component. -->
&lt;/div>
</pre>
  <pre data-lang="css" ></pre>
  <pre data-lang="babel">class Title extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      title: 'Titile'
    }
  }

  componentDidMount() {
    this.timer = setTimeout(() => {
      console.log('changed title!');
      this.setState({
        title: 'new Title'
      })
    }, 2000);
  }

  componentWillUnmount() {
    clearTimeout(this.timer);
  }

  render() {
    const { title } = this.state;
    return &lt;h1>{title}&lt;/h1>
  }
}

class App extends React.Component {
  constructor() {
    super();
    this.state = {
      isShow: true,
    }
  }

  render() {
    const { isShow } = this.state;
    return (
      &lt;div>
        &lt;button onClick={() => {
          this.setState({
            isShow: !this.state.isShow
          })
        }}>Toggle&lt;/button>
        {isShow && &lt;Title />}
      &lt;/div>

    )
  }
}

ReactDOM.render(
  &lt;App />,
  document.getElementById('root')
);</pre>
</div>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

範例跟預期結果一致，**當 Title 一出現時文字是 `Title`，過兩秒後變成 `new Title`**，再關掉又打開也是一樣。
 
---

## 結論
這兩個函式大部分會是成對出現，在不同時機負責不同任務：
- Component render 之後的初始化： `componentDidMount`
- Component 銷毀之前取消 ``componentDidMount`` 的動作： `componentWillUnmount`
