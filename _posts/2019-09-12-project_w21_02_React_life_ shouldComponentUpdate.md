---
title: "[第二十一週] React 生命週期： 性能優化 shouldComponentUpdate、更方便的 PureComponent"
layout: post_project
description: ""
category: project
tags: [Lifecycle, shouldComponentUpdate, PureComponent, project_week21]
file_name: 2019-09-12-project_w21_02_React_life_ shouldComponentUpdate
---

## 決定要不要 render: shouldComponentUpdate

這個 Lifecycle 其實非常直覺，看字面意思就是說「 決定 Lifecycle 是不是該 update ? 」

它在**被呼叫的時間點為：改變 state 之後、執行 `render()` 之前**。

所以如果沒有特別指定的話，預設會回傳 `true`。

以下是一個按 `click me` 按鈕會增加 counter 的小程式：

```javascript
const Counter = props => {
  return <div>{props.counter}</div>;
}
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      counter: 1
    }
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({
      counter: this.state.counter + 1
    })
  }

  shouldComponentUpdate() {
    console.log('shouldComponentUpdate is called'); // => 出現 1.
    return true; // => 就是預設值，有寫沒寫都一樣
  }

  render() {
    console.log('render'); // => 出現 2.
    const {isShow} = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>Click me</button>
        <Counter counter={this.state.counter}/>
      </div>
      
    )
  }
}
```

### 回傳值決定要不要 call `render()`
會發現每按一下按鈕，console 會依序出現 `shouldComponentUpdate is called` 跟 `render`，代表先執行 `shouldComponentUpdate` 再執行 `render`。

那如果我把 `shouldComponentUpdate()` 回傳值改為 `false` 呢？

會發現按鈕突然無法 `render` 了，沒錯！

**`shouldComponentUpdate()` 的回傳值決定了 「 當 Component 的 state 改變時，要不要去 call `render()` function 」**

那既然 state 改變，不就要更新 UI 嗎？ 還是說有什麼情況要把回傳值改成 `false` 呢？

有的，當遇到以下兩種情況，可以使用參數 `nextProps` & `nextState` 來進行檢查：
- 在某種條件下，不想要更新畫面時
- 檢查 `state` or `props` 是否有更新，沒有更新就不必要浪費資源重新渲染

---

### 參數 `nextProps` & `nextState`

#### ☞ `nextState` 改變後的 state

`nextState` 就是改變後的 state，所以我們可以依據 state 的狀態，去決定要不要 call `render()`，例如以下改成，如果 `counter` 超過 `3`，畫面就不要再有變動：

```javascript
shouldComponentUpdate(nextProps, nextState) {
    console.log('nextState: ', nextState);
    if (nextState.counter > 3) return false; // => 超過 3 就不要 call render()
    return true;
}
```

看 console 可以知道，實際上 `counter` 已經增加到 `6` 了，但畫面上的 `counter` 數量還是停留在 `3`

![螢幕快照 2019-09-11 下午8.27.49](https://i.imgur.com/V2nJtaw.jpg)

---
#### ☞ `nextProps` 改變後的 props

`nextProps` 跟 `nextState` 很像，只是目標是 props，實際的用途比較常拿來做**檢查用**。

假設我們有個 `<Title />` Component，外面還有包一個 Component。

`<Title />` 本身並不常更新，但如果 parent Component 要 call `render()`，children Component 也會跟著 `render()`。

所以當 `<Title />` 本身狀態並沒有改變的話，其實是沒必要的動作，此時就很適合用 `nextProps` 來進行檢查。

```javascript
class Title extends Component {
  shouldComponentUpdate(nextProps, nextState) {
    // => 改變後的 props 跟原本一樣的話就不要 call render()
    if (nextProps.title !== this.props.title) return true;   
    return false;
    
    // [ 注意 ] 以下錯誤寫法： 不能只寫一個情況。
    // if (nextProps.title === this.props.title) return false;  
  }

  render() {
    console.log('render title');
    return <div>{this.props.title}</div>
  }  
}
```

---

## 更方便的 Component: `PureComponent`

以上可以知道 `shouldComponentUpdate` 能夠幫助我們做兩件事：
1. 某些特定條件下不要 `render()`
2. 當 `props` or `state` 沒有變動時，不要 `render()`

就第 2 個條件來說，難道每次都要手動比對，沒有更方便的作法嗎？

有的！相較於以前介紹過的 `React.Component` 跟 `functional Component`，還有一個叫做 「 **`PureComponent`** 」。

它的方便之處在於會幫你做 shallowly compares，意思是說會自動偵測 `props` or `state` 的變動，沒有更新就不會 call `render()`。

你可以把以下兩種寫法視為相同的：

```javascript
// 1. 使用 PureComponent 會自動 shallowly compares
class Title extends PureComponent {
    render() {
    ...
  }  
}

// 2. 自己寫 shouldComponentUpdate 判斷
class Title extends Component {
  shouldComponentUpdate(nextProps, nextState){
    return !shallowEqual(this.props, nextProps) || !shallowEqual(this.state, nextState);
  }

  render() {
    ...
  }  
}


```

### PureComponent 的適用場景

這樣 `PureComponent` 聽起來不是超棒的嗎？ 全部改成 `PureComponent` 就好了啊！

沒有這麼萬用喔，最好符合以下兩個條件，才使用 `PureComponent`:

#### 1. **`state` or `props` 很少變動的元件** 
不然因為每次都進行 `shallowEqual()` 比對，如果元件一直頻繁更新，根本沒必要阻止 `render()`，到頭來反而會拖慢效率。

#### 2. **`state` or `props`結構簡單的情況**
因為 `PureComponent` 只會幫你做 `shallowEqual()`，如果是多層的陣列或物件並沒有用處

所以結構複雜的話可以考慮用 `shouldComponentUpdate` 搭配 library，例如 [`Immutable.js`](https://facebook.github.io/immutable-js/)

---
參考資料：
- [ React 官方文件](https://reactjs.org/docs/react-api.html#reactpurecomponent)
- [React 性能優化大挑戰：一次理解 Immutable data 跟 shouldComponentUpdate](https://blog.techbridge.cc/2018/01/05/react-render-optimization/)
- [[React.js] 如何提高 React App 的效能](https://larry850806.github.io/2016/07/25/react-optimization/)