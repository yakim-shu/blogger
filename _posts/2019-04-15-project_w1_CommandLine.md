---
title: "[第一週] Command Line - Terminal、基本指令介紹"
layout: post
description: "有別於一般人習慣的圖形化介面 (GUI)，其實可以實現一樣的動作，差別在於 GUI 是別人 設計規劃出容易理解的介面，像關閉檔案的按鈕大部分都是紅色 X 作為識別，通常是用滑鼠操作。而 Command Line 則是 透過純文字 來操縱指定的動作。 "
category: project
image: 
tags: [CLI, project, terminal]
file_name: 2019-04-15-project_w1_CommandLine
---


## Command Line 基本理解

簡單來說就是跟電腦溝通，做出我們想要執行的動作。

有別於一般人習慣的圖形化介面 (GUI)，其實可以實現一樣的動作，差別在於 GUI 是別人 **設計規劃出容易理解的介面**，像關閉檔案的按鈕大部分都是紅色 X 作為識別，通常是用滑鼠操作。

而 Command Line 則是 **透過純文字** 來操縱指定的動作。 

---

## 環境建置

推薦 Mac 用的 Command Line Tool - iTerm2

優點：
- 可以客製化
- 介面比較好看
- 可以開很多分頁

參考：
- [超簡單！十分鐘打造漂亮又好用的 zsh command line 環境](https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f)

---

#### 客製化步驟：

