# Micro Databases
##Speaker: substack(James Halliday)
### 記錄：FIsh Pan、甘煜堂 2014/10/11(六) 16:30 - 17:20

今天我會跟大家談論有關微資料庫。首先我們先談一下背景的部份，我是 unix 的大粉絲，而不是說建一個 program 做很多事情，一個加一個。

一個 program 的 output 變成另外一個 input，如果你要寫個很大的程式，如果你沒辦法去處理它，我倒希望你能做一個小小的程式。leveldb 是屬於 google 的 db，基本上你有這些東西，最重要的部份就是 create readstream，你要給它 readstream 的時間，leveldb 基本上是很容易開始進行的。

如果你用這個 program 來 run 的話，我們就拿回這個 value 了，如果我們有一些 data，然後有一些新的 value，我們可以使用 db.batch(require)。

（demo）

所以沒有這樣的 data 在裡頭，無論如何，實際上，我們可以用來做一些來測試，做一些readtream 出來，這應該把 data 放在 database 裡頭去。

無論如何，leveldb 現在展示不太好，他可以做圖樣順序的排列，這個 case 是簡單的案子，leveldb 可以做這些處理，所以你可以把 bytewise 的順序做出來，那很重要的，它可以sort array，先確定我們要有一些 data 在這，我要做的是把它 fix 起來，現在我們可以做 bitewise，另外一個要做的是，我可以說 users，用這個 properties，不需要再 batch 了，應該給我這些users。

一般它是可以 run 的，所以我想新的 database，可以很快的把它建立起來，確定它是 value 的，至少得到值了，可以繼續下去了，那這是什麼意思呢？我們應該要有的 key 是user，我們也可以 getpost，但是是單數而不是複數，我們也可以用 batch coding 的方式，每個特定的 user，index 到某個 users去。這個 timestand 的值，文件最後沒有找到，我個人沒有在這裡頭找到太多的問題，他是完全模組化，你可以把 database 的參數，你要做兩個 database，在 npm 做一個 module，給它一個key，sublevel 的好處。

所以我們用這個不錯的 db 技術，這個 database 叫做 substack，我可以把它 print out 出來，那我們就可以有文的部份，我寫了一個程式可以把它拿出來，所以你可以看到所有的字從 database 印出來，所以我們去上面的 database，做一個 structure，你可以用各種各樣的替代，你可以 level、sublevel 做，那這就是我們所要做的，就像我之前所做，我們就可以 push 這些值，把這些連起來就可以得到這些值。我們可以 code.get，英翻中或中翻文，我已經 input 的好了，我可以 run 這個，我們可以做另一個方向，最重要的是它們是模組化的。完全可以分割開的，但是你可以改善它，這是一個比較不好的案例，有其他比較好的案例。

我們可以做的是，LIST 很多事情，你要挑戰的話，可以code這個 function，另一個部份，叫做 level party，用這個 package，做好之後有另一個，我們可以 pass 給所有的 user ，你可以管理 user，這所有的 code 在這邊。首先我們 run一個 server，給它一個值，如果你要的話，所以做好的話，我也可以做完全一樣的事情，是非常模組化的。

（demo）

另外一個好的例子，這個方法叫做 content-addressable-blob-store，你可以在這邊做，這是一個新的特性，我用這個翻譯不知是否對，但是很有趣。做一個新的 store，那看看他的 finish，我把它做成 stream，然後把它存起來。另外一個 document，做一個 store，設定 key 到 process 2，現在可以 read 這個 key，這是一個 powerful 的方式，沒有database 幫你做，那麼有很細緻，或者做反過來的。

require exchange module 出來，然後這邊我們是用記憶體，你可以從另外一個 party，然後你可以得到另外一個回應。我有這個 exchange file，所以我有這樣的一個 module。

那這個在 command line 可以做的，如果我們給第二個 command，有些 key 沒有它，

forkdb，做一個新的 docuemnt，不管你從哪得到這個 document，這個整個順序是對的，forkdb 的好處是，當我在 local run 這個，這是我們的 hash，我們得到 hey yo。

（demo）

現在，我把它大寫，這個 work 了。應該會得到 hey yo，整個 database 的 document，最終進去到它的歷史，這就是往回指的，第一個 database point to 第二個 database，做好了我們可以複製，這兩個 command 可以互相複製，所以第二個 database 來的訊息，是那麼多向的複製。

（demo）

wikidb，我給它一個 database，那這個叫做 oakland wiki，你可以在網站上看到其他人做的application，還有更多的micro db，


## Q&A

###Q1：micro db 可以用在比較大的專案裡嗎?

A1：我不知道，要視專案為主，你把不同的資料存在不同的系統，但是這是一個不同的方法來設計這個系統。

### Q2：我想請教 level db 的效益?

A2：它是比較 low level，原始的 db，但是談到成本的話，必需要視專案為主，但是速度很快

### Q3：如何變成一個很棒的programmer

A3：寫很多programm

## 關鍵字

* modelarity composition parse
* tiny embedded dB
* db.get put del batch createReadStream
* a->b->c  use bytewise
* for keys and values index
* keyEncoding: require(‘bytewise’)
* valueEncoding: ”json”
* modularity
* translation db
* dictdb level db
* level party 
* web server 
* dir
* createWhiteStream
* exchange
* 雙工運作 
* A->B->C forkdb history 
* --prev.key 
* sync 互相複製
* wikidb  recent change 
* maildb batchdb treedb//micro db