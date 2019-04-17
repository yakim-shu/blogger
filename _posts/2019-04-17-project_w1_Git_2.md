---
title: "[第一週] 版本控制 - 進階指令 & GitHub"
layout: post
description: "根據上一節學到 GIT 基本版本控制，都是處於線性的開發流程，每次 commit 的「新版本都是基於上一個版本的修改」，這樣單人作業的時候可能不會有太大問題，但當多人協作、或同時要開發不同功能時，可能就會有衝突產生。"
category: project
image: 
tags: [Git, GitHub, project]
file_name: 2019-04-17-project_w1_Git_2
---

根據上一節學到 GIT 基本版本控制，都是處於線性的開發流程，每次 commit 的**新版本都是基於上一個版本的修改**，這樣單人作業的時候可能不會有太大問題，但當多人協作、或同時要開發不同功能時，可能就會有衝突產生。

---

## 沒有 branch 會怎麼樣

來看 Huli 舉的案例：
1. 公司上線的產品： `穩定版`
2. 近期在開發一個新功能，但只開發到 50% ： 
    - `穩定版` → `穩定版 + 開發新功能(50%)`
1. 客戶突然傳來有個嚴重的 bug 要修： 
    - `穩定版 + 開發新功能(50%)` → `穩定版 + 開發新功能(50%) + 修 bug`

因為要修 bug 很急迫，把 (3.) 的版本拉上線，但因為參雜了未完成的新功能，想必是會有更多問題。

## 有了 branch 會怎麼樣

為了解決以上問題，branch 的作用是讓 **開發過程各自獨立**、簡單一點。`開發新功能` 獨立出來變成支線、 `修 bug` 也是一條支線。

當客戶催著工程師快點修好上傳，就只需要上傳 `穩定版 + 修好 bug` → `新的穩定版`，至於 `開發新功能` 這條線想像成離家出走了，開發完就可以回家**合併**成 `更新的穩定版`

有了好用的 branch，修改上個案例：

