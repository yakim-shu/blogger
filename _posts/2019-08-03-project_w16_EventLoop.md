---
title: "[第十六週] JavaScript 進階：事件迴圈 Event Loop、Stack、Queue"
layout: post_project
description: ""
category: project
tags: [Event Loop, Call Stack, Queue, Stack, project_week16]
file_name: 2019-08-03-project_w16_EventLoop
---

本篇是要介紹 Javascript Event Loop 的原理，透過了解 Event Loop 機制，才能夠了解 JavaScript 中同步與非同步程式的執行順序。

而在那之前，先補充 Stack & Queue 兩種資料結構的背景知識，有這樣的前提下才能了解什麼是 Event Loop。


#### 什麼是資料結構？

資料結構可以理解成某種形式的資料，不同資料結構會有不同類型的特性。

比如說常用的陣列 Array，會有 length 長度等等的性質。又比如說物件 Object 也是一種資料結構，會有 key & value 相對應的特性。

* 陣列 Array（數組）
* 鏈結串列 Linked List（鏈表）
* 堆疊 Stack（棧）
* 佇列 Queue（隊列）
* 樹 Tree


### 堆疊 Stack 

> 大原則是： "Last in, First out" ( LIFO ) => 第一個進去、會最後一個出來

東西往上放、要拿取也是從上面拿

```javascript
stack.push(1)
stack.push(30)
stack.push(6)
stack.pop() // 6
stack.pop() // 30
stack.push(9)
```


### 佇列 Queue

> 大原則是： "First In, First Out" ( FIFO ) => 第一個進去、第一個出來

想像成排隊第一個的，一定會最先進去

```javascript
queue.push(1)
queue.push(30)
queue.push(9)
queue.shift() //1
queue.shift() //30
```


---
## 此篇的重點： 事件迴圈 Event Loop

而以上介紹 Stack & Queue 的資料特性，就是為了要介紹事件迴圈 Event Loop。

瀏覽器在跑 JavaScript 是 single thread （單執行緒），一次只能執行一個任務，所以要有一個機制來跑非同步的東西，而機制的其中一個環節就叫 Event Loop，等於是**決定執行任務順序的主宰者**。

可以想像成 JavaScript 只能跑 single thread，但瀏覽器可以跑 multiple thread，所以會利用 Event Loop 機制去幫助 JavaScript 執行任務。

#### ☞ 小補充： Process 程序 vs thread 執行緒

- Process: 跟作業系統有關，一個 Process 底下可以有很多不同的 thread
- Thread: 執行工作的單位

像是 google chrome，一個分頁就是一個 process，所以分頁當機並不會影響到其他分頁


### Call Stack ( 監測的對象一 )

所有的程式語言都有 Call Stack 概念。

當有 `a` function call `b` function， 此時 call stack 就會把 `a` 先丟近來、再把 `b` 丟在 `a` 上方。


#### Stack overflow 

那如果不停堆疊 Call Stack，在有限的空間裡遲早會爆的，所以當你寫了一個無權迴圈（ function `a` 不停地 call `a` 本身 ）

Call Stack 就會不斷得被填滿、出現錯誤，很可能會拋出 `Maximum call stack size exceeded`，也就是鼎鼎大名的 Stack Overflow。

### Callback Queue ( 監測的對象二 )
而 Event Queue 則是當有非同步操作時，例如 setTimeout ，瀏覽器會先丟一個 timer 到 web API ，等到時間到了，就會把 timer 的 Callback function 丟進 Callback Queue 裡面，讓 Event Loop 去監測。


## Event Loop 運作模式 

**「 不斷得去監測 Call Stack & Callback Queue 」**

Event Loop 的監測順序：
1. 看 Call Stack
2. 看 Callback Queue
    - 如果 Call Stack 為空，且 Callback Queue 有東西
    - 將 Callback Queue 的東西移到 Call Stack
3. 回到步驟 (1.)
    
> 第一優先順序永遠都是 Call Stack，所以有一個問題是，如果 Call Stack 一直有任務的話，那 Callback Queue 就不會被執行到。


意思是假如 Ajax 的 response 早就跑回來了，還是要等 Call Stack 任務清空才會被丟進去 Call Stack 執行。

所以當你發生 Call Stack Blocking (卡住) 的時候，所有在瀏覽器的操作都會失效（例如點擊按鈕），但其實不是失效，只是在等 Call Stack 的任務清空，所以就算你很著急的點了五次按鈕，其實 Queue 還是排進了五次的點擊事件，只是畫面上看起來毫無動靜。

##### 補充影片：
透過這影片用動畫去理解 Event Loop 是怎麼運行的：[What the heck is the event loop anyway? | Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ)，非常清楚明暸，推薦！


---

### 如何觀察 Call Stack

其實在寫 JavaScript 的時候，我們可以從 console 的錯誤訊息觀察 Call Stack 的順序，以幫助開發者了解出錯的源頭在哪裡。

假如有個程式是 `a()` call `b()` call `c()`，而 `function c` 裡面有個錯誤：

```javascript
function a() {
  return b();
}
function b() {
  return c();
}
function c() {
  console.log(abc);
}
a();
```



Call Stack 應該會往上堆疊呼叫的 function，所以順序會是這樣，又最下面的主程式 `main()` 開始往上堆疊：

- console.log(abc)
- function: c
- function: b
- function: a
- main()

而下列這張圖就是主控台拋出的錯誤訊息，可以看得出來錯誤訊息的 Call Stack 順序跟上面排列的預期順序是相同的。

![螢幕快照 2019-08-06 下午11.02.43](https://i.imgur.com/1dCoRkc.jpg)




