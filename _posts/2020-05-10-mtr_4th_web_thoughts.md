---
title: "[開發紀錄] Lidemy 第四期程式導師計畫官網"
layout: post
description: "Web"
category: project
tags: [web]
file_name: 2020-05-10-mtr_4th_web_thoughts
image: https://i.imgur.com/Q32VUbm.jpg
---

## 緣起

本篇是這次製作 Lidemy 第四期程式導師計畫官網的紀錄與心得，感覺很久沒做網站了，感謝 Huli 讓我這個第三期學生可以有點回報，也因為被嗆我落格雜草叢生，決定還是來記錄一下最近在忙的事情。

> 官網前期規劃是 Huli 跟 Minw 兩人籌劃的，但由於大家都非常隨性，官網就是在幾次聚會「 聊天 80%、討論公事 20% 」 中誕生，從**開始畫 wireframe -> 設計 -> 我與 ChihYang41 負責開發，這段過程大概為期兩個月吧。**

不過因為太久沒做設計了，很常自己畫完又不滿意想改，這些猶豫不定修設計的過程壓縮到開發時間，導致本來想加入很多很酷的效果，到最後時間有限就放棄是挺可惜的ＱＱ。

對，所以最後網站就是走一個返璞歸真風格。以下大致上會紀錄這次用到的工具與優缺點：

---

## 設計階段

