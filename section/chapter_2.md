
# ZMQ 的安全與架構 in node.js
## Speaker: Casear Chu
## 記錄：胡學賓、張維元

## zmq安全架構

在場有用 ZMQ 可以舉一下手嗎？是用在 Internet 上面嗎？還是 intranet 中間的加密自己處理嗎？
casear chu，跟 caeser 很像，這是 slide 的url，這是我養的貓，因為上一場有 show 貓的照片，我曾經在微軟跟一些地方工作過，現在自己出來做一個產品，叫做 sildenow，slide 就是用markdown 做 slide，現在看到的 slide 就是用 sildenow 做出來的，如果你覺得東西蠻好玩的話，有興趣可以來找我。

## 與 zmq 緣由
 
第一個，我們要從一個遊戲開始，怕時間不夠，我有做一個 module 叫 zmq-soa，你可以把它裝在 node.js 上，裝的步驟如果是在 mac 上，就install zmq-soa 就好，如果是在 ubuntu 就看看 apt-get zmq-soa 。你可以看看這個程式碼，這個程式碼是在幹什麼的，在日本機器架一台 server ，是在 zmq-soa 的架構，那用底下這個程式會將用 email 傳過去，等到我們的產品上線，你就會收到邀請碼，記得執行時要進去 production mode 才不會顯示 log，如果要看城市碼 code 可以到silde

為什麼要自己用 zmq 做這樣的東西，（後面的影片只是要說 slide 可以用 show 影片）
大概在去年接了一個專案要做付費系統，卡片、信用卡可能會有很多種，如果增加一種卡片就要去改程式碼，因為不想 server 寫死，所以我增加一些 client （worker） 去處理卡片的工作，如果有一個 user 想要用悠遊卡，只要丟給悠遊卡的 worker 就可以幫你處理，最主要是卡片消費的服務太多，所以才做這樣的東西

## zmq soa

soa 的架構主要是想要 xmpp，xmapp 就跟 msn 的那種感覺一樣，一個 server 提供很多 client 連上去，protocol 有很多東西，像是傳檔案或是幹嘛的，一開始想使用 xmpp，但是 node-xmpp 的問題很多，另外他有很多做法，像是要怎麼解析都有一定的方式，就必須要再去看 spec，，而且常常無預警的掛點，所以才開始尋找其他方案

第一個，client 如果瞬斷，必須要處理重連
第二個，確保對方都有收到
第三個，如果可以的話，有 msg 的 queue 會比較好，不然對方太忙的話會無法處理
再來，就是 retry

然後我就 goolge，第一個找到是新世紀通訊函式庫，其實就是 zmq ，看到這個標題就覺得超炫，覺得一定要用，那我就上去 google 找 zmq with node.js。（zmq 其實有幫你做完很多種函式庫，像是 C# 的 module，不過寫 CLRZMQ 的很爛，可以改用 NetQ）
ZMQ 處理事情基本的寫法，ZMQ 可以處理很多事情，例如重連，client/server 先啟，不過ZMQ 誰先啟不重要，如果 client 起來沒連到，過一段時間會自動重 retry，直到連上會把資料送過去，如果還沒連上就送資料，會先 queuey 在本地端，所以不用擔心瞬斷或 server shut down的問題

## zmq 基本連線

基本的 ZMQ，連線的方式有這四種：

第一種就是 req 跟 respone，不過第一種是不能用的，他的問題是，如果 server 沒有 response回來，client 就會停在那邊一直等

第二種 dealer 跟 router 就不會有這種狀況，會一直送一直接

第三種是 publish 跟 subscribe，user 可以subscribe一個主題，當 server publish 這個主題，所有 subscribe 的人都可以收到的，有點像 redis

最後是 pull 跟 push ，可能有一個 job 要送上去，如果有三個人在收，隨便一個收到都可以，以上是最基本的處理方式

我們基本上應該是用第二種方式，有一個 request 跟 response，我一開始用的覺得很奇怪，因為像 socket 可以雙向互傳，可是 zmq 不行，一定要 client 先送過去，server 再回我，不能server 就直接送回來，研究後才知道可以用 router 做到這個功能，但是它有第一個要做的動作，user 一定要送一筆資料過去 server，讓 server 知道 user 的 id，server 才開始知道可以送給它。

## zmq 機制

zmq會自動 reconnect，但沒有 onconnect，ondiscon 所以不知道什麼時候連線，什麼時候斷線。