1. 換顏色
    - 教學是寫開啟 `cmd + I`，但這樣好像是改 Session 的顏色，等於重開視窗就回復成預設樣式
    - 所以我是改主程式的偏好設定 `cmd + ,` ，改掉預設樣式 
    - ![螢幕快照 2019-04-15 上午10.37.33](https://i.imgur.com/qRj2TLy.jpg)

2. 換字體
    - 同樣的，記得是在主程式的偏好設定裡更改
3. 安裝 ZSH
    - `sudo` 指令要輸入密碼，一開始沒發現輸入法是中文，一直進不了有點崩潰
4. 安裝 Oh My ZSH（管理 ZSH 設定檔（configuration）的框架）
5. 安裝 Auto Suggestions（ZSH 外掛）


參考：
- [為 MAC 的 Terminal 上色 - 透過 iTerm 2 和 Oh My Zsh 高亮你的終端機](https://pjchender.blogspot.com/2017/02/mac-terminal-iterm-2-oh-my-zsh.html)
- [[心得] iTerm2 + zsh，打造更好的工作環境](http://huli.logdown.com/posts/402147-iterm2-zsh-better-environment)

---

## 基本指令介紹

### `pwd` ：印出目前所在位置
- Print Working Directory 


### `ls` ： 列出當前資料夾底下的所有檔案 
- List
- 變化型
    - `ls -al`：列出更詳細的資訊（ 包括隱藏檔案 ）
    - ![螢幕快照 2019-04-15 上午10.25.39](https://i.imgur.com/HQrjieI.jpg){:width="600"}

        
### `cd` ：切換當前資料夾 
- Change Directory
- 變化型：
    - `cd ..`：回到上一層資料夾
    - `cd ~`： 回到根目錄
- 小訣竅
    - 當輸入 `cd 空格` 時，按 `tap` 會幫你自動列出底下的資料夾列表（ 等於是輸入 `ls` ）
    - 輸入前幾個字母，再按一次 `tap` 會幫你自動補完資料夾名稱

### `man` ： 使用說明書（ Windows 沒有 ）
- Manual
-  當你不確定某指令有哪些參數可以使用時，可以用 `man` 來看說明。
- 舉例：
    - `man ls`：會列出如何使用 `ls` 的規範文件
    - 按 `Q` 跳出說明

### `clear`： 清空 Terminal 版面 
    
---

## 檔案操作的相關指令

### `touch` 新增檔案 or 修改檔案時間
- 作用 1：如果輸入的檔案存在，則會修改檔案時間（ 好神奇 ）
    - 當你 `touch` 一個現有的檔案，檔案時間會改變成當前時間
    - ![螢幕快照 2019-04-15 上午10.59.11](https://i.imgur.com/w6BT4He.jpg){:width="600"}
- 作用 2：如果輸入的檔案不存在，則會新增檔案
    - ![螢幕快照 2019-04-15 上午11.11.51](https://i.imgur.com/DO4yOcy.jpg){:width="600"}

### `rm` ： 刪除檔案
- Remove
- 變化型：
    - 想刪除 test 資料夾，有以下兩種做法
        - `rmdir test`
        - `rm -r test`
            - `-r`：刪除資料夾及裡面所有檔案，謹慎使用！
    - `-f`：跳過確認訊息，直接刪除檔案
        - 有點危險、可能會誤刪重要檔案，盡量不要用 

            
###  `mkdir` ： 新增資料夾
- Make Directory

###  `mv` ： 移動檔案 or 改檔名
- Move
- 作用 1：移動檔案
    - 當找到該資料夾時，檔案會移到資料夾裡
    - 舉例：
        - `mv file foler`：把 `file` 這個檔案移到 `folder` 資料夾裡 
    - 注意 `folder` 分為 相對路徑 & 絕對路徑，假如我當前位置在桌面 `Desktop`，但要移動到桌面底下的 `folder` 資料夾
        - 相對路徑：相對於當前資料夾（ 此例為 `Desktop` ）
            - `mv file foler`
        - 絕對路徑：以根目錄為為標準，通常以 `/` 開頭
            - `mv file /Users/yakim/Desktop/folder`
- 作用 2：改變檔名
    - 當找不到該資料夾時，會改變檔案名稱
    - 舉例：
        - `mv file new_name`：把 `file` 這個檔案名稱改成 `new_name` 
    
###  `cp` ： 複製檔案
- Copy
- 舉例：
    - `cp file file_2`：複製一個 `file_2` 檔案
- 變化型：
    - 複製資料夾
    - `cp -r folder folder_2`：複製一個 `folder_2` 資料夾 

### `cat` ： 快速查看檔案內容

`cat test`：另一個方法不用額外開啟其他文字編輯器，也可以快速查看檔案內容。
 
![螢幕快照 2019-04-15 下午12.25.04](https://i.imgur.com/IZa45GW.jpg){:width="600"}

---

## VIM 文字編輯器

極度不友善但看起來很帥的文字編輯器，有分普通 & 輸入模式。

### 切換模式：
- 按下 `esc`：進普通模式
    - 可以刪除、複製、貼上
    - 但不能輸入文字
- 按下 `i` ：進輸入模式
    - 可以輸入文字 

### 跳出編輯器：（先切換到普通模式）
- 結束： `:q` 
- 存檔後結束： `:wq` 

---

## 其他常見的好用指令

### `grep`： 抓出並高亮關鍵字
- 舉例：
    - `grep keyword file`
    - ![螢幕快照 2019-04-15 下午12.41.02](https://i.imgur.com/ypJqL4C.jpg){:width="600"}

    
### `wget` ： 下載檔案
- 非內建指令，所以需要自行安裝：
    1. 失敗經驗，只是想記錄一下拿磚塊砸自己腳的過程
        - 看到網路上的教程前幾則都是用 yum 安裝 wget，所以先安裝 yum
        - 參考教學：[centos 系统如何安装 yum 工具？](https://newsn.net/say/centos-yum.html)
            - yum 官網下載位置：http://yum.baseurl.org
            - 結果教程是用 wget 下載 yum 安裝檔... （ WTF ? ）
            - 放鬆心情、改手動下載 zip 、照步驟安裝 
            - 回報說沒有裝 rpm...（ WTF !!!!）
            - 放棄此教學
   2. 看這個就好，因為超簡單就可裝好 
        - 參考教學：[Homebrew](https://brew.sh/index_zh-tw)
        - 安裝 Homebrew
        - 用 Homebrew 安裝： `brew install wget`

            
### `curl` ： 送出 request
- 送出 request，主要用來測試 API，也可以像 `wget` 下載檔案
    - `curl API_url`： 輸入 api 網址，會用 get 方式顯示出回應
- 變化型：
    - `curl -I API_url`：顯示出更多資訊
- 更多操作可以參考：
    - [Linux Curl Command 指令與基本操作入門教學](https://blog.techbridge.cc/2019/02/01/linux-curl-command-tutorial/)

---

## 指令組合技

### `>` ： 將指令的結果存到檔案內容
- Redirection
- 重新導向 input output
- 舉例：
    - `ls > file` ：將 `ls` 輸出的內容存到 `file` 檔案
    - 注意： 
        - 使用 `>` 符號為 **全部覆蓋**，所以原本 file 的內容會被取代
        - 如果不想整個覆蓋掉，只是想要新增內容，可以使用 `>>`
代替。
    - ![螢幕快照 2019-04-15 下午2.52.56](https://i.imgur.com/E35bX7b.jpg){:width="600"}
        
### `|` ： 將左邊指令的**輸出**，變成右邊指令的**輸入** （ 這句話聽了好多遍才聽懂啊 ）
- Pipe
- ![螢幕快照 2019-04-15 下午3.20.37](https://i.imgur.com/7W0zgB5.jpg){:width="600"}

### 試試結合以上兩種符號

![螢幕快照 2019-04-15 下午3.29.44](https://i.imgur.com/xQoUoed.jpg){:width="600"}

無用題外話：一聽到 Pipe ，想說這詞怎麼這麼耳熟，原來是美劇「矽谷」裡面的公司名稱 Pied Pipe。（ 很無用吧！ ）


### `echo` ： 將資訊輸出到 **螢幕** 或 **檔案** 中

參考資料： [維基 - echo (命令)](https://zh.wikipedia.org/wiki/Echo_(命令))

```shell
$ echo This is a test.
This is a test.
$ echo "This is a test." > test.txt
$ cat test.txt
This is a test.
```

---

## 後言

隔天就開始學 Git ，但如果有任何的檔案改動機會，盡量都使用這一節學到的 Command Line 指令，不得不說是個很好的練習機會，跟學習語言一樣，派得上用場才會體現出價值。

### 學習小樂趣

例如當我想把資料夾 `folder1/` 搬到資料夾 `folder2/` 裡面，Huli 上一節有教到 **移動檔案** 的指令是 `mv`，然而按照之前檔案跟資料夾關係的邏輯，開始亂試會不會 `mvdir foler1 foler2` 或是 `mv -r foler1 foler2`，當然我每次這樣亂猜幾乎都不會成功啦哈哈哈哈，最後還是認命爬文比較實在。

（後來才發現貼心的 Termial 有高亮提示，錯誤的指令在還沒送出前就可以看出來了） 

#### 爬文後，學到操作資料夾的其他指令

- 移動資料夾： `mv foler1/ folder2/` （ 可惡，不能統一嗎？ ）
- 改資料夾檔名： `mv folder new_name` 
    - 和修改單個檔案名稱一樣
