
# node-ffi, 用 node 使用 c/c++ lib
## Speaker：othree
### 記錄：胡學賓、張維元


我的演講就這樣開始吧，這是我的投影片，讓我簡單自我介紹一下。我網路上用的 ID 是 othree，網站是 othree.net，現在是 mozilla taiwan community 的成員，今天會先介紹 r 3這個 library 是什麼，然後透過 node ffi 把這東西跟 node.js 接在一起。然後最後會介紹 node r3，我設計的那個 library 大概要怎麼用。

## r3 導入 node-ffi

先從 r3 開始，r3 是 c9s 寫的，應該很多人都知道，在 github 上算有名的台灣開發者，他寫一個 router library，他超級快。他用的原理是是有限狀態自動機（DFA）跟自動化，東西進去會自己會跑到某一個狀態，資料結構是前綴樹或是叫 prefix tree，或是叫做 tri，比較特別的一個專有名詞。這個 library 很好用，他自己本身使用流程是先把 tree create 出來，然後把 route data 都 insert 進去，然後 compile 出來就可以用，程式碼看起來像這樣，這是 c 的程式碼，一開始先 create tree，他和 js 不一樣因為他不是物件導向，所以 insert 的這種 method 就是把 tree 的節點丟進去，就是第一個參數，然後後面就是和 route 相關的資料，然後最後就是 route data，屬於這個 route 的資料，這資料通常是你在這 router 要求進來後進入 router 希望拿到這資料，最後就會 compile，compile 之後就會產生類似這樣的一個樹，這就是前綴樹，可以看到每個節點都只有一部分的網址，他只要經過節點就會把東西放在現在的值的後面，就是最後經過的節點網址加起來就是 match 的一個網址。

那 tree compile 出來後要怎麼用？程式碼長這樣，tree compile 出來後要先 create matrix entry，然後拿這個 entry 跟 tree 做 match，然後就可以拿到樹上的節點，然後這節點的屬性就是我們要拿的 data，用法大概就是這樣。
剛剛沒講到 match entry，match entry 是 r3 的資料結構，你拿 requet 網址進去，也可以用來抓網址裡面的變數，或是 http method condition 也可以加上，未來打算加上 ip mask 和 port。

像剛剛那棵樹右邊兩個節點就有用到 angular expression，那 match entry 第一行黑色我有指定 method get，match 之後 entry 下的 vars 這個屬性就是底下就是抓到的 varible，這是一個字串陣列，這是自定的結構體，後面會稍微介紹一下。看到 r3 後就想能不能在node 裡面用這個 library，答案當然是 ok，不然今天大概就不用講了，問題其實是如何辦到這件事，目前我所知是用 node js 的 extension 然後 compile成 node package，有另外一個專案叫 sock，這也是一個台灣人，他的 id 和他的帳號剛好是反過來的，他也有一個node r3 的 project，用的就是這個方法。

## ffi 說明

另外一個方法用 foreign function interface，簡稱 ffi，這就是我用的方法，不過我是寫到一半才想到應該先看看有沒有人做同樣的事情，才發現上面那個 node r3 已經做到一半，不過最主要動機是在寫論文，只要不是寫論文的事情都想做做看，結果還真的把它做出來了，介紹一下 ffi，ffi 在維基百科定義是用 a 程式語言寫的程式去執行或是呼叫 b 程式語言寫的 library 或是他的 service，通常我們在 python 可能就只能用 python 的東西，js 就只能用 js 的。

ffi 提供介面可以在 a 程式用到 b 程式的程式碼，基本上要先了解 calling convetion，中文叫呼叫慣例，這慣例是說在執行一個函式的時候要怎麼送參數，怎麼執行他，然後怎麼把那些 return value 拿回來。

在組合語言這個階層的慣例，ffi 如果沒有特別說就是在講 call c 的動態 lib，如果要呼叫不同程式語言，各種語言有各自實作的方法，要去實作各自呼叫慣例才有辦法去執行，這是一個簡單的c標準的 function call 的組合語言寫法，上面這段是 c 的 code，下面這段是組合語言，這是執行 2+3 的範例，實際上要先把 3 push到 stack 在把 2 push 到 stack，再執行，再把 return value 拿回來，實際上我要做類似像這件事情，在 javascript 參數的數量是可以任意變動的，在呼叫時可以丟很多參數進去，組合語言裡就有相對於他的呼叫慣例，那 c 就會有不一樣的作法，ffi 提供了不同程式語言的 calling convention 介面。

