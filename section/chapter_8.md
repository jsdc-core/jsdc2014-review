# GitHub 大冒險
## Speaker: Muan

哈囉大家好，這是我第一次在台灣演講，也是第一次用中文言講，我叫做秋陌安muan，我是在台灣長大的小孩，我在台灣一直念到大一下，念中央法文，後來休學因為法文太清閒了，總總機緣跑到英國唸書，在英國工作幾年，我同時是嘖嘖的創辦人，是我跟幾個英國朋友創辦的，我負責做網站，整個網站前端後端資料庫是我一個人做的，我在github主要做設計，前端，偶爾寫ruby，我現在是遠端工作我目前住在台灣，70%員工是遠端，不住在舊金山，這是我們全公司的照片，我把我跟ＯＯ指出來，我們在我們的CEO頭上比椰，源端工作很重要的是，要用非同步的溝通方式，因為不可能同時溝通，用很多email，pull issue，留言在聊天室等等，你不會期待對方立刻回話，因為這樣，不能要很容易被阻篩住，假如我問別人問題還沒回我的話，可能過兩天都還沒回我我應該要怎麼辦，不能一直等答案，所以要常常一心多用，可能要有多個同時執行

## education.github.com

一個人可以有很多團隊，自己想加入就去加入，不是誰指派給你，我是做設計的所以一定會在設計裡面，設計工程無障礙，錢跟教育，如果你發現錢有錯誤的話，那應該是我的錯，教育的話，有給學生的免費帳號，只要你是在做研究的人都有這個帳號

## accessibility

首先，你想這是我的engineer team，裡面又包含很多team，設計是包含在其中，另外還有一個CSSteam，還有一個小小的accessibility，那是我一開始創力的，那是有關有關使用需要的，我就看到很多使用者的問題，所以我開始開心這個問題，因為我很愛很愛github，有人沒辦法用這是很痛苦的事情

第一個是我們的前端團隊，韓我們做的lintion的方法跟無障礙設計

一般公司前端跟設計團隊是分開的，但我們這邊是幾乎一模一模，你會看到有gap，因為有些很hard code的javascripters，大部分的公司都不是這樣運作的，設計師是提出一個prototype，不會空口說白話，大家都可以直接看你的demo，你就是送出一個pull request，而不是一個人見一個東西，也不知道你們講得是不是一樣的，因為口說無憑

## style guide

第二的，我們都很在乎我們寫出來的東西好不好，所以有style guide告訴你，應該要怎麼寫程式，向html就是寫少一點會越好，假如有個css sudoclass可以做到就不要新增一個tag，除此之外，設計師會一直很involved，你送出一個req之後你就要一直iv到做完為止，所以對我來說，設計師會寫程式的，就像魯膚吃了橡膠果實一樣會很厲害ㄝ，是一個很特別組合

## hubot

把測試加到你的CI裡頭去，如我測試沒過的話你就不能deploy，我們有一個聊天室機器人，聊天室會自動幫你測試，送到production，像這個例子，所以你可以測試raw data，因為我丟的branch可能會把github用壞的，好險hubot沒有讓我這樣做

## deep selectors

CSS我們有很多不同的測試再告訴你寫的是不是好的，第一個會測試已經沒有用到的id或是partials，你沒有用到的話就應該把它刪掉，會去找有沒有相對應的配對，如果沒有的話就會失敗，規則最多只能有5曾，希望可以到三層，因為太多層的話就很難被overwrite且無法重複利用，也會影響到效能，一個按鈕會連接到一個js event，如果有這個行為的話，就要在css tag加上js prefix，可是這些測試都很簡單，在資料夾找有沒有css名稱，有的話就會告訴你，你不能把css style，不能用js prefix來做define

## aria label

accessilbily，可是在乎的人很多，有辦法做什麼人很少，所以我們大部份做的修改都是被動的方式，可以看到有哪些人，colorblind這個team比較哀傷，不見得有辦法修他，因為我們資源少，三個人只有兩個人會修，所以我們不可能一行一行去看，所以我們做了一個js log it的程式，檢查有沒有alt 屬性，如果沒有的話就會跳出warning，所以這個錯誤就不用自己去找，然後有我們js是怎麼做這個的，找一個label找，還有就是，`<a>` 如果沒有 href或是role的話voice over會沒有辦法測試的，所以js寫法就是check有沒有這兩個而已，只有一個icon的按鈕，他就會逼醫生，你也不知道他是幹嘛，所以我們家個一個測試，到底tag有沒有文字說明，如果沒有的話就會加上一個aria label。

要非同步的溝通是比較好的，因為我習慣github的方式，我爸媽就會很受不了，因為
要non-blocking，知道卡住了，就要繼續做下去不要卡住

我之前在香港有說為什麼設計師要學程式，首頁製作百寶箱，我就是看那個網站開始學的，小時候不知道設計室設計，一直以為自己要當畫家，在學css跟html的時候完全不知道phothshop，所以對我來說他們就是工作，就像畫家跟顏料一樣

大的project很容易就變得很mass，雖然html css廣泛被認為不是程式語言，我認為html跟css還是要定期除雜草的，我認為他們也是跟後端城市一樣需要被測試

昨天有人問到關於前，向我們的做法就是用最小的資源去打到無障礙的成果，有心的話是可以做的到了，我還知道這個accessibilitycamp.org
，它是一個conference，你有一群人註冊去，邀請大家對這個議題有興趣的人，去分組討論，是沒有講師的，這個東西是從2011開始，每年都有半比較不同的，參加都是不用錢的，前幾個月有在東京辦一次，
如果有人在意的話，也可能去試著做做看。


## Q&A

### Q1: github有多少中文？大陸跟台灣，你們會用中文在留言板聊天嗎？

A1: 我沒想過這件事情，不過台灣有兩個人，另一個是會計師，一個是香港人但是在美國長大
公司的日本人也會講中文，但是中文不是很好

### Q2:hardcore javascripter ，他們主要是在做什麼東西？不用做design？

A2:對他們不用設計，他們寫比較多ruby，跟最其他很多事情，或者focus在效能跟framework上

###Q3:CI，可以介紹你們做這件事情嗎？

A3:我們有三個測試環境，我之前都是在新創公司，所以之前寫完都會馬上送，test會在三個不同的環境run，沒過就不能deploy，在chatroom，會自動幫我送出測試

### Q4: design 在 prototype要做的什麼程度？如果designer從頭跟到尾的話，時間掌管要由誰負責？

A4:pull request越早送越好，要把pull request當做一個討論區，我們沒有deadline，可是大家都是工作狂，

### Q5: 剛剛你有提到你在臺北也有一些講座，你也有說跟你做成朋友的話你就會教他們，那想問一下你有沒有在台灣做比較技術方面的研討，

a5:我很宅，所以我不太出門，下禮拜活動學員有50個，我之後可能會試著半半看patch work，如果大家有興趣幫忙的話跟我講一下，學習的話就比較沒有，在倫敦會去幫忙，在台灣還沒有過

### Q6: 不是技術的人使用github，有打算把web UI做翻譯，因為對做UI的人說，英文可能不是很好

a 6:三四年前曾經把ui翻譯成各種語言，但後來發現似乎很多人希望我們也能用各種語言回答問題

### Q7: 你們有在做style的，在提交比較大的feture前是否會先做測試？
A7: 類似twitter bootstap的東西，