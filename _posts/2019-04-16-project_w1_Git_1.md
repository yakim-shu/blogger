---
title: "[第一週] 版本控制 - 原理 ＆ 基本 Git 指令"
layout: post
description: "版本控制簡單來說，就是把一個檔案所有歷史紀錄的版本都保存起來。Git 的好處之一，能幫你管理好檔案的所有版本，第二是在開發新功能時，能夠同時保留不同功能版本，以利於之後的合併，例如：主要的穩定版、開發新功能版、修 bug 版..."
category: project
image: 
tags: [Git, project_week1]
file_name: 2019-04-16-project_w1_Git_1
---

## 進入主題之前，先來聊聊版本控制的原理

版本控制簡單來說，就是把一個檔案所有歷史紀錄的版本都保存起來，像是做設計的同學一定有很深的體會。

- poster.ai
- poster_2.ai
- poster_final.ai
- poster_final_final.ai
- poster_客戶去死.ai
- poster_我要離職.ai

最後同事都被弄到氣走了，這樣下去不是辦法，所以需要一個更有系統、且可以輕易比對各版本差異的模式來管理檔案。

以上是 Git 的好處之一，能幫你**管理好檔案的所有版本**。

第二是在開發新功能時，能夠**同時保留不同功能版本，以利於之後的合併**，例如：**主要的穩定版** 、 **開發新功能版** 、 **修 bug 版** ...


## 簡易版控

假如一個網頁資料夾，裡面有不同檔案互相有關聯的 `index.html`, `main.css`, `index.js`，當你只改了其中一個檔案，那要怎麼做版本控制呢？

#### 一個人的版控

1. 複製單個檔案出來？
    - 但最後會越來越複雜，而且不好對應各個檔案。
2. **還是網頁資料夾都複製出來？**
    - 好像是個不錯的方法，來實做看看。 
        
假設昨天修改了 `index.html`，那就把原本的 `web 資料夾` 複製一份出來成 `web_v2 資料夾`，這樣一個人做板控其實沒有什麼問題，但要是多人協作的情況呢？

#### 多人協作發生衝突，小明不要鬧了

假設現在跟同事在雲端的空間上都是同步著最新的 `web_v2 資料夾`。

小花需要修改檔案，所以捉一份下來要改成 `web_v3 資料夾`，但旁邊的同事小明剛好也抓了一份下來修改成 `web_v3 資料夾`，**造就了現在有兩份 `web_v3 資料夾`，名稱一樣但內容不同的衝突情形。**

所以為了解決這狀況，那就試著把資料夾名稱 `web_v3`，**轉成 hashes 亂碼** ，假設 hashes 是完全不會重複的，所以小花這份的資料夾名稱改成 `eyokend3d28sf`，而小明的是 `akuebh83ks02d`。