node ffi 是 node ff 的 package，基本上就是給 node js 去 call c 或 c++的動態函式庫，node ffi 知道要如何去執行 c 或 c++ 的動態函式，如何去轉資料，因為資料轉換也是一個問題，怎麼送參數，取得 return value。
他的作者是這位，這是他的 ID，我不知道怎麼念，他不只是這個package的作者，他也是 node js 這本書的作者之一，除了 node-ffi 以外還有很多 package 跟 ffi 相關，後面都會介紹到，ffi package 除了 ffi 以外還有三個，ref 就是指標，ref－struct 就是結構體，c 或 c＋＋的結構體，再來就是 array helper。

## node-ffi 使用方式

再來看一下 node ffi 怎麼用，這是一個非常簡單的範例，npm install ffi 之後在程式碼就可以 require ffi 進來，然後他有一個 method 是 library，這是我們主要要 call 的，然後給他一組參數，裡面有一些定義，之後就可以拿 return value 的 method 來用，第一個參數是你要用的 library name，以這例子是 c 的標準數學相關 library，他會去用 dl open去找這 lib 在哪，第二個參數是你想用哪個 function。

除了 function name 以外還要講清他 return value 跟他收的參數的屬性，那這是回來的資料型別，因為會 c 跟 c＋＋宣告時就要宣告好參數的型別，所以這邊要做同樣的事情，他支援非常多的資料型態，他 int 有超過 javascript 的 int 上限，但是如果超過的話會轉成字串，object 也有，但我還沒有直接用過，剛剛那樣宣告沒有問題的話，直接就可以用 lib m，就能夠直接用 ceil，ceil 是天花板，也就是無條件進位，所以你要用 node ffi，基本上你要知道你要 call 的 function 的定義。

通常都寫在 .h 檔檔案。以剛剛的 ceil 來說，他的 .h 裡面的定義大概就長這樣，然後就改寫成下面這樣。中間要有個像是翻譯的過程，這就是相對應的位子，r3.h 一打開就看到結構體，剛剛講的東西還無法處理結構體，結構體在 c 那邊其實是一串記憶體，一個 array 知道 size，每個 byte 分別是代表哪個屬性，是什麼型別都定義好了，總之他其實就是一塊記憶體，所以事實上可以用 pointer 來取代實際的結構體，但是要用 pointer的話在 javascript 就無法把結構體的值取出來，如果要取出那個屬性的話就要用 ref-structure 這個 package。

## ref-structure

ref-structure 這個 package 用法其實還蠻簡單的，上面這結構體的定義就把它轉成 javascript 把它轉成像下面我有一塊 ref-structure 進來，然後把拿進來的東西當做 constructor，然後丟結構體的定義 key value pair 給他就好，就非常簡單。

ref-structure 實際上是實作了和 c 同樣的 library interface 的結構體，他其實也是在 node.js 裡拿一塊記憶體，根據你給他的資料型態去要那些大小的 size，然後你要哪些屬性出來的時候，他就是去抓某個 offset 然後個 data type size 出來，然後轉成 javascript 的物件。

c 和 c++ 的 api，是一個 binary interface，是一個比較底層的介面，他在 binary 層，就是 0101 的，在這邊 ref-structure 用到只有整數資料型態二進位要怎麼排，這邊用到的是他結構體如何去排二進位，實際上還有 machine code，你的 function 要怎麼轉成二進位存到記憶體都有定義，c 和 c++ 其實還沒有很完整的涵蓋到每個領域，其實不只是資料型態包括蠻廣泛的範圍。

## 結構體的部份

繼續看 header file 會發現會有這種情形，str array 剛剛有說到，這東西其實也是結構體，他不是 c 的，是另外一個結構體，他剛好又在另一個結構體裡面，像在這 case 我會需要另一個寫法，要用 ref，剛剛有提到的工具，ref 到 ref type 這個 method 來用，ref 是，node.js 的 package，他的用途就是讓你在 javascript 裡可以做像 c 的指標的一些處理，調整 buffer 大小之類的，這是他的文件，在 google 上找得到。

ref type 用處是從一個 data type 去找到那個 data type 的 reference，文件的說明是像這樣，他會 clone一份 data type，把 indirection 屬性加一，把 indirection 在 C 裡面就是在宣告變數的時候指標是幾層的星號，舉例來說，上面有個直接宣告的 structure var1 跟 structure pointer var2，下面的寫法會不一樣，第一個是第一層，就是直接指到的，第二個就是一個指標指到他，那所以我可以再更多層，譬如說兩個星號就是兩層指標，一個指標會指到 a 指標，a 指標會指到 str array。

