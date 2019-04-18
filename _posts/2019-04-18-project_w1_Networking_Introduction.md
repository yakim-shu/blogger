---
title: "[第一週] 搞懂目錄位置 & 網路基礎概論"
layout: post
description: "另外查資料時，發現一些小方法，如果有時候目標資料夾埋在很裡頭，或是名稱很長、又是中文之類的，打一大串也太累了吧，可以試試看以下兩種加速的方法。"
category: project
image: 
tags: [CLI, project]
file_name: 2019-04-18-project_w1_Networking_Introduction
---

## 1-2 練習

> 給你一個亂數的數列，例如說：1, 8, 9, 2, 5 ,4，你能想出什麼步驟把這些數字由小到大排好嗎？ 請用本堂課程教的寫法把解法寫出來。或者用自己的話講也可以

---

其實用這樣條列說明還蠻不簡單的，覺得比直接寫程式還難，一不小心就會腦袋打結 Q____Q，但我也同意想法才是重點，再加油吧！

（ 以下不確定正確 ）

```
1. 把數列的長度設為 length、一個變數 temp、設一個空的新數列 array
2. 設變數 i = 1、temp = 1、lowest = 第 i 個數字
3. 當 lowest > 第 i+1 個數字 
    - lowest 設為 第 i+1 個數字 
    - temp = i+1
    - i + 1
4. 當 i == length 進行以下步驟，否則，回到步驟 (3.)
    - 把 lowest 提出來，放到 array 的後面
    - 把第 temp 的數字從數列中刪掉
    - length - 1
5. 當 length == 1 進行以下步驟，否則，回到步驟 (2.)
    - 把第 1 個數字提出，放到 array 的後面
    - 回傳 array 2
    - 結束程式
```

---

## 2-2：基本指令練習，用文字來操作檔案吧！

講到 `~` 跟 `/` 位置的時候，感到有點抽象，試著寫下自己能夠加深理解的步驟。

### `cd ~` ： 回到 home 目錄 

- 用 `pwd` 查看，我電腦裡的 home 目錄位置是 `/Users/yakim`

### `cd /` ： 回到根目錄

- 用 `pwd` 查看，我電腦裡的 home 目錄位置是 `/`

---

### 再複習一次 相對位置 & 絕對位置

假設我現在位置在 `/` 根目錄底下，想要切換到桌面 `Desktop` (在 home 目錄下一層)，有兩種方式：

1. 相對位置：
    - 要知道**當前資料夾**與目標位置的**關係**
    - 輸入 `cd Users/yakim/Desktop`
2. 絕對位置： 
    - **無論在哪一層資料夾**都可以直接跳到目標位置
    - 輸入 `cd ~/Desktop`
    - 輸入 `cd /Users/yakim/Desktop`
    
意思是如果不是以 `~` 或 `/` 開頭，代表是輸入相對位置。 

### 自己很常用、要記下來

- **回桌面**： 不管我在哪個目錄下，都可以用 `~/Desktop` 回到桌面。 

---

## 快速進入資料夾

另外查資料時，發現一些小方法，如果有時候目標資料夾埋在很裡頭，或是名稱很長、又是中文之類的，打一大串也太累了吧，可以試試看以下兩種加速的方法。

### 方法一 