1. 公司上線的產品： `穩定版`
    - 有三條支線：
        - `branch_main_穩定版` 主要支線
        - `branch_1_開發新功能` 獨立成一條支線
        - `branch_2_修bug` 獨立成一條支線
        - ![螢幕快照 2019-04-17 下午12.16.55](https://i.imgur.com/3eSPSSY.jpg){:width="600"}
2. 當客戶催著工程師快點修好上傳： 
    - 由 `branch_main_穩定版` + `branch_2_修bug` → `branch_main_穩定版(v2)`
    - ![螢幕快照 2019-04-17 下午12.21.00](https://i.imgur.com/ZQLlv0a.jpg){:width="600"}

3. 新功能可以慢慢開發、等**完成了再合併**： 
    - 由 `branch_main_穩定版(v2)` + `branch_1_開發新功能（100%）` → `branch_main_穩定版(v3)`
    - ![螢幕快照 2019-04-17 下午12.25.26](https://i.imgur.com/k8ZpCU7.jpg){:width="600"}

----

## 正式跟 branch 做朋友

有了對 branch 的基本概念，來開始打有趣的指令吧！（有趣嗎？）

### 操作 `branch` 

如果還沒有新建 branch，系統預設的 branch 就叫做 **`master`**。

---

#### `branch -v` ： 查看當前 branch （ 看你在哪個平行時空 ）
    
- 查看當前在哪一個 branch 裡面，以剛剛的資料夾舉例，可以想像成**我現在處於哪個上層資料夾裡**。
- ![螢幕快照 2019-04-17 下午12.37.39](https://i.imgur.com/USjpOos.jpg){:width="600"}

（ 表示我在 `master 支線`、`最新的 commit 碼(前 7 碼)`、`最新的 commit message` ）

---

> 自己覺得 `branch -v` 有點像把 **門牌號碼** 列出來

- 門牌號碼：
    - branch ： 最左邊的資料夾
    - commit ： 中間的資料夾
- 檔案 ： 則是位於此地址的檔案狀態

![螢幕快照 2019-04-17 下午1.59.09](https://i.imgur.com/vrN3BAK.jpg){:width="800"}

---

#### `branch <branch-name>` ： 新建一個 branch （ 新開一個平行時空 ）

- 新建 branch 很簡單，只要在後面加上 branch 名稱就可以了！
- ![螢幕快照 2019-04-17 下午1.24.31](https://i.imgur.com/NeRblus.jpg){:width="700"}

- 試著用 `branch -v` 查看有沒有新增成功
- ![螢幕快照 2019-04-17 下午12.48.51](https://i.imgur.com/e4geZZM.jpg){:width="600"}

（啊哈) ( `commit 號碼` 跟 `commit 訊息` 沒有變，因為中間我沒有新的 commit ）

---

#### `branch -d <branch-name>` ： 刪除 branch （ 關閉一個平行時空 ）

![螢幕快照 2019-04-17 下午1.12.40](https://i.imgur.com/yiFRs1b.jpg){:width="700"}

---

#### `git checkout <branch-name>` ： 切換 branch （ 跳耀時空的少女 ）

- 還記得上一節教過的切換 commit 版本嗎？ 切換不同 `branch` 也是一樣的道理。差別在於前者要輸入很難識別的 `hashes` ，後者輸入 `branch-name`，如以下：
    - 切換 commit 節點 : `git checkout <commit hashes>`
    - 切換 branch 分支 : `git checkout <branch-name>`
- ![螢幕快照 2019-04-17 下午1.45.21](https://i.imgur.com/XAFXp2t.jpg){:width="700"}
 - 接著同樣用 `git branch -v` 查看當前狀態
 - ![螢幕快照 2019-04-17 下午1.46.10](https://i.imgur.com/a2hK5gs.jpg){:width="700"}

（ `* 當前 branch` 被高亮顯示！ ）

> 可以把 `git checkout` 當成拜訪不同的 **門牌號碼**，也因此會得到不同版本的檔案

---

## 重頭戲： `merge` 合併分支

講了這麼多都在鋪陳 branch 的作用，但 `branch 支線` 最終還是要 merge 合併成 `master 主線`，所以在討論如何 merge 之前，先釐清一個觀念。

---
> 以下由我自己胡亂設定的故事理解，但不確定對不對

- 合併不是 `branch_A` + `branch_B` → `branch_C`
- 應該是以下兩種可能：
    - `branch_A` + `branch_B` → `branch_A`
    - `branch_B` + `branch_A` → `branch_B`
- **順序**很重要 ，應該會影響到合併後的分支 
- 想像成主從關係，老大跟小弟進行合併，總不會最後變小弟吧！
- **所以要先 `老大` 設成當前位置： `老大` + `小弟` → `老大`**

---

那加強老大惡勢力的步驟應該是：
- 先切換到老大支線： `git checkout master`
- 再把小弟合併進來： `git merge new-feature`
- 查看狀態，看小弟的 commit 資料是不是 **被合併到** 老大： `git log`
- 可憐的小弟如果沒有作用，進行捨棄： `git branch -d new-feature`
（ 社會上維持該有的秩序與倫理，很棒 ）

![螢幕快照 2019-04-17 下午2.54.17](https://i.imgur.com/otWZmwh.jpg){:width="700"}


以下是 log 內容，可以看到小弟的 commit 訊息有在**合併後的老大支線**

![螢幕快照 2019-04-17 下午2.55.56](https://i.imgur.com/z1wazR0.jpg){:width="700"}

---

## 重頭戲之二： conflict 發生衝突了啊啊啊啊啊啊啊

這是之前我個人的惡夢，每次遇到衝突就胡搞瞎搞，使用第三方套件的情況下，解決的辦法就是按一按衝突的位置，看能不能改掉再 commit 一次。

> 看完這堂課才有「啊～原來就這樣而已麻」，人果然很容易對未知的領域感到恐懼。

先了解衝突是怎麼發生的，以詞義來說，聽起來好像是個 **錯誤**，以前是這樣理解的，所以容易感到很不安。

不過其實就是「 **同一個檔案的兩份版本**，裡面有**一個**或**多個**不同的內容 」

**而電腦無法幫你選擇哪一個當作最終版本**，因為只有你自己知道想要留下的內容是什麼，所以當發生衝突時，只能自己**手動編輯**。 

----

首先為了解決衝突，先故意製造一個衝突
- 我在 `master` 跟 `new-feature` 這兩條支線上都改了同一個檔案 `new.txt`
- 然後當我要把 `new-feature` merge 到 `master` 支線上
- 果然跳出錯誤訊息。

![螢幕快照 2019-04-17 下午3.50.35](https://i.imgur.com/0dxvj6X.jpg){:width="700"}

現在我們要試著手動修改檔案、解決衝突，打開編輯器會看到衝突的位置被清楚標示，視不同編輯器會有不同高亮效果，但有衝突的位置應該都會用 `======` 分隔線隔開。
（ 此圖是用 VS Code 開的 ）

![螢幕快照 2019-04-17 下午3.44.09](https://i.imgur.com/sMNjVpx.jpg){:width="700"}

最後我希望的內容是兩邊都各留一點，再加上新的內容，然後記得刪除 `提示文字` 再存檔。

（ `提示文字`指的是：`=====`、`<<<<<<<< HEAD`、`>>>>>>>>>> new-feature` ）

![螢幕快照 2019-04-17 下午4.03.39](https://i.imgur.com/FudHzzT.jpg){:width="700"}

（ 其實這圖不是很重要，就只是看你檔案要保留什麼內容而已。 ）

#### 衝突修好了、開啤酒慶祝！

把有衝突的檔案改好了，只要再把修好的檔案再 `commit` 一次就成功啦！ya～

----

## Git VS GitHub

以上學了那麼多，應該知道 Git 就是一種版本控制的**程式**。

而 **GitHub** 可以想像成「 提供存放使用 Git 專案倉庫（Repository) 的**服務** 」，你也可以不要用 GitHub、自己架一個 Git Sever。

意思是就算我們的專案用 Git 做版本控制，也不一定要上傳到 GitHub 作為託管；但如果專案放在 GitHub 上供多人協作開發，代表此專案一定有使用 Git 做版控。

### GitHub

世界上最大的程式碼存放網站和開源社群。

當我們把專案傳到 GitHub 上，因為是 GUI 介面，就可以很清楚的看專案的 Commit 紀錄、檔案修改的歷史過程、修改者是誰...等等的資訊，都可以在倉庫 repository 的頁面找到。 

### Pull request

想像成在 GitHub 上執行 merge 的動作，就是 pull request 。

因為在本地端 merge 的時候，其實不好查看檔案的差異，所以大家習慣是在 GitHub pull request 進行合併。

### Issue

專案的討論區，有問題可以在上面發問及回答。

---

#### 曾經碰到的問題 - 上傳專案到 GitHub

![螢幕快照 2019-04-17 下午6.03.12](https://i.imgur.com/98Xt14Q.jpg){:width="600"}

這裡遇到問題了，當我要上傳本地專案到 GitHub 上，按照網頁的指示，應該輸入這兩行指令就行了。

但當我輸入 `git push -u origin master` ，會出現錯誤訊息： 

![螢幕快照 2019-04-17 下午5.47.37](https://i.imgur.com/1L6eW38.jpg){:width="700"}

照理來說這應該是沒有權限才會出現的問題，但明明我的帳號啊？
查了一下好像是沒有建立 SSH 公鑰（到底公鑰還私鑰，永遠搞不懂ＱＱ）

一開始有點惶恐，因為 [官網文件](https://help.github.com/en/articles/error-permission-denied-publickey) 列出來的可能因素很多、看了頭好痛，最後找到這個 [知乎回答](https://www.zhihu.com/question/21402411)，按照步驟生成 public key，再回到 GitHub 網站上的設定、新增 SSH ，就解決啦！太好了～

----

## 與 GitHub 相關的操作指令

### `push`： 推上 repository
- 同步到 GitHub 上面
- `git push original <branh-name>`

### `pull`： 從 repository 拉下來
- 從 GitHub 同步回到本地
- `git pull original <branh-name>`

### `clone`： 從 repository 複製一份
- 從 GitHub 下載一份到本地
- `git clone <SSH>`
- 沒有權限修改別人的 repository，不能 push 回去

### `fork` ： 從 repository 複製一份成為自己的 repository
- 從 GitHub 複製一份成為自己的 repository
- 有權限修改自己的 repository，可以 push 回去

---
 
## GitHub flow

GitHub 建議管理專案的流程： [官網說明](https://guides.github.com/introduction/flow/)
    
所以如果你在 GitHub 看到有興趣的專案，想要參與貢獻：

- 先 `fork` 到自己的 repository
- 新增一條 branch 開發
- 每到一個段落記得 `commit`
- 開發完再 `push` 回自己的 repository
- `pull request` 到原本專案的 repository 請求合併
- 在上面做討論，等待對方的 code review
- 合併成功，刪掉 branch
    
---

## 狀況劇

> Q: 可以改 `commit message` 嗎？

A: 使出 `git commit --amend`

但如果已經 push ，就盡量不要改，所以保持好習慣，push前都先檢查沒錯再推上去。

> Q: 哎呀！還有個東西沒改到，可以放棄剛剛的 commit 嗎？

A: 使出 `git reset HEAD^`

`HEAD` 其實就是最新的 commit ，而 `^` 是指上一個，所以 `git reset HEAD^` 指的是回到上一次 commit。也可以直接加 commit hashes 碼：`git reset <commit hashes>`

> Q: 哎呀！把東西搞壞了，不過我還沒 commit，可以回復到上次 commit 的狀態嗎？

A: 使出 `git checkout -- <file>`

上例是改單個檔案的做法，如果想把整個專案的所有檔案都復原：`git checkout -- .`

> Q: 可以改 branch 名稱嗎？

A: 先切換到要改名的 branch 下，使出 `git branch -m <new-name>`

> Q: 要怎麼拉下遠端的 branch？

A: 超級簡單又神奇，在本地端使用 `git checkout <branch-name>`，本來不存在的 branch 就會從遠端拉下來了

---

## Git Hooks

蠻有趣的功能，意思是說你可以自訂腳本、客製化「 在 **某些事件發生** 時會 **觸發**你寫好的腳本 shell 」

舉例來說，像是你可以規定，再提交 commit 之前，先檢查**程式碼有沒有符合團隊規範**、或是有沒動到**一些不能動的檔案**等等。

參考資料： [Git 客製化 - Git Hooks](https://git-scm.com/book/zh-tw/v1/Git-客製化-Git-Hooks)



 