這是我第一次用 [figma](https://www.figma.com/) 這套線上設計工具，一開始是看公司設計用這套，研究一下發現不得了，雖然這次設計只有我一人，其實用不到 figma 最厲害的多人協作功能，但對於小組溝通討論的情境也是超級方便：

1. 跨平台（ 雖然我電腦太弱，用網頁版會一直發出記憶體不足的警告 QQ
2. 分享設計稿很容易，直接發佈網址
3. 可以看各時期歷史版本
4. 觀看者可以直接在設計稿留下評論
    - 這是我最喜歡的一點，因為協作者可以直接指出哪裡需要修改，減少設計跟工程的溝通成本

除了方便溝通之外，設計上還有很多好用的地方，像是 layout grid 系統及 [component](https://medium.com/design-with-figma/10-tips-on-using-components-in-figma-c7db9c5e7fe1) 的概念我都很喜歡。

最後因為我完全是畫畫苦手，所以網頁上能看到的可愛插畫都是來自 [GostIllustrator](https://getillustrations.com/illustration-pack/ghost-illustrations-builder)，免費且可供商業使用，簡直佛心！

---

## 技術棧

沒有任何很潮的技術，都是一些基本到不行的工具。

這次有特別用一些沒用過的工具，例如像 [教學成果頁](https://bootcamp.lidemy.com/achievement.html) 的圖表，用 [charts.js](https://www.chartjs.org/) 根本超簡單，把資料丟進去 `data`、設定丟進 `options`，超快就搞定！

- HTML
    - [pug](https://pugjs.org/api/getting-started.html)
- CSS
    - [uncss](https://github.com/uncss/uncss)
- Performance
    - [lazyload](https://github.com/verlok/lazyload)
    - [gulp-svg-sprite](https://github.com/jkphl/gulp-svg-sprite)
- Other
    - [clipboard.js](https://clipboardjs.com/)
    - [rellax](https://www.cssscript.com/demo/lightweight-vanilla-javascript-parallax-library-rellax/)
- Data Visualization
    - [charts.js](https://www.chartjs.org/)
- build tool
    - [gulp](https://gulpjs.com/)

---

### 💡 Svg sprite

- plugin: [gulp-svg-sprite](https://github.com/jkphl/gulp-svg-sprite)

因為貪圖方便又不想失真，大部分圖片都是輸出 svg，然後再使用 [gulp-svg-sprite](https://github.com/jkphl/gulp-svg-sprite) 轉成 sprite 減少圖片請求。

託文件寫得很清楚的福，其實轉 sprite 比想像中簡單很多，有興趣的可以看看 [Basic example](https://github.com/jkphl/gulp-svg-sprite#basic-example)，非常建議把 `config.mode.example` 打開，跑完會生成一個範例 html 檔，能幫助你快速搞懂怎麼使用。

但還是有些小雷區：

1. 輸出可以設定 `maxWidth`, `maxHeight`，所以我覺得不太適合把尺寸差異很大的圖都放進同一張 sprite，儘管可以設定每張小圖之間的間距，但還是滿容易露餡的（ `background-position` 位置沒有對準 )
2. 很難微調圖片（ ex: `hover` or RWD 時 `background-size` 要變化... ）

輸出的 sprite 圖：

![-w648](https://i.imgur.com/HPfMguc.jpg){:width="400"}


---
 
### 💡 Lazy load

- plugin: [lazyload](https://github.com/verlok/lazyload)

其實在這官網案例中是不太需要 lazy load 的，因為圖片其實不多 size 也小，能做 sprite 我也都先做了，但最後我還是手癢加上去，典型的為加而加 😂 。

而且因為沒有處理 placeholder image（通常會是模糊縮圖，或自訂的圖），反倒當使用者 scroll 到該位置時，瞬間載入圖片造成網頁佈局 reflow，對 UX 是一種傷害嗚嗚。

如果想要避免網頁因 lazy load 而造成的 reflow，可以參考這篇：[Preventing Content Reflow From Lazy-Loaded Images](https://css-tricks.com/preventing-content-reflow-from-lazy-loaded-images/)，而**我使用的套件的建議是不要用 placeholder，而是用 padding-bottom** 直接把該有的高度撐出來，有興趣的人可以參考文件上的建議：[vertical-padding-trick](https://github.com/verlok/lazyload#vertical-padding-trick) 

( 但無論哪種我都覺得好囉唆... )

```css
.image-wrapper {
    width: 100%;
    height: 0;
    padding-bottom: 150%; /* You define this doing image height / width * 100% */
    position: relative;
}
.image {
    width: 100%;
    height: auto;
    position: absolute;
}
```

---

### 💡 Remove unused CSS

- plugin: [uncss]((https://github.com/uncss/uncss))

之前做 [效能優化作業](https://github.com/Lidemy/lazy-hackathon/issues/7) 時就覺得這套工具超神，這次當然也免不了它，不過之前用的 [gulp-uncss](https://github.com/ben-eb/gulp-uncss#gulp-uncss) 已不再維護，換成 [postCSS 版本](https://github.com/uncss/uncss)。

瘦身成果：

- ☹️ before： 303 KB 
- 😘 after： 82 KB

（ 都已壓縮過，且沒有 source map ）

這套工具的原理很簡單，利用 [jsdom](https://github.com/jsdom/jsdom) 把網頁上的 html DOM 元素爬過一遍，看看 CSS 有沒有沒用到的 classname，工具幫你把 CSS 裡面看似用不到的 class name 砍掉，為什麼會說是「 看似 」呢？！

因為有個顯而易見的問題，JavaScript 動態加上去的怎麼辦呢？！像是就是 click 後才會改變樣式的 class，這些都不在原本的 html，[uncss 的文件](https://github.com/uncss/uncss#options) 寫得很清楚，可以透過兩種方式避免清除掉這些動態加入的 class：

- 在 configuration 加上 ignore list

```javascript
{
    ignore: ['.fade']
}
```

- 在 CSS 裡面加上 ignore 單行註解

```css
/* uncss:ignore */
.selector1 {
    /* this rule will be ignored */
}

.selector2 {
    /* this will NOT be ignored */
}
```

不過現在通常不太會直接寫 pure CSS，**對於 SCSS 記得要用區塊註解**，才可以註解整個巢狀區塊 (`start` & `end`)

- 在 SCSS 裡面加上 ignore 區塊註解

```scss
/* uncss:ignore start */
.selector {
  &__inner {
    /* both selectors will be ignored */
  }
}
/* uncss:ignore end */
```

---

### 💡 Data Visualation

- plugin: [Chart.js](https://www.chartjs.org/)

用 Chart.js 的理由就只有一個：看起來夠簡單 😅

事實上也真是夠簡單的，打開 [sample](https://www.chartjs.org/samples/latest/) 選一個符合需求的，把資料改掉，就差不多了，學習痛苦指數 0。

真要說缺點就是覺得可控制的參數不夠多，沒辦法完全照著設計稿做，不過如果提供太多也就不夠簡單直覺，所以整體來，如果需求是個簡單不複雜的圖表，chart.js 真心讚。

![-w771](https://i.imgur.com/Xr9mrJa.jpg){:width="600"}


---

### 💡 Parallax scroll

- plugin: [rellax](https://dixonandmoe.com/rellax/)

有關視差滾動的套件非常多，但我也沒有要一個很酷炫的網頁，只希望不要那麼無聊跟死板，所以用了看起來很輕量且是 VanillaJS 的 [rellax](https://dixonandmoe.com/rellax/)。

使用上也很簡單，new 出視差元件 `new Rellax('.rellax')`，參數都是透過 `data-*` 屬性調整。不過參數要調好（ 移動速率... 等等 ），不然元件亂跑的機率很高。使用心得是 rellax 不太適合用在重要的內容區塊，像範例那樣**應用在一些小裝飾是比較明智的作法**。

---

### 💡 optimize webfont

值得一提的是，網頁用到許多來自 font awesome 的 icon font，打包出來的字型檔真的有夠肥，Huli 大大出手幫忙，另外寫個腳本，目的在於取出網頁上有用到的 icon 字型的 `minfont.js`。

> 半自動把所有用到 font-awesome 的地方找出來然後放到 minfont.js，再利用程式去找出 unicode，就可以用 fontmin 去優化，自動刪掉不需要的字元，以後有用到新的記得放到 minfont.js，不然會出現方框

瘦身成果：`140 kb` => `5 kb`，讚啦！

---

### 💡 gulp

以上大部分工具都要靠 gulp 建置，對於跑自動化任務，gulp 真的很厲害，易學好懂也方便處理各種靜態資源，更重要的另一個原因是我懶得學 webpack 😗（ 不過最近終於有動力研究 webpack，所以或許可以把這專案改用 webpack 當作練習 ）

以下是這次 gulp 的執行任務：

- clean target folder
    -  清空 `docs/`（ 因為網站放在 GitHub Pages ）
- compile sources
    - JS: `babel`, `browserify`
    - HTML: `pug`
    - CSS: `SCSS`, `postCSS`
    - Image: `svg-sprite`
- compress ( HTML, JS, CSS, Image )
- source map ( JS, CSS )
- copy asset sources
- optimize webfont
- local server with live reload

值得注意的是 gulp 4 的寫法比較不一樣，可以指定任務的執行順序：

- 同時執行： `gulp.parallel(js, css, html, image, font)`
    - 適合不會互相影響的任務
- 照順序執行：`gulp.series(clean, bundle, build)`


---

### 感恩的心

感謝前期規劃的 [Huli](https://medium.com/@hulitw) & [Min](https://medium.com/@minw)、共同開發的 [ChihYang41](https://github.com/ChihYang41)（ 辛苦了！ ），還有各種隱藏 QA 提出的建議，參加第三期課程的收穫真的不只是學習網頁開發，還有認識這些朋友們！

🥳 最後最後，成果在這 => [程式導師實驗計畫第四期](https://bootcamp.lidemy.com/)

當然我知道網站還有很多能夠改進的地方，也因為時間有點趕、寫得亂無章法，參考價值不高，但如果你有興趣的話，[原始碼在這](https://github.com/Lidemy/mtr-4th-web)，感謝你的耐心閱讀。