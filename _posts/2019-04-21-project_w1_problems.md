---
title: "[第一週] 問題總整理"
layout: post
description: "分支只是一個指向某個 Commit 的指標，刪除這個指標並不會造成那些 Commit 消失。"
category: project
image: 
tags: [Git, GitHub, CLI, problems, project_week1]
file_name: 2019-04-21-project_w1_problems
---

## 關於 Git 的疑問 
    
> Q: `pull` 跟 `clone` 的差別

A: `Clone` 跟 `Pull` 這兩個指令的應用場景是不同的。如果這個專案你是第一次看到，想要下載到你的電腦裡，請使用 `Clone` 指令；如果你已經下載回來了，你只是想要更新最新的線上版內容，請使用 `Pull`（或 `Fetch`）指令。
簡單的說，`Clone` 指令通常只會使用第一次，`Clone` 之後的更新，就是 `Pull/Fetch` 的事了。

---

> Q: `fetch` 是什麼？

`git pull = git fetch + git merge`

參考資料：
- [Pull 下載更新](https://gitbook.tw/chapters/github/pull-from-github.html)

---

> Q: `push` & `pull` 是全部分支都進行同步，還是只有當前分支

A: 預設是當前分支。

之間的關係是 `git push <远程主机名> <本地分支名>:<远程分支名>`， `远程主机名` 的名稱預設應該是 `origin`。
而要把本地的**所有分支**都推到遠端可以用 `git push --all origin`。

參考資料：
- [git push命令详解](https://blog.csdn.net/sky1203850702/article/details/41344131)
- [30 天精通 Git 版本控管 (28)：了解 GitHub 的 fork 與 pull request 版控流程](https://ithelp.ithome.com.tw/articles/10140305)

---

### [為你自己學 Git](https://gitbook.tw) 

真的是非常詳細且講得很清楚的一系列教學，把看到有趣的紀錄下來：

- [【狀況題】等等，這行程式誰寫的？](https://gitbook.tw/chapters/using-git/git-blame.html)
    - `git blame` 抓兇手
- [【狀況題】剛才的 Commit 後悔了，想要拆掉重做…](https://gitbook.tw/chapters/using-git/reset-commit.html)
    - `git reset` 搞清楚相對與絕對
    - 這篇也把絕對跟相對的關係解釋得很好：[30 天精通 Git 版本控管 (12)：認識 Git 物件的相對名稱](https://ithelp.ithome.com.tw/articles/10136575)
- [【狀況題】我可以從過去的某個 Commit 再長一個新的分支出來嗎？](https://gitbook.tw/chapters/branch/branch-from-old-commit.html)
    - 應該是蠻常見的情形
- [【狀況題】不小心把還沒合併的分支砍掉了，救得回來嗎？](https://gitbook.tw/chapters/branch/restore-deleted-but-unmerged-branch.html)
    - 這篇對 branch 的解釋，讓我有種恍然大悟的感覺
    - 分支只是一個指向某個 Commit 的指標，刪除這個指標並不會造成那些 Commit 消失。

---

## 關於 CLI 的疑問 

> Q: 要怎麼知道程式跑完了？  

因為之前也會跑一些指令，有些跑有點久、而且過程中完全沒有提示，通常是安裝或下載檔案，所以每次都很困惑、到底是還沒結束還是已經卡住了？

（ 爬文結果，不確定正確 ）

A: 许多命令会花费一些时间来执行，然而这中间不会给出任何提示或者进度条。一般结束后会出现一个“用户名$”的标记。如果没有出现，那么说明最后一条命令正在执行。

參考資料：[Mac OS X Terminal 101：终端使用初级教程](https://www.renfei.org/blog/mac-os-x-terminal-101.html)

---

> Q: `sudo` 指令都要輸入密碼？

A: 光是要看懂 `sudo` 到底是什麼就不簡單，好像問錯問題了，先放棄。

---

> Q: 要清空版面，直接下 `clear` 跟 使用快捷鍵 `control + L` 有差別嗎？

A: 好像沒啥人有相同的問題，我猜應該就是一樣的。

---

> Q: 輸入 `:wq` 沒有問題，但直接輸入 `:q` 會跳出錯誤訊息： `E37: No write since last change (add ! to override)`。

A: 輸入 `:q!` 就可以不存檔離開。
（ 其實錯誤訊息已經寫得很直白，但我一直以為是在檔案內容加上 `!` ... ）

參考資料：[How do I quit from Vi?](https://unix.stackexchange.com/questions/3334/how-do-i-quit-from-vi)

---

> Q: 當我要刪除資料夾，使用 `rm -r folder` ，照理說應該要跳出提示問我：「確定要刪除嗎？」，但卻沒有跳出來，奇怪了。

A: 再用 `man` 看一次文件，好像是正常的。

如果想要有提示，可以輸入 `rm -ri folder`。（ 但還是就像 Huli 說的，有點危險、盡量不要用吧！ ） 