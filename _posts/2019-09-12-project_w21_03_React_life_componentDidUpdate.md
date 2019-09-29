---
title: "[第二十一週] React 生命週期：更新時期 componentDidUpdate、Lifecycle 總結"
layout: post_project
description: ""
category: project
tags: [Lifecycle, componentDidUpdate, project_week21]
file_name: 2019-09-12-project_w21_03_React_life_componentDidUpdate
---

## `componentDidUpdate` state 更新之後
現在要介紹另一個 lifecycle : `componentDidUpdate`

作用是什麼呢？

首先要知道 **`this.setState` 是非同步的**，當我們用 `this.setState` 改變 state 後，再馬上 log 出來，結果不一定是我們剛剛設置的 state。

所以其實 `this.setState` 有提供**第二個參數當 callback function** 的寫法：

```javascript
log() {
    console.log(this.state.num);
}

handleClick() {
    this.setState({
        num: 1
    }, this.log); // => callback，確保能取得最新的 state 狀態
}
```

但如果畫面上有很多改變 state 的 function，且需求是每次改變 state 之後都要 log 出來，難不成每個 `this.setState` 都要掛上 `this.log` 當作 callback function 嗎？

當然不是！**`componentDidUpdate()` 觸發的時機就是當 state 改變之後。**

那不如換個角度思考：「 與其在 `this.setState` 做 callback，既然所有的 state 改變都會觸發 `componentDidUpdate`，不如用把 `log()` 放在 `componentDidUpdate` 裡面。」

```javascript
componentDidUpdate(prevProps, prevState) {
    if (prevState.num !== this.state.num) { // => 比較更新前後的 state 屬性
        this.log();
    }
}
```

參數說明：
- `prevProps`: 更新前的 `props`
- `prevState`: 更新前的 `state`


### 特別注意

如果 `shouldComponentUpdate` 回傳 `false`，那麼 `componentDidUpdate` 也不會被觸發！

---

# React 生命週期：Lifecycle 總結

學到這邊就可以了解 Lifecycle 其實很簡單理解，就是「 一個 Component 從 "建立" 到 "更新" 到 "銷毀" 所會經歷的各個階段 」，而如果我們足夠理解各階段的觸發時間點，在操作 Component 的同時也會更游刃有餘。


非常推薦看這個網站：[react-lifecycle-methods-diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)，用圖像化表示 Lifecycle 變得超級清楚！

![螢幕快照 2019-09-12 下午4.15.20](https://i.imgur.com/Ib5gpFY.jpg)

( image from [react-lifecycle-methods-diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) )

#### Component 的 Lifecycle 大致來說可以分成三個大方向：
- Mounting : 建立初期
- Updating : 更新時期
- Unmounting: 銷毀時期

---

#### ☞ Mounting： 當 Component 被建立時

1. `constructer()`
2. `render()`
3. React 處理畫面到 DOM
4. `componentDidMount()`

#### ☞ Updating： 當 Component 更新時

當使用了 `setState`、或者 `props` 更新時

1. `shouldComponentUpdate()` => 如果 `return false`，就不會繼續往下跑
2. `render()`
3. React 處理畫面到 DOM
4. `componentDidUpdate()`

#### ☞ Unmounting： 當 Component 銷毀時

1. `componentWillUnmount()`
2. `render()`
3. React 處理畫面到 DOM