![螢幕快照 2019-04-16 上午11.46.12](https://i.imgur.com/WNDyORt.jpg){:width="600"}

( 發現 `main.css` 打成 `man.css` 了，拜託忽略這個丟臉錯誤 )
( 提醒別人忽略是一件很矛盾的事 )


#### 如何識別順序、最新版本，永保辦公室和諧

現在解決了版本號碼衝突的問題，但亂碼無法看出順序關係，該怎麼辦呢？  

不如新增一個 `版本紀錄.txt`，裡面記錄好上傳時間、上傳者、版本號碼：
- `4/15 08:00`, `小花`, `eyokend3d28sf`
- `4/15 08:05`, `小明`, `akuebh83ks02d`

檔案隨著時間越來越大，不好看出最新的版本怎麼辦，再新增一個 `最新版本.txt`：
- new: `akuebh83ks02d`

#### 不用做版本控制的檔案

有些檔案確定不會變動，像是**電腦設定檔、 log 檔、或者是你覺得沒有必要做版控的檔案**，例如剛剛管理版本號碼的文件，那要怎麼做勒？很簡單，移出資料夾就就好了。所以我們最後的版本控制目錄會變成：

- `最新版本.txt`
- `版本紀錄.txt`
- 有用版本控制的檔案
    - `eyokend3d28sf/`
        - `index.html, main.css, index.js`
    - `akuebh83ks02d/`
        - `index.html, main.css, index.js`

![螢幕快照 2019-04-16 下午12.18.48](https://i.imgur.com/9rA1x2C.jpg){:width="600"}

( 這兩個檔案只是拿來舉例 )    
    
#### 最陽春的版本控制系統就完成啦

沒錯，這樣就是版本控制的雛形拉，沒想到原理沒有很複雜啊，那為什麼我之前看了一堆教程，對 Git 還是一知半解呢？（ 回望過去 ）


---

## 正式進入 Git 旅程

對版本控制有大致上的了解後，就可以開始正式使用囉。

### `init` 版本控制初始化

在桌面先開一個測試資料夾 `git_test`， 打開 Terminal、將當前目錄改到 `git_test`，輸入 `git init`，會發現多了一個 `.git` 的隱藏資料夾。

小技巧：
>  Mac 預設是看不到隱藏檔案的。想要開啟可以參考：[三招讓 Mac 顯示出隱藏檔案](https://www.macuknow.com/node/76787)

![螢幕快照 2019-04-16 上午10.58.46](https://i.imgur.com/J1vhBQF.jpg){:width="600"}

### `status` 查看狀態

顧名思義就是查看狀態，非常常用的指令，有事沒事就查看一下，保佑全家健康、版本正常。

![螢幕快照 2019-04-16 上午11.04.55](https://i.imgur.com/S0QJtUd.jpg){:width="600"}

( 按 `Q` 跳出 )


### `add` 被選上的孩子們

把檔案移到 `git_test/` 裡面，用 `git status` 查看狀態會看到下圖：

![螢幕快照 2019-04-16 下午12.31.09](https://i.imgur.com/lNVNohT.jpg){:width="600"}

看到 **檔案內容用紅字標示** ，表示這些檔案雖然在此資料夾中，但並 **沒有加入版本控制** ，Git 提供很貼心的提示（黃框內），表示你可以用 `git add <檔案>` 加入版控。

那就按照提示用 `git add index.html` 讓 `index.html` 加入版控，再用 `git status` 查看狀態，會發現 **檔案被分成兩個區域，分別是 Stage (有版控) 、 Untracked （沒版控）**。  

![螢幕快照 2019-04-16 下午12.41.16](https://i.imgur.com/zNEwYU8.jpg){:width="600"}

可以看到粉紅線提示：
- 加入版控： `git add <檔案>`
- 取消版控： `git rm --cached <檔案>`

可是如果有超多檔案，可以使用 `git add .` ，把全部檔案加入版本控制。

## `commit` 新建版本 - 人生 on-line 不要忘了存檔

安排好了哪些檔案要加入或不加入版控了，接下來要開始新建人生的第一個版本，以**剛剛「簡易版控」的例子來說，就是新增一個資料夾。**

- 輸入 `git commit` 
    - 會進入 VIM 編輯你的版本訊息 commit message，如果你覺得很惱人也可以在 commit 的時候一次完成：`git commit -m <訊息>`

注意：

> 如果檔案有變動，想要再提交一次 commit 之前，還是需要再輸入 `git add <該檔案>` ，不然是無法 commit 的喔！

（ 慘痛教訓要記下來 ） 

## `log` 回顧 - 偉人都會有個時間軸

當然也可以查看其中的版本紀錄，就像「簡易版控」的 `版本紀錄.txt` 一樣，內容有：**版本號碼、提交者、提交時間。**

- 輸入 `git log` 查看
    - 可以看粉紅線底下，也是 **「簡易版控」的 hashes 版本號碼**
![螢幕快照 2019-04-16 下午1.58.07](https://i.imgur.com/I28vRhM.jpg){:width="600"}
    - 變化型
        - `git log --oneline`： 輸出更簡潔的 log ( 差點看成 on-line )
            - 只輸出 `hashes 前七碼` & `commit message`
        - ![螢幕快照 2019-04-16 下午2.19.51](https://i.imgur.com/Q8fZZyz.jpg){:width="600"}

        
## `checkout` 切換版本 - 回到人生的存檔點

人生無法隨意切換存檔點，Git 可以。

- 輸入 `git checkout <版本號碼>` ：切換到任何 commit 過的版本 
- 輸入 `git checkout master` ：切換到最新版本 

## `.gitignore` 忽略不要版本控制的檔案 - 派對中的邊緣人

還記得剛剛學到 `add` 指令時，檔案會分成兩區嗎？（ 版控＆非版控 ）

每次提交 `commit` 前都一定要 `add` 需要板控的檔案，但輸入 `git add .` 提交所有檔案，又會把不要的檔案 `add` 進來。

因此 **邊緣人列表 `.gitignore`** 閃亮登場，把**不需要版本控制的檔案記錄**在 `.gitignore` 裡，之後就可以放心使用  `git add .` ，省得擔心會加到不必要的檔案。

注意：
- 這是**一個檔案**，不是 Git 指令！
- **`.gitignore` 檔案本身也需要加入版本控制**

![螢幕快照 2019-04-16 下午2.54.47](https://i.imgur.com/sajNixN.jpg){:width="600"}

( 此例中的邊緣人就是 `history.txt` 跟 `new.txt`，一樣只是拿來舉例 )

想知道一般 `.gitignore` 會紀錄哪些檔案，可以參考 Huli 舉出 [Facebook 的開源專案 React 裡的 `.gitignore`](https://github.com/facebook/react/blob/master/.gitignore) 示範。（ 覺得很棒！不然憑空真的不好想像 ）

--- 

## 另外的小技巧

### `commit -am` 一次完成

有沒有人跟我想的一樣，人生的麻煩還不夠多嗎？每次都要 `add` 再 `commit` 也太累了吧～

所以救星出現了！

- `git commit -am "message"` ： 
    - 比起 `-m` ，多一個 `a` 就可以讓兩個動作一次完成。
    - 建議每次都輸入 `git commit -am "message"` 當成習慣，省得忘記 `add` 的麻煩。
    - **這裡要小心留意回報的訊息**，如果出現 "no changes added to commit"，代表可能有新增的檔案忘記 `add`

因為很遺憾的，救星沒有這麼給力，根據 [Git 官網文件](https://git-scm.com/docs/git-commit)：

Tell the command to automatically stage files that have been modified and deleted, **but new files you have not told Git about are not affected.**

意指 `-a` **不適用於新增的檔案**，所以如果有新檔案的加入，還是得先 `add` 再 `commit`。（深呼吸）

### `diff` 查看與上一版本不同之處

- `git diff` ： 能夠查看檔案與上一版本的內容差異

![螢幕快照 2019-04-16 下午5.01.07](https://i.imgur.com/FJDzjI1.jpg){:width="600"}

（ 按 `Q` 跳出 ）
（ Terminal 的顏色還沒改好、所以不是很明顯，通常以新增的內容會用綠色表示、減去用紅色表示。 ）


## 小小總結

複習以上學到的，要開始使用 Git 的步驟如下：

> 括號的內容：對照「簡易板控」的步驟

1. `git init` ： 初始化
2. 建立檔案 `.gitignore` ： 忽略不需要版本控制的檔案
3. `git add .` ： 把需要版本控制的檔案加進去（ 新增一個暫存資料夾 `temp/`，把東西丟進去 ）
4. `git commit -am "message"` ： 新建一個版本 （ 把 `temp/` 資料夾名稱改成版本號碼 `hashes/` ）
    - 如果步驟 (3.) 已經完成的話，也可以直接用 `git commit -m "message"`
    - 可以用 `git diff` 查看與上一版的差異。
5. `git checkout <hashes>` ： 可以切換各個版本 （ 切換各個 `hashes/` 資料夾）
    - `git checkout master` ： 回到最新版本






    
