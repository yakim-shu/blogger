---
title: "[第六週] CSS - 跟著 🐸🐸 學 Flex 排版"
layout: post
description: "排版是非常基本又重要的一環啊，以前都是似懂非懂，忘了再查資料就好，今天徹徹底底研究了一上午，總算對 flex 有了比較深的理解。想檢驗有沒有搞懂 flex，可以再玩一次 🐸🐸 遊戲，全程不能瞄到上面的提示。"
category: project
image: https://i.imgur.com/Iw1M6TP.jpg
tags: [CSS, flex, project_week6]
file_name: 2019-05-19-project_w6_CSS_flex
---

[FLEXBOX FROGGY 遊戲連結在此](http://flexboxfroggy.com)

排版是非常基本又重要的一環啊，以前都是似懂非懂，忘了再查資料就好，今天徹徹底底研究了一上午，總算對 flex 有了比較深的理解。

其實這遊戲當 flex 出現不久時就有了，但因為從沒來記起來過，我這次可能是玩第三次... 教訓是不做筆記不可能記得住，別相信你的腦袋。


以前很困擾的問題是，到底某屬性是下在**外容器**還是**內元素**，今天才發現其實子元素能設的有限，只有 `order`、`align-self`、`flex`... 大部分都是在外容器上操作。

想檢驗有沒有搞懂 flex，可以再玩一次 🐸🐸 遊戲，全程不能瞄到上面的提示。
（ 但也因為離很近、很難不看到就是了 ）

此筆記當作是在統整一次所有屬性，要理解 flexbox 最快的還是看圖片，推薦此範例：[Codpen - handy flexbox tool](https://codepen.io/vailjoy/live/pRgYbw)，很方便連字都不用打哈哈哈

---
## 外容器

### `display: flex`  Flex 排版起手式
要改成 flex 排版，外容器一定要加 `display: flex`

- `flex`
- `inline-flex`
    - 作用類似於 `inline-block` + `flex`

### `justify-content` 水平對齊方式 

( 正確應該說「 主軸線對齊 」，因為更改 `flex-direction` 有可能會換方向 )

- `flex-start`  靠左對齊 `預設值`
- `flex-end`  靠右對齊
- `center`  置中對齊
- `space-between`  分散對齊（ 左右邊不留間距 ）
- `space-around`  分散對齊（ 左右邊有留間距，最旁邊的間距為內間距的一半 ）
- `space-evenly`  均分對齊（ 左右邊有留間距，所有間距一致 ）
  - 比較新的屬性值，要注意瀏覽器支援度問題。

### `align-items` 垂直對齊方式

( 正確應該說「 交錯線對齊 」，因為更改 `flex-direction` 有可能會換方向 )

- `stretch` 元素裡的**內容撐滿**對齊 `預設值`
  - 會計算最長的元素高度，把所有元素都設成一樣高度
- `flex-start` 靠上對齊 
- `flex-end` 靠下對齊
- `center` 置中對齊
- `baseline` 元素裡的文字內容對齊 flex 基準線



### `flex-direction` 排列方向

- `row` 水平排列 `預設值`
- `row-reverse`  反向的水平排列
- `column` 垂直排列
- `column-reverse` 反向的垂直排列

> 注意：如改成垂直排列 `column`，調整水平 & 垂直的方式會倒過來

- `justify-content` 會變成「 調整垂直對齊 」
- `align-items` 會變成 「 調整水平對齊 」

如下圖，原本垂直往下對齊是用 `align-items`，但此時要使用 `justify-content` 更改。

![column 對齊方向相反](https://i.imgur.com/WhhkFWz.jpg)
![column 對齊方向相反2](https://i.imgur.com/6P00c4t.jpg)

> 注意：如有使用反向排列：`row-reverse` or `column-reverse`

- `justify-cotent` 的 `flex-start` 會變成 「 靠右對齊 」
- `justify-cotent` 的 `flex-end` 會變成 「 靠左對齊 」，如下圖。

![reverse - justify-content 方向相反](https://i.imgur.com/DydL4zV.jpg)
![reverse - justify-content 方向相反2](https://i.imgur.com/B3YvTuz.jpg)


### `flex-wrap` 元素超過範圍是否換行
當頁面寬度過窄，以致於內容無法完整呈現的時候，是否要進行換行。

- `nowrap`: 所有元素排在同一行 `預設值`
- `wrap`: 元素會換行
- `wrap-reverse`: 元素會換行，且反轉方向

### `flex-flow` 排列方向 與 斷行方式

其實就是另一種縮寫，結合 `flex-direction` & `flex-wrap`。
- `<'flex-direction'> <'lex-wrap'>`

### `align-content` 多行元素的垂直對齊

其實就是 `align-items` 的多行版本，裡面有需要換行的元素就可以使用 `align-content`，若元素只有單行則無作用。

- `stretch`: 所有元素 撐滿上下邊 `預設值`
- `flex-start`: 緊密 靠上對齊
- `flex-end`: 緊密 靠下對齊
- `center: Lines`: 緊密 置中對齊
- `space-between`: 分散對齊（ 上下不留間距 ）
- `space-around`: 分散對齊（ 上下有留間距 ）


---
## 子元素 

### `order` 單個元素的排列順序

設置 `order` 可以重新定義元件的排列順序，順序會依據數值的大小排列。
( 這在 RWD 想改元素位置，應該非常實用！ )

- `整數` : `預設值為 0`
- 順序： 最小（可能為負數）→ 最大 

### `align-self` 該元素的垂直對齊方式
- `flex-start` 靠上對齊
- `flex-end` 靠下對齊
- `center` 置中對齊
- `baseline` 基準線對齊
- `stretch` 撐滿上下邊

---

> 以下三項比較難懂，可以看完說明再去 [Codpen - handy flexbox tool](https://codepen.io/vailjoy/live/pRgYbw) 改改數值，大概就能夠理解！

### `flex-grow` 該元素佔用容器剩餘的比例
可以當作是 **平分外層容器「 剩餘的位置 」**。
如果子元素加起來的空間大於外容器，設置 `flow-grow` 則不會起作用。
- `整數` : `預設值為 0`

### `flex-shrink` 該元素壓縮比例
當外層空間不夠時，壓縮元素的比例
- `整數` : `預設值為 1`

### `flex-basis` 該元素的基準值，類似於 `min-width`
可使用不同單位，意思是一開始就會分配給該元素多少的空間，不會被壓縮到。
如果同時設置 `flex-basis` 跟 `width`， `width` 會被蓋掉。
- `<num> px` or `<num> %` : `預設值為 auto`

### `flex` 全部寫在一起
結合剛剛介紹的三項 `flex-grow`、`flex-shrink`、`flex-basis`
- 例如： `flex: 1 2 50px`
- 預設值： `0 1 auto`
- 如果只寫一個數值，代表 `flex-grow`
    - `flow: 1` ( 如果只有一個子元素設置，代表該子元素佔滿「 外容器所有的剩餘空間 」 )
 

---
### 總整理

父容器可設置的屬性：
```css
.flex_container {
  /* flex 排版 */
  display: flex | inline-flex;
  /* 水平對齊 */
  justify-content: flex-start | flex-end | center | space-between | space-around;
  /* 垂直對齊 （ 單一行 ） */
  align-items: flex-start | flex-end | center | baseline | stretch;
  /* 排列方向 */
  flex-direction: row | row-reverse | column | column-reverse;
  /* 換行方式 */
  flex-wrap: nowrap | wrap | wrap-reverse;
  /* 結合 排列方向 與 換行方式 */
  flex-flow: <flex-direction> <flex-wrap>;
  /* 多行的垂直對齊 */
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

子元素可設置的屬性：
```css
.flex_item {
  /* 該元素的排列位置 */
  order: <number>;
  /* 該元素佔用容器剩餘的比例 */
  flex-grow: <number>;
  /* 該元素壓縮比例 */
  flex-shrink: <number>; 
  /* 該元素的最小值 */
  flex-basis: <length> | auto;
  /* 結合上面 3 種屬性 */
  flex: <'flex-grow'> <'flex-shrink'> <'flex-basis'>
}
```

---
## 實際應用

如果是用 float 來切版，下面這種基本版面也是靠杯難切。
- 所以 [此範例就是來演示 flex 的好](https://codepen.io/anon/pen/NVvroV)，為行動優先、先手機版再到電腦版 
- 當切換到電腦版時，主要內容設 `flex-grow` ，再更改其他區塊排列位置 `order`，就這樣完成了！

<iframe height="400" style="width: 100%;" scrolling="no" title="Demo Flexbox 3" src="//codepen.io/chriscoyier/embed/vWEMWw/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/chriscoyier/pen/vWEMWw/'>Demo Flexbox 3</a> by Chris Coyier 
  (<a href='https://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
( demo from [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) )


參考資料：
- [圖解：CSS Flex 屬性一點也不難](https://wcc723.github.io/css/2017/07/21/css-flex/)
- [簡單介紹 CSS Flexbox（上）](https://medium.com/@xz051571t/簡單介紹-css-flexbox-上-ad82cb11eb1e)
- [深入理解css3中的flex-grow、flex-shrink、flex-basis](http://zhoon.github.io/css3/2014/08/23/flex.html)
- [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [Master CSS Flexbox in 5 Simple Steps](https://webdesignerwall.com/tutorials/master-css-flexbox-5-simple-steps)
- [How can I make this f*****g layout?](https://www.slideshare.net/DavideDiPumpo/understanding-flex-box-css-day-2016)