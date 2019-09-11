---
title: "[第二十一週] React 基礎：Functional Componet & 父子 Component 之間的溝通"
layout: post_project
description: ""
category: project
tags: [React, functional-component, project_week21]
file_name: 2019-09-11-project_w21_04_React_basic_functional-component
---

## Functional Componet
之前用的 Component 都是用 class 的形式來寫 ( `Class Component` )，而其實有另外一種方式寫 Component，叫做 `functional Component`。

以下是 `Counter` 這個 Component 用不同形式來寫：

```javascript
/* Class Component */
class Counter extends Component {
  render() {
    console.log('Counter-render');
    return <div>{this.props.number}</div>;
  }
}

/* Functional Component */
function Counter(props) { // => 一樣有 props 的屬性可以使用
  return <div>{props.number}</div>
}

/* Functional Component - 或是更進階一點，使用解構語法 */
function Counter({number}) { // => 其實就是 const {number} = props
  return <div>{number}</div>
}
```

### Class Component 跟 Functional Component 的差異

那寫 Component 時要怎麼決定用哪種寫法呢？

有個很簡單的判斷技巧是，當你只是要寫個很單純的 Component，不需要儲存 `state` 也沒有其他事件監聽，**功能只有 `render` 畫面時，就很適合用 Functional Component**，因為語法比起 Class Component 會比較簡潔一點。

--- 

## 父子 Component 之間的溝通方式

在 React 裡面， children Component 是沒有辦法改變 parent Component 的 `state`，所以**最好的方式是把 parent 改變 `state` 的 function 傳下去給 children，當成其中一個 props 的屬性。**

```javascript
const Title = (props) => {
  return (
    // => 接收到 App 傳來的 handleClick，才能藉此改變 App 的 state
    <h1 onClick={props.handleClick}>{props.text}</h1>
  )
}

class App extends Component {
  constructor() {
    super();
    this.state = {
      counter : 1
    }
    
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({
      counter: this.state.counter + 1
    })
  }
  render() {
    return (
      <div>
        // => 把能夠改變 state 的 handleClick 傳給 Title
        <Title handleClick={this.handleClick} text={this.state.counter}/>
      </div>
    )
  }
}
```

此時不斷得點擊 `<h1>` 就可以看到數字增加。

### codepen 範例

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="yakim-shu" data-slug-hash="xxKjXrq" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="[React] change state by props">
  <span>See the Pen <a href="https://codepen.io/yakim-shu/pen/xxKjXrq/">
  [React] change state by props</a> by Yakim Shu (<a href="https://codepen.io/yakim-shu">@yakim-shu</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


#### 步步拆解以上的流程解析：
- Title
    - render Title props.text => `App's state.counter: 1`
    - props.handleClick => `App.handleClick: func`
- Trigger Title => h1 => onClick => props.handleClick => `App.handleClick`
- App.handleClick => setState => `counter : 2`
- **App state changed => re-render**
- render Title props.text => `App's state.counter: 2`

### 結論： children 必須透過 parent 傳進來的 function 來改變 `props`

所以重點是**當 children Component `<Title />` 要更新傳遞進來的 `props` 時，是沒有辦法直接對 `props` 做更改的，必須透過 parent Component `<App />`  傳遞進來的 function 做更新才行。**

---

## props function 帶參數的寫法
把剛剛的範例改寫成「 每點一次，counter 就變成一個隨機數 」，示範傳進去的 function 有帶參數的例子，變成很像在寫 callback function：

```javascript
const Title = (props) => {
  return (
    <h1 onClick={() => {
      props.changeCounter(Math.random()) // => 就像一個 callback function 的寫法
    }}>{props.text}</h1>
  )
}

class App extends Component {
  constructor() {
    super();
    this.state = {
      counter : 1
    }
    
    this.changeCounter = this.changeCounter.bind(this); // => 不要忘了 bind
  }

  changeCounter(num) {
    this.setState({
      counter: num
    })
  }
  render() {
    return (
      <div>
         // => 一樣把能夠改變 App state 的 function 傳進去
        <Title changeCounter={this.changeCounter}  text={this.state.counter}/>
      </div>
    )
  }
}
```

### codepen 範例

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="yakim-shu" data-slug-hash="qBWYPMV" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="[React] change state with random number by props">
  <span>See the Pen <a href="https://codepen.io/yakim-shu/pen/qBWYPMV/">
  [React] change state with random number by props</a> by Yakim Shu (<a href="https://codepen.io/yakim-shu">@yakim-shu</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 結論

所以要記住的重要觀念就是，只有 Component 可以改變自己的 state，如果要改變其他 Component 的 state ，只有該 Component 有提供的方法才可行。