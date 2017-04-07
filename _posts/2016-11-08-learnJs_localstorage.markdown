---
layout: post
title: "【學習】LocalStorage 本地端備忘錄"
description: "最近在練習 Localstorage，寫了一個類似 keep ( 陽春非常多 ) 的本地端 Todolist
可以輸入幾個訊息再重新整里，會發現訊息還在，資料是記在瀏覽器上，不同瀏覽器沒有共享喔！"
category: Html5
image: http://i.imgur.com/jvrbSPq.jpg
---

最近在練習 Localstorage，寫了一個類似 keep ( 陽春非常多 ) 的本地端 Todolist  

功能就只有：

* 輸入 & 移除訊息
* 加入 & 移除便條
*  設置便條顏色


可以試著輸入幾個訊息再重新整理，會發現訊息還在  ，資料是記在瀏覽器上，不同瀏覽器不能共享喔！  

因為localstorage **沒有過期時間** ( 不像cookie )，我也沒有特別再寫  
所以沒有手動清除的話，資料就會一直存在唷～


``` javascript
// 設置localsotrage ，命名為 listStorage
localStorage.setItem("listStorage", JSON.stringify(arrStorage));
// 存取 listStorage
var oList = JSON.parse(localStorage.getItem('listStorage'));
```

但其實本範例其實應用到的只有 ``getItem`` & ``setItem`` 而已阿阿阿，大部分的時間都花在一些無關緊要的事上。

假學習、真自high

→ [Todolist 範例網址](http://output.jsbin.com/fofidep/1){:target="_blank"}  
