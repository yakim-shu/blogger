---
title: "[第五週] 練習串 GitHub API 抓同學作業"
layout: post
description: "平常如果想要參考同學的作業，一直覺得步驟有點繁瑣，決定實驗有沒有辦法把同學在 GitHub 的作業、以當週為分類下載。"
category: project
image: https://i.imgur.com/EEehoSQ.jpg
tags: [API, GitHub API, project_week5]
file_name: 2019-05-13-project_w5_practice_github_api
---

## 契機 

平常如果想要參考同學的作業，一直覺得步驟有點繁瑣：

`點開 Issue` > `點進留言附的 pr 連結` > `進去 Code 標籤` (註1) > `homeworks/` > `當週資料夾/` 

[ 註 1 ] 其實點進 File-Change 也可以，但紅紅綠綠的我看了很痛苦。

> 洗澡時點子找上門，決定實驗有沒有辦法把同學在 GitHub 的作業、以當週為分類下載。

### 遇到的困難 1 : 沒取得 token，一小時只能發 `60` 個 request

像個無知的愚婦，一開始串得很開心、用第一個迴圈跑的時候，出現了以下訊息。還好錯誤訊息非常簡單好懂，也附上了文件連結。但 token 分很多種，說實在有點搞不懂之間的差異。

```
HTTP/1.1 403 Forbidden
{
  "message": "Maximum number of login attempts exceeded. Please try again later.",
  "documentation_url": "https://developer.github.com/v3"
}
```
最後是使用了 `personal API tokens`，加上去一小時可以發 `5000` 個 request，送到突破天際也不怕。

### 遇到的困難 2 : Callback 不熟、只好砍掉重練
一開始用 require 抓，但要發太多 request，處理資料完全超出能力所及。
雖然很掙扎，因為前前後後 5 個小時的沈沒成本... 但就算都知道怎麼拿資料，敗在 callback 太多、我完全束手無策，還是決定整個砍掉重練。

換下 curl 指令用腳本寫，邏輯變得超級簡單啊，雖然字串的篩選寫的很爛，但有了前面的經驗後，沒有碰到太多問題。最後終於有點小成果，有點感動啊！

### 最後完成的程式邏輯
- `curl` 用 Issue ID 取得 title & user-name & label
- `curl` 取得該 ueser 的當週 repo 裡面的所有檔案的 file-name
- 新建資料夾，名稱為 `[week-number]` `user-name` `label`
- `curl` 下載 repo 裡面的所有檔案

###  執行方法
`./get.sh <id-start> <id-end>`

![執行結果](https://i.imgur.com/EEehoSQ.jpg)


## 感想： 結果不如預期，但做中學很開心
- 雖然跟想像中不同、簡化操作的目的沒有達成，看檔案也沒比較方便。
- 程式一堆 bug，邏輯非常沒有彈性、下載時間也很久。
- 本來還想著如果其他同學有同樣的困擾，還可以一起分享寫出來的小工具，但最後實用性實在是不高就還是算了哈哈哈哈。
- 能夠實現自己天馬行空的想法，真的無與倫比。
    
> 此練習剛好運用了之前的課程所學，有 `week1 的挑戰題 + 超級挑戰題` 跟 `week4 的串接 API 作業` 的經驗，其實並不會很難實現，超級感謝 Huli 用心的課程設計。

### 詢問建議

因為抓檔案的效率不好，也懷疑自己是不是用很愚蠢的方法在實現目的，問了 Huli 有沒有改善的方法，得到關鍵字 `parallel`，原本速度的問題應該是出在順序下載，所以可以改成抓完所有 url 再一次下載，真是非常感謝！日後會好好研究，等優化完程式、有實用性再來分享。
    
### 之後優化目標

- 速度慢：
  - 抓一個人的當週 repo 的所有檔案（ 每週作業大概都 5, 6 個檔案 ），大概要 10 ~ 20 秒不等。
  - 用 `parallel` 改善下載效率，參考 [範例](https://github.com/julienc91/multidl)
- 撈資料：
    - 如果檔案是資料夾要再往內層爬資料
    - 字串篩選的方式寫的很瞎，可能要研究一下正規表示法
    - Issue 的標題要是不符合 `[Week_num]`，要如何排除與防呆

### 還沒優化的程式碼

（ `<your-token>` 要改成你申請的 token ）
<script src="https://gist.github.com/yakim-shu/7e22c8c8dc90882d62e4b00c1b1e6472.js"></script>