結構體的部份處理完所以就繼續回到 r3.h，往下看會看到有些灰色的 define，define 是 c 的 preprocess，這些東西其實不是真的程式碼，在 compile 之前會先做字串的 replace，前面的東西都會被換成後面的，實際上這邊看起來有定義了一個 function，但實際上這個 library 是沒有這個 function 可以用的，遇到這種情形就只好在 javascript 裡做類似的事情，所以我就去寫一個假的前面的 function，然後去執行他實際要做的事情，r3.h 裡面目前看到的問題大概都可以解決，那就可以把 node r3 的定義寫出來，他大概有 200 多行，30 多個 method，還蠻多的，所以一開始我有去找一些自動化工具，這是目前比較多人在用的，他會用 clang 幫你做自動化，他會把 header（.h檔） 寫成 nodeffi 定義檔的工具，缺點是太詳細了，以我的 case 其實不太實用，且無法處理 define 的情形，所以我就自己寫定義，最後只拿了十個 method，和 tree，match entry 相關的東西，最後終於可以寫 node r3 出來試試看。

##  route data 的問題

事實上還有很多問題，這邊列三個比較有趣的，第一個問題 route data，剛剛有說這個 data 是一個 void pointer，型別是 void，意思就是說你丟什麼資料的 pointer 進去給他他都不管你，lib r3 就是他只記這個 pointer，match 到後會拿回一個 pointer，要做什麼是他都不管你，所以要自己處理這一塊，問題是 javascript 又沒有 pointer，可以用的最接近的就是 buffer，buffer 是 node.js 才有，buffer 其實就是一塊記憶體，他有個地址指到某個記憶體位址，有個 size，可以指定資料型態，nodeffi 丟 buffer 給他他就會轉成 pointer，所以剛好可以再這邊用。

這是一個簡單的 buffer，如果用 buffer 來做 route data程式就會長這樣，這邊是用一個字串，要把 javascript 的字串後面加上 null byte 00 的byte，c 的 string engine 是用 null byte 來做結束，所以我要自己加，程式碼會像這樣，那拿回來的時候會去 match route，然後 node 的 data 就是我要的東西，然後這邊有個 ref.DREF，是取值運算，實際上我剛拿到的 data 其實是新的 buffer instance，即使我對他做取值，他並不會回給我剛剛我給他的字串，因為新的 instance 他的 size 是 0，type 是 undefined，所以他就是只有一個 adress 指到某個位址，size 是 0 就是什麼東西都沒有，所以這時候要怎麼把值取回來呢？

有兩個方法，第一個方法是手動，如果我知道有拿回來就知道他多大什麼型態，這時候就可以去指定他的 buffe type，然後可以用ref.reintergrate 去 resize 這個本來 size 是 0 的東西，然後就可以把這個值拿回來，這個方法的問題就是我並不知道別人會送什麼東西當 data 給我，所以我這樣作必須要處理很多 data type，邏輯會太多，後來我並沒有這樣做，為了迴避這問題，我用了另一個方法，我先限制所有人只能用 string 這個 data type，string 這個 data type 我就可以用readCString 這個 method 來把 pointer 轉回我剛剛送給他的字串，readCString 比較特別的是他可以從 buffer 把字串讀出來，然後讀到 null byte 的時候才會停下來，而且縱使你讀超過那個 buffer 的 size 的話他也會停下來，所以剛剛那個 case 我就可以處理掉了，就是我丟給他的字串就可以用 readCString 把它讀回來，我不需要把它的 type 跟 size 重新設定回去，程式碼大概會像這樣，我把 pointer 拿出來然後再用 readCString 把 data 讀出來。

那這方法當然有缺點，就是被限制在只有 string，不過基本上 data 還是能丟進去拿回來，所以我就先繼續處理下一個問題，就是其實他的 performance 不好，所以第二個想要處理的問題是 performance，我用了一個跟 c9s r3 lib benchmark 同一組資料在 imac 上跑（i5的imac），我拿了另外一套叫 director，也是 node router的 library，看起來比較多人用，測試數據大概叫 director 是兩千多 request per second，我那版的 node  r3 比他還慢，我就想說這樣不行，我都已經用了超快的 library 來接了，結果竟然還這樣，不過這 1900 我現在怎麼跑都比他快，我不知道發生了什麼事，我在想什麼會花時間。