這有一個很大的問題，如果處理的東西很重要，一定要送到 server 上，也要馬上收到結果，例如有人付錢，sever 斷了，這個東西就會一直 queue 在電腦，serve r也沒有 response，client 跟本就知不知道有沒有送成功或是斷了，等到送出去之後可能就沒用了，或是說如果斷了之後，後面幾百個人同時付費，會一直 queue 本地端上，等到 server 開啟之後，會有瞬間很多筆資料送過去。

加密，zmq 的傳輸過程中沒有加密，雖然他包起來，但是資料是沒有加密的，所以加密是一個很大的問題，當 internet 要傳敏感的資料，如果沒有加密的話是完全不能用的。

驗證，zmq 沒有驗證的功能，client 端連上就連上了，所以沒有辦法限制 client 送資料過來，只能限制資料能不能送過來，不過這樣在 server 上才能做檢查，所以我做了zmq-soa 的 package，今天主要不是推廣，是講過程中的遇到的問題。

你也知道工程師懶得寫 readme，這是我昨天晚上才寫的，slide 這個圖的架構是在 zmq 網站上面的 document，那上面會教你怎麼用跟有很多 pattern，這些 patten 是架構的 pattern，如果你要做什麼 zmq 功能，像是加密/驗證都要由 pattern 實現，這是我實際上拿來做的 pattern 的架構，中間是 brocker，可以幫忙 client 請 worker 工作，我裡面還設計了一些功能，像是寫了一個 client 專門在記 log，每做一個事情，worker 就會請 worker log 去記錄 log，裡面基本上都是用 js 去寫的

server 也是一個 Queue server，同時有同名的 worker 可以輪流做事，一個在忙的時候就會由另外一個去做的，這是剛剛那個 pattern 的名字

## 潛在可能問題及處置

第一個問題，reliable，你不知道節點是活的還是死的，裡面的 pattern 提到，可以用 heartbeat 來做，heartbeat 就是送以一個無意義的訊息給 server 讓他知道你有連線，heartbeat ping-pong 是雙向的

基本上流程大概就是這樣：client 會跟 server 說 hello，server 如果 ok 會回傳 ready，client 就可以確定已經連線了沒，然後繼續做 heartbeat，一個說 hello 一個說 welcome，重複做這件事，pattern 說可以時間可以設定在 2～3s，但我認為太短，所以我設定在設定 10s，因為 10s 斷線是我可以接受的，如果十秒沒有回我，會先 disconnect，再做 testing socket，如果斷線的話這些 heartbeat 會queue 本地端上，然後定時用 TCP 看看有沒有通，如果通，才會繼續送，另外 zmq 有一個 Queue watermark，每個 client 有自己的 watermark，如果 queue 資料太多，超過水平就會把這些 queue 都丟掉，server 也有 event 去偵測 user 上線/斷線，用這個 event 可以知道現在有多少 user 在線上

第一個 demo 就是 heartbeat，你要先起一個 zmqsql 很簡單，告訴他 port 跟聽的 range，再來 clinet 可以用 soa.clent() 做連線，因為我們有下 debugmode，裡面會做一個 heartbeat 的訊息會顯現出來，每過一段時間會跑一下，就是 heartbeat 的過程（user 取得 heartbeat 的訊息，再丟一個 heartbeat 過去）

timeout，server 上面會計算一個時間當 timeout，送了一個服務給 worker 很久沒都沒回，如果超過 timeout 就會告訴 client 重送

第三個問題是 security 的部分，送的資料上面沒有加解密，一般我們用用 tls 或是 ssl 之類的，你如果去官網看，他自己有說要做新的假解密方式，不過目前沒有看到單純用 js 就可以用的東西，他有出一個 c 的 library，比較新的橢圓加密 XP 不支援，再來就是加解密，大部分 TLS/SSL，連線 server 時，server 會給我 private key，client 會拿到 privet key ，再加密就好，如果擔心送過去的資料會被篡改，就可以利用 private key 去做驗證，過程有用到 ursa，沒有遇到什麼問題，但前幾天 nodejs 的群組好像有遇到問題，透過 ursa 可以產生一個新的 key，目前機器每次啟動，都會產生一個新的 key，client 重連連到的話就會換 key，encrypt 跟decrypt 有一定的大小，必須照著他的方式作加解密，如果自己寫的話記得要分段。

## 使用 ZMQ 實際範例

這個是 sign 跟 verify，資料要確保是哪個 client 送，加解密這樣就可以搞定，不會有 key 搞錯的問題，brocker 送出來的 private 是一樣的，當然 heartbeat 是不加密的，只是要確保有沒有連線而已，只是送一個自定的 buffer 格式，server 會拿頭幾個 byte flag 知道怎麼處理，不會看裡面的資料是什麼

