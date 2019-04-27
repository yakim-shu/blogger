---
title: "[第一週] Shell Script 腳本"
layout: post
description: "今天認真看了一些教學，其實語法還真的不難，被 [鳥哥的 Linux 私房菜] 的豐富內容嚇到以為很難。"
category: project
image: 
tags: [shell, CLI, project_week1]
file_name: 2019-04-27-project_w1_shell_script
---

認真看了一些教學，其實語法還真的不難，被 [鳥哥的 Linux 私房菜](http://xyz1943.idv.tw/~xyz1943/data-linux/20060117102949/#script_why) 的豐富內容嚇到以為很難，之後看到一個鐵人幫文章 - [快快樂樂學會讓電腦幫我做事](https://ithelp.ithome.com.tw/articles/10129445)，寫得很簡單易懂，非常適合入門的初學者。（ 話說本來要留言表示感謝，結果被擋說等級太低無法留言... ）

還在初階、培養興趣的階段，我還是需要一個講人話的教學啊，等到進階後、還有興趣或碰到問題時，鳥哥的存在就真的是全人類的幸福啊！


### 寫 script 的重點就三個字：自動化

#### 根據鳥哥的回答：連續指令單一化

對於新手而言， script 最簡單的功能就是：『彙整一些在 command line 下達的連續指令，將他寫入 scripts 當中，而由直接執行 scripts 來啟動一連串的 command line 指令輸出入！』

> 但具體來說，shell script 能夠如何改善我的生活？

我知道大家都會說自動化、但我對於具體的想像還是很模糊，有什麼生活中的例子可以應用的嗎？看到一個也是鐵人幫的回答算是解答我的疑惑：

以下說明來自：[Linux shell script 常用的程式有哪些](https://ithelp.ithome.com.tw/questions/10188680)  ( 回答得非常具體、推薦！ )

#### 很久才做一次、又繁瑣的事情

你也可以把好久才要做一次，然後每次都忘記細節要怎麼弄，查一陣子才想起來的事情自動化。例如程式發佈新版本，網站要放上最新消息，修改下載連結，軟體頁的版本與歷史版本都要更動。包含多國語系的頁面，哩哩扣扣要修改將近十個檔案。即使詳細的步驟都寫成筆記了，有一次還是漏改了檔案，乾脆讓 script 自動去處理。從此發佈新版我只要將版本號與版本說明設好，網頁要改的東西都自動處理好了。

#### 多台環境的安裝、設定

另一個久久才跑一次的例子是伺服器環境的安裝與設定。我們一次更新機器就是幾十台，伺服器上面除了常用的那些套件以外，還要跑自行開發的幾個服務。我在進公司以後遇到第一次更新伺服器就花時間將所有東西自動化了

#### 重要又怕自己忘記的工作

某個沒用版本控管的網站。檔案修改後要先傳到 develop 去，修改過的檔案要移到備份資料夾並加上時間戳，確認沒問題才能傳到 production。有一次在找誰把 develop 的東西丟到 production 去，我說不是我，因為全部都 script 自動在跑，這東西跑幾個月都沒事所以不可能今天突然傳到錯的機器去。當然也不可能發生忘了備份就把檔案覆蓋的事情。

---

## 實用文章記錄備用

由鐵人幫文章中挑出幾篇實用的，以後再來認真練習：

如果有寫過程式的人，對於 shell script 入門會非常的快，因為 shell script 本身就算是個簡單的程式，差別在於不用編譯（compile），他是逐行執行的。

- 也可以寫成 function：[[Shell Script] Day25-提高可讀性之函示寫法(一)](https://ithelp.ithome.com.tw/articles/10138189)
- 寫作業提早看到這篇就不用掙扎了哈哈哈：[[Shell Script] Day19-常用的指令介紹之grep和awk](https://ithelp.ithome.com.tw/articles/10136126)
- 把實用的指令帶入情境使用、寫的很生動：[[Shell Script] Day18-常用的指令介紹之男人與時間](https://ithelp.ithome.com.tw/articles/10135661)

---

## 反問：自己能用 Script 幫到什麼忙、減少工作
 
- 重新命名巨量圖片：
    - [Linux Shell大師進化論-實戰演練之批量
重命名](https://kknews.cc/other/jkokvx6.html)
    - [Linux Script：mv, rename 單次及批次修改檔案名稱](https://www.ewdna.com/2012/04/linux-scriptmv-rename.html)
- 撈資料、做篩選
- 初始環境設置
    - 開一個新的 Git 專案資料夾、包括 `mkdir`、`git init`、`add .gitignore`
- 要不停重複做的事
    - 例如用 Git 上傳文章時，把 `add` & `commit` & `push` 一起執行
---

參考資料：
- [鳥哥的 Linux 私房菜](http://xyz1943.idv.tw/~xyz1943/data-linux/20060117102949/#script_why)
- [Linux shell script 常用的程式有哪些](https://ithelp.ithome.com.tw/questions/10188680) 
- [鐵人幫系列文 - 快快樂樂學會讓電腦幫我做事](https://ithelp.ithome.com.tw/articles/10129445)