我想到兩個，第一個資料轉換，我去 create buffer 和把字串讀回來這個過程，這些事情我想可能都會有一些 overhead 在那邊浪費你的運算資源，首先第一個字串轉換，我要怎麼減少這字串轉換，程式碼像這樣，感覺就做很多事情，那我要怎麼略過這個過程，我想到的方法是把資料存在 javascript，就不送過去 r3 那邊，取而代之我送一個 interger 過去，因為 interger 大概比較簡單，不太需要轉，這個 interger 送過去是當一個索引，match 到之後我就去把資料要回來，程式碼變成像這樣，一開始會先拿到使用者給我的 data，存到這個 this.data 裡面，就是這個i ，就是我剛剛說的索引，那個i每次 insert route 都會加一，再來我拿這個i去用這個 method，我就可以直接給這個 method 我的 value 和buffer 的 size，然後他就會直接給我一個 buffer，我就去把 i 的 pointer 拿出來，然後把 buffer 當 data 送出，在下一行最後面，接下來還是要用 resize 處理一下，然後就可以用這個 method 把值讀出來。

這邊就是一個 interger，所以下面我就可以用 make interger 把我剛剛丟進去的資料讀出來，這個 method 是 readunit32LE 是 node 內建的，u 是 Unsigned 就是不管正負號，這樣修改我發現效能提昇很多大概是 2.5 倍多，有個好處就是 router data 不限制是字串，現在所有 javascript 的資料型態都可以丟，連 function 都可以丟，再來就是 foreign function call，其實有用到的就這些，然後實際上只有這兩個可以省略，這兩個可以省掉的條件有兩個，第一個就是完全不管 http method，只要同網址做的事情都一樣，另外一個就是網址裡面不會有參數，像是 restful 裡面有 id 的就沒辦法用，但是其實省略掉並不實用，我有去測試如果這兩樣都省掉效能可以提昇 1.5 到 2 倍，我目前還沒把東西加進去，因為 route library 不一定是給 http 用，程式碼互相溝通也可以用，所以我可能會給使用者自己決定，根據使用情境去判斷他需不需要這些東西，這是未來目標之一。

這兩個問題都解決，感覺速度有了，而且資料型態都沒有限制，感覺很棒，但其實他會 crash，這個不太清楚，你會看到他跑一跑，看最左邊有一排 1 有沒有，然後突然變成 16806400，這是我剛說的 index，就是他本來是 1，然後我就打同一個都是 1，然後打到某一天突然變成一個奇怪的數字，然後就 crash 了，有時候他就直接 segment fault，這個 bug 我找非常久，就藏在這裡面，這邊有個有獎徵答，大家可以一起來找 bug，找到這邊有個禮物，就是 mozilla 的 t－shirt，size 是 m，我先解釋一下這段程式在幹嘛，這個 router 是一個 constructor，他會建構 lib r3 的樹。

然後把 route 全部都插入後compile 然後回一個 library 物件給使用者去用，然後一開始我會宣告所有的 local varible，然後這邊就 create 一個 tree，就是用 r3 tree create，然後這個 size 是 ok，因為 r3 會看情況去 resize 他的 tree，然後這個是要存 data 用的屬性，使用者的 router data 都會放在這裡，再來我就去使用者給我 router 時的資料，假設他給的都適合規範的，他是給我一個物件，是 key-value pair。

key 就是 router 的網址，value 就是 data，然後這邊會用 for 迴圈把 key value 抓出來，然後這邊是 index，然後把 route data 丟進去，第二行就是我把 index 做成一個 buffer，然後就會變成新的 local data 物件，這個之後會 insert 到裡面，然後這邊比較沒關係，我可以說 bug 不再這啦，這邊就只是把拿到的 route  request url 去做一下處理，然後我會把它切成兩段。

如果使用者有支援 get/post，我有一個特別的格式，就是他可以用 get 空格然後網址這樣，然後我會用空格切成兩段看是不是 get post 格式，再來if就是判斷有沒有 get post 資料，如果有的話我會去設定 method 進去，然後把 route insert 進去，第一個是 tree，然後method，然後後面的資料可以假設他都是正確的，然後中間有個 i++，最後就是 compile 這個 tree，講解到這邊，有沒有人想要挑戰看看。
這個 bug 我想了很久，想的很久基本上就是蠢 bug。


## Q&A

### A：大於一嘛？

講者：不是

### A：undefined 嘛？

講者：undefined...no，route 你說小寫的還是大寫的，route 是使用者丟進來的。

### A:（太小聲）

講者：假設使用者丟進來的資料都正確。

剛剛投影片大家看的那一份是沒有答案的，所以就不用去找答案了，因為後面被我卡掉了，沒有人想挑戰那我們就繼續往下了。

