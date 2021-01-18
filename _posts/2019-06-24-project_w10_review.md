---
title: "[第十週] 參加「 程式導師實驗計畫第三期 」的第十週心得"
layout: post
description: ""
category: project
image: https://i.imgur.com/g0TBWhr.png
tags: [review, project_week10]
file_name: 2019-06-24-project_w10_review
---

這次比較懶惰，大多都是對每章節很廢的心得註解，紀錄一下新學到的東西，希望學習路上總是能快樂比挫折多一點。

#### 還需要練習的部分
- CSS Grid 排版
- Fetch, Promise
- async/await
- JSONP 應用練習
- OOP 練習

----
### 第六週：舒適圈
本週應該是整個計畫裡面最熟的一部分。

切版對我來說有點又愛又恨，如果是切自己的設計稿，再累也甘之如飴。但依以前的工作經驗，有時候要是切千奇百怪的設計稿，厭世指數會突破天際 🤣 

這週開始愛上用這網站 [Typing.com](https://www.typing.com/student/start) 練習英
打，但現在完全忘記有這回事哈哈哈哈

##### [ Chrome 網頁除錯功能大解密 ]
在 Debug 那塊學到很多技巧，剛好在 week7 的作業派上用場，覺得在進入會寫比較複雜的程式碼之前，先教怎麼除錯真的很棒！

##### [ HTML、CSS ]
雖然很久沒寫，但還好依稀記得有哪些屬性，雖然 HTML 標籤跟 CSS 新屬性多又雜，但我一介金魚腦也是寫久就會記下來。

認真把 flex 屬性跟 form 標籤研究過一輪，也算是補齊以前忘了就查的壞習慣，開始嘗試搬到腦子裡的長期記憶區裡。

##### [ SEO ]
比較陌生的領域，只有個概念，在前公司因為是新聞網站、流量很大，完全不用我來擔心 SEO 這一塊，反倒是比較著墨在新聞部的人對標題、tag、圖、圖說這些內容如何下的精準。

#### 作業
- `仿 Medium`： 不誇張我切了快三天，不過還好 Medium 本來就滿精美的，把厭世感稍微壓下來一點，但切完還是有點想吐，所以挑戰題我真的無法。
    - 不過現實世界有些設計師就真的會跟你叼那 1px（ 雖然毫無原則的我當設計應該不會 ）
    - 嘗試了 BEM 的寫法，還滿不習慣的，沒有辦法完全照規則走，所以現階段還是自己取捨，覺得舊習慣比較好的地方、可能就會選擇混用。
    - 能把 `flex` 好好實踐滿棒的，學到的新語法 `sticky` 也有應用上，看到一些同學的回饋更是開心。

小發現： 在切文字列表時意外發現可以用 `偽元素` 自定義數字列表，覺得超酷。

```css
.article__body ol {
  counter-reset:post;
}
.article__body li::before{
  counter-increment: post;
  content: counter(post) ".";
}
```

#### 筆記
- [[第六週] HTML - tag 基礎標籤介紹](https://yakimhsu.com/project/project_w6_HTML_tag.html)
- [[第六週] HTML - 表單 form 介紹](https://yakimhsu.com/project/project_w6_HTML_form.html)
- [[第六週] 基礎 SEO 標籤 - meta、og、JSON-LD](https://yakimhsu.com/project/project_w6_HTML_SEO.html) 
- [[第六週] CSS - 跟著 🐸🐸 學 Flex 排版](https://yakimhsu.com/project/project_w6_CSS_flex.html)

---
### 第七週：考驗邏輯規劃的作業
本週的難度很明顯上升一個檔次，甚至我覺得作業的邏輯規劃更是這四週最難的。

當然要做得多完整是個人選擇，有時會很猶豫要不要先交作業，但想到某個功能只要再花一些時間、說不定就可以研究出來了！

我一直都不是那種會想追求完美的類型，身為一個墓誌銘可以刻上得過且過的人，人生都是抱著有個 7.80 分就很滿意了，所以在寫作業的時候有這種心情覺得還滿意外的，也挺開心自己可以認真投入。

##### [ 操作 DOM 介面 ]
最有趣的部分就是程式可以直接改變到介面這塊，以前用慣 jQuery，一開始覺得用 Vanilla.js  很不習慣（ 這個為戲謔而生的詞好幽默 ），後來發現其實也還好，沒有麻煩到哪去，可能是因為現在沒有寫到太多操作畫面的內容，且  `querySelector()` 解決了超常用但又很雞肋的選擇器語法。

##### [ 事件傳遞 & callback ]
以前只知道有冒泡的概念，沒想到如此複雜，Huli 的文章真的幫了很多忙，交作業的時候被糾正觀念，這就是 mentor 的好處啊，能夠及時糾正非常感謝～

#### 作業
- `反應力遊戲` 
    - 在複習週時試圖重構成 OOP，但自己好像太心急了根本越級打怪，結果當然是越構越糟，閱讀性也變差，但給自己氣餒個一天就沒事了，也是個不錯的經驗。
    - [初始版本](https://github.com/Lidemy/mentor-program-3rd-yakim-shu/blob/aea7c413016c40b4b61194e25580eac8be54eb0f/homeworks/week7/hw1/main.js) > [OOP 版本](https://github.com/Lidemy/mentor-program-3rd-yakim-shu/blob/d34609ce25e31dde3923f138747cf3ab79d47cd5/homeworks/week7/hw1/refactor.js)
- `仿 Google 表單` 
    - 在處理 radio 的時候踩了不少坑，之後看同學作業邏輯切分的很清楚，深感佩服。
- `計算機`
    - 從一開始規劃頁面就挺興奮的一個作業，感覺的出來如果想做的完整、難度會比之前的都要高上不少。
    - 當初有點野心太大，要處理兩排顯示數字還滿複雜的，而且我發現自己都是專注在畫面上的結果，而比較欠缺邏輯上的考慮，所以還是有滿多 bug，目前都是以強制歸零為先，希望以後可以把作業補全的更完整。

#### 筆記
- [[第七週] DOM - 操作 DOM 介面、事件監聽](https://yakimhsu.com/project/project_w7_DOM.html)
- [[第七週] DOM - 事件傳遞機制：捕獲與冒泡、事件代理](https://yakimhsu.com/project/project_w7_eventListener.html)
- [[第七週] 瀏覽器資料儲存 - Cookie、LocalStorage、SessionStorage](https://yakimhsu.com/project/project_w7_storage.html)

---
### 第八週：又見 API
發現課程影片在第七週就看完了，突然有種賺到的感覺，在寫作業時收穫挺多。

##### [ 交換資料 - 表單、Ajax]
以前會寫一些簡單的表單跟 Ajax 串 API，但就是停留在「 知道怎麼寫會得到我要的資料 」的層次。

至於背後的 Request 怎麼送、又怎麼接 Response，中間會有哪些機制在跑，在瀏覽器間的限制是什麼，我通通不了解，又或是說「 甚至不知道我該了解？ 」

所以有了 week 4 詳細的解釋網路傳輸的原理、 HTTP 的協定，這樣的課程安排對我這種半桶水非常有幫助。

##### [ 同源政策、CORS、JSONP ]
最驚訝的是原來有簡單請求這個機制，這種安全機制的背後或許有一些慘痛的經驗，如果有對這部分有多一點著墨，應該會滿有趣的。

#### 作業
- `hw1: 王 MAMA 抽獎` 
    - 因為參數有點多，一開始想用物件當參數傳入，但寫了許久才改成比較理想的方式。
- `hw2: 王 MAMA 留言板`
    - 同樣是在想如何將 Request 簡化、目的是將 GET & POST 方法結合成一組，但這題的參數不像 hw1 是寫死的，POST 的 `msg` 跟 GET 的 `page` 都需要傳變數以動態更換。
    - 後來靈光一閃，想到用 function 包不就解決了嗎？把參數傳進去再 return 出我要的 object 格式，是我覺得寫這作業學到最實用的技巧！
- `hw3: Twitch 直播`
    - 有了前幾次的練習後，串 API 相對沒有問題，這題的難關在於搜尋功能跟無限滾動。
    - 對於要用哪些屬性做計算查了很久，`offset`、`scrollTop`、`scrollHeight`、`innerHeight`... 有時間要好好研究寫個筆記，畢竟計算頁面有沒有滾到哪裡的功能其實滿常會用到。
    - 要實現搜尋功能，還是要再查一次正則表達式，還真是想躲也躲不掉，還好這 case 不難，未來要優化按 ⬇︎ 按鍵可以逐步選到提示框。

#### 筆記
- [[第八週] 交換資料 - 表單、Ajax、XMLHttpRequest](https://yakimhsu.com/project/project_w8_Ajax_Form.html)
- [[第八週] 交換資料 - 同源政策、CORS、JSONP](https://yakimhsu.com/project/project_w8_CORS_JSONP.html)

---
### 第九週： 後端初體驗
要跨入後端的第一步很興奮，之前想自學 PHP ，但被環境設定卡的不能動彈，伺服器那端也是毫無頭緒，這部分真的很需要手把手的帶，所以迎來第九週豐富爆棚的學習影片，其實還滿安心的，加上重複的片段不少，無形中加強重要的部分很受用。

##### [ PHP ]
語法寫過一次就沒那麼難適應了，感覺字串拼接是大家的心頭痛，這點我倒覺得還好欸，可能 JS ES6 以前也是用這種靠背拼接法，太方便還會想說是不是有詐（？）

倒是 User Story 的部分要寫得完整很吃經驗，雖然通常是 PM 負責，但如果是單獨要做出完整專案，還是要好好練習規劃，感覺會大大影響資料庫設計。

##### [ SQL ]
Excel 是人類最美好的發明，沒有之一，SQL 給我感覺就像用指令去操作 Excel。
雖然還不熟，但 SQL 語法寫起來很有趣。（ 或是還沒被折磨前都覺得很好玩 ）

##### [ OOP ]
難以摸透的概念啊，我可以理解怎麼寫可以達成什麼目的，但在大量練習前，還難以明辨各情況的 Best Practices。 
不過聽到直播 Huli 也說大概第三年才有頭緒，簡直是會行走的強心針呢！

#### 作業
- `hw2, hw3 留言板`
    - 有了 BE101 的助攻，作業寫起來還滿無痛的，想說只是留言板有點無聊，就亂定了一個名言大亂鬥的主題，時間都花在這 [網站](https://www.juzimi.com/writers) 流連忘返，沒想到有同學留言居然真的照我規則走，感謝配合的同學哈哈哈哈哈
    - DB Class 參考網路上的寫法，本來用 `mysqli_fetch_array` 把取出所有資料的方法也寫在 Class 裡 ，但又想到這樣外部呼叫的時候，不是也同樣要跑迴圈印資料嗎？感覺有點脫褲子放屁就算了

#### 筆記
- [[第九週] 後端基礎 - PHP、資料庫、SQL 語法](https://yakimhsu.com/project/project_w9_PHP_SQL.html)
- [[第九週] SQL - 內建函式、JOIN、排序](https://yakimhsu.com/project/project_w9_SQL_JOIN.html)
- [[第九週] OOP - 物件導向基礎概念](https://yakimhsu.com/project/project_w9_OOP.html)

---
### 第十週： 拖延週

##### [ Code Review ]
原先理想計畫是複習週可以地毯式 Review 這四周的所有作業，但還是太低估這地獄般的作業量，看完 week7 hw1 & hw2 就果斷放棄，雖然真的可以學到不少，但耐心消磨得更快，真心佩服 Huli 每天消化拔山倒樹而來的作業。

Code Reivew 心得：
- [Week7 hw1](/review_w7_hw1.md)
- [Week7 hw2](/review_w7_hw2.md)

##### [ 重構 Week7 hw1 ]
是一次打擊信心的重構。現在想想，我從一開始就沒有想好「 為何重構？ 」。

當時看著 hw1 覺得可以寫得更好，但第一版的「 哪個部分很差、哪個部分其實 OK 」其實我沒有自信答的出來，所以連**要改善什麼的前提都沒有釐清的狀況下，這次重構算是未經思考的**。

只因第九週學到了新的概念，「 彷彿 」OOP 是可以讓作業變得更好的急效藥，只想著引用封裝的概念，如果把外部的介面簡化，應該就是比較好的作法吧？！真的完全就是為了物件導向而物件導向。

經過這次的經驗，下次大概不會如此著急應用新學到的用法，就算是練習也要先評估素材是否適合。


##### [ 綜合能力測驗 ]
原先看到空白，直覺是把 CSS 都刪掉，結果還是一樣可惡啊！後來認真研究 PHP 後覺得：乾好有趣喔，也挺順利拿到 flag。

藉此剛好問一下，請問是拿到 flag 就結束了嗎？還是後面還有關卡、我只是沒找到入口、不得其門而入，因為有種比賽才剛開始，就被宣布各位該回家囉～的錯愕感，我還想繼續玩啊！

##### [ r3:0 異世界網站挑戰 ]
也是非常喜歡啊！尤其是顏文字跟最後一題，我是看提示找 window 物件的方法才過關，看別人心得才知道可以從顏文字中找到線索，JavaScript 的大千世界充滿奧秘。

能跟如此神的同學一起參加這計劃非常激勵人心，菸酒生不都超忙的嗎，強烈懷疑同學有操縱時間的能力？！

---
### 關於拖延
先自首這週很不用心，還有搬家等一些外務干擾，且搬回家住變得超級懶散，沙發頻頻對我喊著「 躺在這很舒服唷！ 」

想到要一次回顧前五週就覺得，乾肩膀好重、好值得拖延，雖然沒規定不能寫的很簡短，但還是希望未來回顧可以多一點具體細節，畢竟我很常懷疑自己是不是活在空白之中、大腦放在浴缸之類的。

拖延的症頭在這兩週嚴重起來，我自己分析一些原因，但又想著把這些原因抽掉應該還是一樣，對於學習的內容都是一些痛苦、一些樂在其中，人絕對都是好逸惡勞的，所以我的結論是：得自己想辦法讓學習樂子增加才行。

想到前幾天看到爭議女王 XDite 的發言（ 其實我也不清楚她現在評價如何 ），她說自己克服拖延、寫出新書的其中一個技巧是，先把自己會心虛的事做盡，例如狂打電動、連打兩天打到吐，最後就會自己心虛到爆炸，乖乖的開始瘋狂寫作。

我覺得這想法很有趣，感覺跟趕死線時、效率會突破性成長有點像。

進度報告可以看出不少同學深受拖延之苦，Huli 也很適時地跳出來鼓勵大家，真的是很想繼續拖延呢（喂）

這種時候我覺得再多的治拖延技巧也比不過心態上的轉換，所以正在努力的轉念，但真的挺難的，第一步像 Huli 文章寫的，不要再為拖延找藉口。

回顧以往，每當認真想做一件事，初期經常是死命往前衝、把自己精力榨到一滴不剩，拼過頭的後果換來後繼無力，這種短跑心態來到兩個月的時間，差不多就是自己最容易彈性疲乏的階段，希望我可以在此計畫進步成不擅長的長跑選手。

---
### 小建議

#### ☞ 作業 Issue 的推薦原因可以更精確
這邊純粹是以一個想要看 Code 的學生角度出發，因為作業實在太多份，不太可能每份作業都可以好好看過一遍。

雖然現在有值得參考標籤，但就比較像一個大集合，雖然 Huli 有時候會在底下留言值得參考的原因。但比較無法快速篩選我想要參考的作業，所以如果有其他推薦原因的標籤應該還不錯，例如：`邏輯不錯`、`版面美觀`、`新語法`、`底下回應 Q&A`、`簡答題`... 等等之類的。

所以今天假如有同學只關心程式碼部分，如果看到只有掛版面美觀的標籤、就可以直接略過了。

會有這個想法是在地毯式 Code reivew week7 作業時，在滿多沒有掛`值得推薦`的作業也學到不少，雖然要做的話可能會讓你工作量增加哈哈哈哈，反正有許願有機會 🤣

#### ☞ 希望可以多辦幾次直播問問題

有鑒於上次直播問題很踴躍的情況，後來在想為什麼會這麼多問題，有幾個想法。

個人覺得匿名是至關重要的一個因素，因為「 發問的內容不再跟我這個人 」有所連結，所以可以很大膽的想問就問。尤其是在公開（？）的場合下，會擔心說自己是不是又問了個蠢問題、或是這樣問會不會顯得很伸手牌。

但要表現出「自己不是個伸手牌」的成本也挺高的，所以我自己也是想到發問前的準備工作：把問題解釋清楚、查資料、截圖、附範例程式碼... 等等，最後都懶得問了哈哈哈

當然我不是指直播上的問題就不用認真準備，但我覺得對於學生角度「 可以不用想這麼多 」，如果問題很隨便，也可以直接回答說這看起來像沒查過資料、直接跳過。
    
還有一點是 sli.do 上可以按問題 like，如果時間不夠，應該也可以只回答最多人有共鳴的問題，而且對於站在 Huli 的角度，也可以明顯看出學生哪個部分不懂。

總結來說，我覺得直播回答問題的好處是，匿名比較能夠放心問、問答成本雙方都小、可以讓同學投票想要問的問題，比起用文字溝通，拋答問題的方式感覺有效率。

最後許願直播問問題可以多辦個幾場，當然也要你那邊網路允許的前提下啦，或許兩、三週一次？

----
### 一臉正經發廢文

最後我只想寫一些很廢的內容，靈感來自於 Huli 之前分享的緩和文字溝通，所以也來分析一下個人會用的符號。

打從網路誕生的那刻，我跟自己約法三章不能用 `= =` & `XD` 的符號（單純不喜歡，沒有任何理由）（乾破功），所以少了超實用的差滴使用權，該怎麼在文字對話中讓語氣看起來緩和呢？

- 這個變數名稱好奇怪 😂
- 這個變數名稱好奇怪 (๑´ㅂ`๑)
- 這個變數名稱好奇怪 ～
- 這個變數名稱好奇怪 ！！！
- 這個變數名稱好奇怪 哈哈哈哈哈
- 這個變數名稱好奇怪 QQ

#### 選手比較

- `emoji`  
非常理所當然的選擇，但很常用 😂 又顯得很不誠懇，各平台的 emoji 也長得不太一樣，加上要決定哪個圖案最貼切也不容易（ 裝上 chrome 外掛後有改善 ），總之是個容易掉入花時間選圖的陷阱中

- `顏文字`
跟 emoji 一樣門檻比較高，投入時間多，吃電波、遊走在非萌即宅的高風險區 (°ཀ°)

- `～`
在結尾加上 `～` 以模擬語氣飄揚的效果，副作用是看起來很輕浮～

- `！！！`
數量已 1 到 3 個為佳，同一段文章至多用一次，否則瀰漫著精神錯亂的氛圍！！！

- `哈哈哈哈哈`
哈哈哈哈哈好處是句子前後都可以加，但使用時機更加狹隘，再加上不明白對方的屬性前使用有點冒險，使用不當會讓人引起「 哩希勒秋殺小 」的反感。

- `嗚嗚`、`QQ`
營造一個楚楚可憐小動物感，讓人拳頭硬不起來。

#### 結論
每個選項都各有優缺，完全視當下情境、文字內容、對象以及閱讀空氣的能力來決定，但不管是哪一個都有不誠懇的風險。

還有更重要的是，如果使用者肩上背著羞恥心的沈重框架，恐怕難以靈活使用，且要是太靈活應用，有很大的機率會看起來像人格分裂。

所以如果都顧了這麼多、對方還是受傷了該怎麼辦，第一種可能就是文字內容太不恰當，第二種是對方就是個玻璃心，前者好好檢討自己，後者讓對方好好檢討自己。