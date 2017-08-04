---
layout: post
title: "【學習】CSS3 漸層 background pattern"
description: "看到網路上很多很酷的CSS3漸層範例，應用的層面很廣，可以作圓點、斜線、格紋...各種花樣"
<<<<<<< HEAD
image: demo_css3_background.jpg
=======
image: http://i.imgur.com/sVs4ZDL.jpg
>>>>>>> gh-pages
category: CSS3
---

網路上很多很酷的CSS3漸層範例，

- [JSFiddle](https://jsfiddle.net/JohnnyWorker/r30oqptu/?utm_source=website&utm_medium=embed&utm_campaign=r30oqptu){:target="_blank"}
- [CSS3 Patterns Gallery](http://lea.verou.me/css3patterns/){:target="_blank"}
- [Codepen_Comic Texture](http://codepen.io/frank890417/pen/jrAKbO?editors=0100){:target="_blank"}


應用的層面很廣，可以作圓點、斜線、格紋...各種花樣，把比較實用的類型挑出來當範例 。

→ [我的 CSS3 漸層 DEMO](http://output.jsbin.com/hogazo){:target="_blank"}  

---

### 基本-圓點

``` css
background: radial-gradient(#ff8da1 30%, pink 34%);
background-size: 30px 30px;
```

> 背景色要比圓點多1~5%，視background-size而定  
※ background-size越小，%數差距要越多  
→ 差距太小：邊緣會有鋸齒  
→ 差距太大：邊緣會模糊


---


### 基本-線條

``` css
background: linear-gradient(90deg, lightblue 50%, #dceef4 0%);
background-size: 30px 30px;
```

> 橫條紋：0deg  
 斜線並不是45deg，要用其他方法

---


### 格紋

``` css
background-color: #86c5da;
background-image: linear-gradient(90deg, rgba(255, 255, 255, 0.3) 50%, transparent 50%), linear-gradient(rgba(255, 255, 255, 0.3) 50%, transparent 50%);
background-size: 25px 25px;
```

---

其實可以變花很多花樣，但要調整可能要多花心思才不會跑版，可以打開上面的demo來修改初自己想要的樣式。