這個 bug 就真的是非常蠢，data 是 local varible，javascript function 結束後就被Garbage collection，所以在 javascript 裡面會認為那塊記憶體是可以被清掉的，所以等我拿回來的時候拿到的是別的 instance，跟我丟進去的 data 沒有關係，所以我拿回來的是完全不一樣的東西，只是只到相同的位址，那東西已經被 javascript 清掉了，所以裡面的東西是不相關的兩回事，大家聽得懂嘛？再看一下 code，那個 data 在這邊拿的一塊記憶體，在這邊是 local varible，所以這 function 結束後沒有任何東西索引到他，就被清掉，所以只要我跑一跑他被清掉然後就 crash 掉，完全不知道發生什麼事，console.log 看也不知道怎麼回事，這個非常難找。

解決方法就是 keep 索引在他上面就好。就是有這感想，在 javascript 拿的 buffer 變數都要記得記下來，如果還要繼續用的話。

這三個問題都解完其實 node r3 就蠻穩定的，還有一點小問題我還在研究到底是怎樣，不過就都還很正常，然後那個時候大概是 0.0.3，然後後來就一直加一些 helper讓http server 比較好用，然後現在 0.0.9，不過 performance 都沒什麼變化。
最後就是稍微介紹 node r3 介面，npm install node r3 裝的是這個。

如果你 intall node-lib-r3 是另外一個人做的，我還不敢跟另一個比較誰的效能比較好。我這個 router 用法就是拿近來的物件有一個 router，會有兩層是因為我還有一個 path router，那個是希望能比較好用，那個東西是一個 constructor，new 這個 constructor 然後丟 router 的設定參數進去，key 是網址，value 是那個網址的 data，這個 tree compile 就可以用router.match 來抓我的 data 出來。

另外一個用法是我有提供 http helper，他可以接到 node http create server，在這例子我所有 data 都是 request handler 的 function，我可以在 key 裡面加 method，然後也可以加像第三個這個有 id 的變數。還有就是 route.http 這個 method 我還要給另外一個 function 去做例外處理，如果全部都不 match 的話就會去 call 那 function，handler function 參數基本上跟 node http 一樣，就是 request 和 response，另外會把抓到的變數放到第三個參數，例外處理就沒有那個參數，只有 request 和 response。

目前 node r3 我剛剛說還有一點小問題，使用上記憶體會慢慢一點點的增加，非常非常緩慢的增加，目前完全找不到，不知道是 node 本身就會吃記憶體還是我程式碼有問題，找很久還沒有找到，希望就是可以把我剛講的還有 performance 再改上去。不知道大家有沒有問題？和 js 相關的也可以或是和博士相關也可以。

### Q：router node r3 是想嘗試原本 javascript routing 很慢的問題嗎，這個 performance 看起來很驚人，如果能到 ten thousands per second其實是很實用的一套 library，我很好奇說除了這套以外是不是還有類似的東西？

A：第一個問題是因為我在寫博士論文，只要不是論文我都拿來弄看看，然後就剛好看到 c9s 有這個，就想說來接接看，然後研究一下發現可以。

第二個問題是其實我有開一個專案打算收集比較常見的 node route library，來比較 performance，不過我搜尋一下現在比較多人在用的就是 director 那一套，其他就有些就很久都沒有更新了，我就直接把它省略掉了，如果有什麼忽略掉很多人在用的 route library 的可以告訴我，我就會試著把它加上去，performance 要到上萬其實還有一點困難，雖然現在有機會上萬，但是使用情境還蠻有限制的，只有在內部的溝通可以用，如果要做外部的應用，我的 http server 目前還不太可能，透過 node 接效能下降很多，c9s 用他自己寫的 benchmark 大概有上百萬 request per second 的速度，到我這邊就剩千分之一不到，其實他還有寫一個叫 h3 可以 parse header library，不知道之後是不是有打算把它整個組起來之類的。


## 關鍵字

* ffi
* node-ffi
* r3
* node-r3
* router rb
* DFA(確定有限狀態自動機)
* prefix tree
* match_entry
* caasi/node-r3
* FFI(foreign function interface)
* calling convention(呼叫慣例)
* ffi
* ref
* ref-struct
* ref-array
* 
* ABI-compliant
* 
* ref.refType
* indirection
* node-ffi-generate
* Route data
* ref.deref(dereference)
* ref.readCString
* director
* readUint32LE
* Unsigned
* Little en..
* segment fault
* 
* Garbage collection
* helper
* handler
* Error Handling
* memory leak