- 參考 [[MAC] 小技巧 – 快速將終端機開啟在指定路徑下](https://www.goston.net/2013/11/19/4381/)，教學寫得非常清楚
- 但此教學是開預設的 Terminal，如果要開 iTerm2 的話記得要打勾，也可以直接設好快捷鍵更方便

![螢幕快照 2019-04-18 下午12.45.31](https://i.imgur.com/FtgcHN2.jpg){:width="600"}

![螢幕快照 2019-04-18 下午12.42.47](https://i.imgur.com/tIYcQv8.jpg){:width="600"}

( 設好快捷鍵更方便！ )

### 方法二

這更簡單了，什麼都不用設定。直接按著資料夾、用滑鼠拉進終端機。

![螢幕快照 2019-04-18 下午12.49.24](https://i.imgur.com/rND8QFi.jpg){:width="700"}
![螢幕快照 2019-04-18 下午12.53.05](https://i.imgur.com/PfWkVxT.jpg){:width="700"}

就到達目的地啦！

參考資料：[Using Terminal in Mac OS](https://kaochenlong.com/2011/01/06/using-terminal-in-mac-os/)

## 補充 － 實用小技巧

（ 在後面的教學影片看到的，覺得好實用、趕緊記下來 ）

### `open .`： 打開所在位置

相反的，如果終端機裡處在很深處的資料夾位置，例如：` ~/Desktop/yakim/很冗長的資料夾，沒人想輸入/`，想用 finder 圖形化介面進入該資料夾該怎麼辦，滑鼠一直點進去也是很麻煩，所以可以用 `open .` 快速打開當前資料夾！

---

## 2-3：更多常用 command line 指令

- `date` ： 查看當下日期
- `top` ： 看當下電腦運行的狀態 (Table Of Processes)
- `less` ： 用分頁的模式顯示檔案內容，比起 `cat` 比較不會擾亂 terminal 介面
- `echo` ： 輸出字串到檔案或終端機上

---

## 4-1：為什麼我連不上這個網頁－－網路基礎概論

> 「當你在網址列輸入：google.com，這期間發生了什麼事？」

要回答這個問題前，必須先來點名詞解析。

### 網址

顧名思義就是網路上的位置，你要到對的位置、才可以得到對的內容麻。

### IP

每一個主機都有個 IP 位置，也等於是網路上的地址，更具體一點可以想像成門牌號碼，由四個 0 ~ 255 的數字組成。
例如：

### 域名 （ Domain ）

就是我們常用的網址，比起 IP 位置更好讀且好記，例如 google.com 就是一個域名。

可以把域名想像成 **景點名稱**，像「恆春圖書館」是一個域名，而「屏東縣恆春鎮天文路3號」則是 IP 位置。


### DNS ( Domain Name System )

DNS 能夠查詢域名，並指向正確的 IP 位置。

有點像 **Google 地圖** 的其中一項功能，輸入 "恆春圖書館" 就可以**得到恆春圖書館的地址**。

### 小小總結一下這些名詞的關係：

- 如果你想到達 **恆春圖書館** 這個地方（ 域名 ）
- 但因為不知道地址在哪，所以請求 **Google 地圖** 幫忙（ DNS ）
- 大神回覆正確地址： **屏東縣恆春鎮天文路3號** ( IP )。

---

## 前端 VS 後端

前端：就是打開網頁，一切你看得到的畫面，都算是前端的範疇。 
後端：在背後處理資料的流程，都是使用者看不到的。

1. 瀏覽器會向 Server 發出一個 request（ 前 ）
2. Server 再向 資料庫 存取資料（ 後 ）
3. 資料庫再挑出資料丟回 Server（ 後 ）
4. Server 發出一個 response 回覆瀏覽器（ 前 ）
5. 瀏覽器進行解析資料、並轉成畫面顯示到網頁上（ 前 ）

---

有了基本概念就可以來回答 「當你在網址列輸入：google.com，這期間發生了什麼事？」

以下直接截圖課程影片內容。（ 寫得太清楚！！ ）

![螢幕快照 2019-04-18 下午2.21.18](https://i.imgur.com/3Zj9XOq.jpg){:width="500"}


---

## 4-2：我的 IP 怎麼跟別人的一樣－－內網與外網

這章節真的把腦袋充滿問號的名詞、講解得好清楚，覺得很厲害！

### 內網 VS 外網

家裡應該有個 IP 分享器，目的是讓家裡的不同電腦都可以同時上網，此時家裡的電腦就形成一個**內網**，每台主機都會被各自分配到一個**虛擬 IP**。

而同一內網底下的所有電腦，雖然虛擬 IP 都各自不同，但連到外網的時候，對**外網而言都是同一個 IP**。

最常見的情況應該是在公司，可能公司所有電腦都是在同一個內網底下，而**有些公司的內部系統或服務，有限制只能在內網底下存取**，所以當你回到家，就會發現上不了公司的系統，這也是出於安全性的考量，也是防堵一些非公司人員進入的措施。

![螢幕快照 2019-04-18 下午2.42.44](https://i.imgur.com/IyBJfuf.jpg){:width="500"}

（課程影片截圖）


> 待查詢問題：網路上的芳鄰？

### VPN ( Virtual Private Network )

以剛剛的內容來舉例，假設一個狀況劇。

> 你人在海島度假，萬惡的主管突然打來說有急事要處理、但公司的管理系統只能公司內部登入啊，救命啊！

VPN 一個箭步登場，就算我們人處在休閒的海島，也可以透過 VPN 先連到 **公司內部**，就可以連上公司的系統拉。

中國俗稱的「 翻牆 」，也是透過同樣的機制。

當他們想連上 Facebook 這網站，但國內的 IP 會被鎖，所以會利用 VPN 先連到其他國家的伺服器、例如台灣，假裝自己來是自台灣的電腦，就可以順利連上 Facebook 了。