broker auth，我們現在中間都加密，要驗證合法的使用者，所以必須要提供你的 pwd，透過一個 function 來做，broker 會有一個 event，如果正確/錯誤回傳 true/false，他以後送過來的都不會處理，以上是完全透過 js 來控制，沒有什麼加解密的東西，因為 pkg 都把段包起來了，正常worker 收到的資料的 buffer，因為不知道內容是字還是什麼，在我的應用裡面有可能是送檔案

這一段是 client 在做的，有一個 worker 的 event，ready 的 event，auth 的 event，連上線會確認，驗證成功會開始送資料，第三個要看驗證成功或資料由 server 的 result來看，

再來我們看一下 auth，大概就是這樣一段，ready 開始送 auth(key) 過去 server，如果送的內容不只是字串，還想要送什麼帳號密碼，就可能要把 json 弄成字串送過去，server 也是自己定義怎麼確認的。

client 有分 worker 跟 client 兩種，client 只是送服務，不回傳任何東西，worker 的功能差在要不要加 service 的名字，可以由這個來判別，也可以透過 broker 處理。
我自己的實作，在 db 存的很多列表，每個卡片都有序號，所以可以根據這個去看他是什麼worker，run 起來大概會長什麼樣子，我會檢查他的 auth，或 service，可以透過這邊去給他
為了避免太多 debug 訊息雜訊，所以改成 production mode。

brocker 寫說他已經起來了，再來 clinet 就連上了，會顯示是誰連上了，如果我斷線，也會告訴你誰斷線了，直接回傳 false 說現在沒有辦法送，disconnert 會顯示 connect ip port failed。會顯示 brocker 存不存在，不存在的話就會送 ready 給他，再做 SSL key 來做交換。

我現在起了一個 brocker，起了一個 jsdc 的 service，另外一個 client，只是要 service，如果成功的話，最後會 get 到一個 msg，剛剛要做的事情就是送一個 email 過去，worker 再把 email 回傳回來，就可以做很多種服務是你想做的。最後 demo push，brocker 有很多 worker，想要跟每一個 worker 都想訊息，例如想要 shut down 的時候，我寫個一個 publish 的 function 可以做到這件事，我的程式指定是，15 秒送” jsdc caeser ”這個字，這個 library 基本上是這樣。

## Q&A

### Q1 ：有考慮 redundity 嗎？

A1 ：目前沒有做，因為 heartbeat 15 秒，掛了會重啟，應該不會有這個問題，也可以搭配 redis 處理

### Q2：ZMQ，公司裡面會有軟體跟系統的人員，這樣要如何考慮這樣的產品，因為 ZMQ 不是那個完成 xmapp 的工程，所以應該要由軟體人員實做，還是要系統的人員完成？

A2：看服務考慮，不能有錯誤的話就找軟體，如果自己要弄一個小的 service 可以自己用這一套弄，因為我們的系統不是很龐大的東西，ZMQ 比 TCP 高一層可以，程式裡面溝通也可以用它，所以拿來跟軟體比不太一樣。

### Q3：沒有選擇 socket.io，選 zmq 的原因是什麼？

A3：socket.io 比較走向為了 web 處理，一般 server 考量會用socket，另外，client 用 socket.io 也可以跟 web 溝通，他們的層次不太一樣

## 關鍵字

* ZMQ
* internet intranet
* slidenow
* markdown
* ZMQ-SOA
* node.js
* 
* NODE_ENV=production node test.js
* 
* worker 處理卡片的工作
* XMPP
* protocal
* node-xmpp
* xml
* message queues
* 新世紀通訊函式庫 ＝ zmq
* ZMQ with node.js
* C# clr-mq net-mq
* 
* Connect type
* Req/Res
* Dealer/Router
* Pub/Sub (publish/subscribe)
* Pull/Push
* Router
*   D:Hello
*   R:Store id
*   R:Welcome
* Reconnect
*   onConnect?
*   onDisconnect?
* Encryption
* Authenticate
* npm
* Broker
* Queue Server
* Service Oriented
* heartbeat ping-pong connect
* C;hello
* S:ready
* C:onConnection
* C:hello
* S:welcome
* Testing Socket
* Queue Watermark ：queue超過水平就丟掉
* Timeout
* 
* 
* Security : Encryption,verify
* CurveCP
* NaCL
* TLS
* handshake-類似TLS
* RSA
* pub key
* 3DES key
* ursa
* RSA key
* RSA Encrypt分段
*   RSA sign
*   RSA Verify
*   buffer
*   flag
* Authentication
*   Broker Auth
*   Client Auth
*   json
*   consumer
*   worker
* Publish to worker