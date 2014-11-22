
# Functional JavaScript, why or why not?
##Speaker: Greg Weng
### 記錄：胡學賓、張維元

 * 投影片：[http://bit.ly/jsdc2014-funjs](http://bit.ly/jsdc2014-funjs)

如何成功的用 javascript 做出 functional programming，什麼可以用、什麼不能用。我是 Mozilla Taiwan 的開發者，我負責 Firefox OS 的 lock screen 開發，因為牽扯到 security，做起來會比較複雜。如果我們 mozilla 有心得 feature，我會做到我的專案裡面。

會跟 functiona l扯上關係，是因為我學的 haskell。不會有 scheme/scala/lisp 的概念。我之前在當兵站哨的時候，會一直想 MVC 的東西。但是 MVC 很複雜，不適合站哨時想。於是朋友就介紹 functional 的東西，就想到了現在。
會做這簡報的東西是因為 moz 在台灣有 blog，這篇文章意外的 like 數滿高的。另外很多開發者對 function 不是很了解，於是就來跟大家介紹。

這個簡報不是 20 分鐘用某工具寫一個 blog 之類的，也不是 functional 裡面很數學的概念項 lambda calculus。這篇著重的是普通的 javascript，先從 functional programming 基本的概念來講。OOP 跟 FP 實在是很難比較誰好，他們只是用不同的想法來做 programming。

## functional programming 特色

如果你習慣 functional programming 可以重新讓你設好 programming，還有很多 pattern，還有很多有趣的特色。

到底有哪些特色才算 functional，投影片上前兩項在 javascript 都可以做到，recursion 需要runtime的支援。function 是 functional programming 最重要的東西。()=>{}在 firefox 裡面就是一個函式。投影片講解型別轉換的過程。最後一項在 firefox nightly有支援，在投影片裡會會大量用 function signature 來解說。像[]是一個 type variable。

可以想像 array 在 push 的時候也是一個 functional 的過程，是一個 immutable data structure。每一次都要回傳新的 array 給你。facebook 有做一套 immutable-js 的 lib，大家可以去玩玩看。

我們第一個重點是一個叫 high-order function 的東西，map 跟其他函式不同的地方是 map 接收了一個函式當作參數。這概念有用是因為光是用 map 跟 reduce 就可以架構生活上許多功能。使用 high order function 來處理 list 的好處是一行就能解決了。有些時候不是僅僅對行數有刪減，這個範例是從 array 建立一個 cluster 的架構就這幾行來講行數沒有減少特別多。可以參考我們 Gaia 的 issue，別人看到 map 跟 reduce 的時候就大概知道你要做什麼事，不需要把所有 code 看完。
比如說這是一個很經典的 map 的 type signature，這裏有個前提是 lazy 跟 control。

foreach 大家應該知道，他跟 map reduce 的差別是如果 mapreduce 保持型別一致的話可以減少一些問題。可以使用 lo-dash 這個 library。

其實光是 reduce 就可以講很多東西。其實 map 就可以用 reduce 來做。transducer 有玩一些 reduce 的深入概念。如果你可以從 type 去思考 map 跟 reduce 的話，你可以做更多的事情。
map reduce 每個瀏覽器都有支援，不用擔心某個 i 開頭的瀏覽器會壞掉。

## functional programming 的重要概念

functional programming 的重要概念是 composition。檢查如果級數就回傳 true，使用 compose這個 functional 去組合這個函數。這樣 anonymouse compose 的結合是安全的。可重用性也很高，彈性表現在你可以用任何時間。

不幸的是在 js 中沒有那麼多語法可以用，就很遺憾，其實連接近也不怎麼接近。我們 jquery 已經用爛的 composition 還勉強可以用。
跟大家開始介紹 context 的概念，這個 array 表示法可以把 List 的東西表現成一個 type variable。如果從型別去思考 map 可以用在很多地方。getString 其實就是從 stdin 去欲取得一個字串，今天我們把 m 代換成 this 的話就是我們剛剛的例子。haskell當中就是一個很基本的例子。
怎麼 map 跟 map 的函式，fmap 就是 function map 的意思。

這邊要講 fmap 所有 context 都會去實作他，有 fmap 的 context 就叫 functor，最重要的是 function 對 context 一無所知，裡面不知道怎麼處理 list, httprequest..
相應的是 fmap 傳進去就自動幫你做打開 socket 這些事情。

我們可以保證我們使用的函示是很單純，不需要知道這些事情的。 functor 表現出來更有威力的是 context 是可以疊加的。如果我們有一連串的 http 在一個 list 裡面，他不需要知道怎麼處理 io怎麼處理 list。他的順序是很重要的。把 context 跟運算切開。functor 這東西是單純的，重點是怎麼去串。

## context 的威力

今天重點，Monad，大家如果上 stackoverflow 的話就看到很多人問。到處查就有一些可怕的圖，愈查愈多東西沒有一個是你懂的。剛剛這例子他 http 也許只是把 header body 改掉，我們想要他回傳的也許是200 401 403 500，但functor做不到。
如果我們今天 response 是給 404 用的，但 functor 做不到這一點。monad 比 functor 多的是可以做到這點。

他的型別是從 a 到 mb。函式可以控制要使用什麼 subcontext 去還給他，就是一個 unwrap 跟 wrap 的過程。假設我們今天在 http context 裡面，在下面這時候如果成功就回傳 200 給他，失敗就回傳 404  403。這時候 contex t就可以訂。a 到 mb 意義是說我變換的過程連 context 一起變換。

他保持著我們剛剛 map 一路走來優良傳統，不用知道裡面在做什麼東西，把它放到不同的 monad 就會達到不同的運算效果。所以 monad 最大的好處就是可以做各種有副作用的運算，再把它串起來，composition 合成的都是沒有副作用的函式，在 monad 是都可以串接起來。可以串有待 context 或是有副作用的函式。他們做的事情很像，只是他處理範圍變大了。
monad 可以防止意料之外的事情發生。所有都已經被 monad 包過了。

上面是沒有副作用的，沒有 context，下面 getString ...是有副作用的。如果用 bind 的方式副作用就會減少或受控管。

這例子就是 Gaia 的 notification，就是拉下來的地方。我們這種 js 程式有很多需求來自不同 context，比如說 dom 是一個 monad….全部都以 monad 的方式再結合 functional programming。
monad 就是要把所有 side effect 都包起來，但只能在 monad 裡面做運算，不讓外面拿到他的值，其實有例外，但是這裡不會講。
剛剛的例子，沒辦法說把裡面的值拿出來在外面做運算。就像是 promise，如果在 async 裡面拿出來的話，外面會有問題。像 promise 很早就帶這特色，jquery 他 context 可以說裡面所有操作都跟 dom 有關，lodash 的話就是跟 lodash 有關。如果你去點一個按鈕，他不會保證你已經點到了。這個做法是確保每一件事正確了才做下一步。

## Monad (functional programming)

JS code 跟 haskell code 其實很多一樣的地方。我們接下來的重點是真的 monad 的需求：lazye、async 控制，type 跟 monad load 的問題。左邊是我自己做的 monad，就是模擬 haskell 不會馬上回傳，這例子算是夠接近了。

這個文法可以做的更漂亮，這裡面做一些噁心的是，可以 monad 又有 async 的控管。反正我們在一個 weak type 的語言，他像 monad 他就是 monad。雖然他不是那麼好看。

結論是說，monad 它的好處是讓我們思考如何隔離 side effect，有人要做個更 general 的，讓大家方便做工作。不過當需求增長的時候會自然寫出 monad 概念的東西，沒時間介紹的是 context 可以互相疊加的問題，大家可以下載這簡報，看最後的附錄。

## Monad 實際使用範例

FRP 在講的是 react 跟 flux，大家可以聽。讓我們重新思考什麼叫 GUI program，使用者行為跟 data 可以被控管，但是 view 有重劃之類的問題就變得很恐怖。我們每次都 create 完整的 view 出來，至於效能的問題就是 react 要去煩惱。

在 haskell 不會把整顆樹全部做一遍，他只是去做一些差異比對，然後看起來回傳一顆全新的樹。Flux 又更有趣了，他算是很好思考 FRP 的架構。以 funcion programming 的基礎來去思考react 的架構。以前常聽到 haskell 不實用，但現在我們有 FRP 就可以拿來做很多事情。

composition 是很好但沒有漂亮語法，high order function 有一些方法可以讓你 code 更乾淨。Functor&monad 是控管副作用。最後如果覺得 funcational 在 GUI 不實用，可以試試看 react。
有人在 2009 年說 jquery 其實是 monad，但她其實不是。

## Q&A

### Q：前面提到盡量用 map reduce 取代 for loop，但是在一些文章看到 for loop 效能其實比較好？

A：我實際測試 map 跟 for loop 是一樣，reduce 通常是寫 code 使用糟糕的 pattern 讓效能變糟。如果很介意效能，應該考慮 asm.js，可以看看是不是有人做了蠢事讓效能變差。

## 關鍵字

* functional features/programming
* Haskell
* MVC framework
* hammer brainwash 
* lambda calculus
* OOP / FP
* closure
* function composition
* parity / managed side-effect / laziness
* ES6
* pro literals
* immutable data structure
* high-order function
* receive function as argument
* ijk tricks
* SLOC
* map and reduce
* transducer.js
* function composition
* lo-dash
* context & functor
* fmap(function map)
* functor
* encapsulate
* stockpiled
* stockpiling order
* SPDY
* monad
* subcontexts
* bind function
* unwrap/wrap
* trivial & reliable
* promises
* fluent interface
* functional reactive programming(FRP) 
* react & flux
* Yampa
* FP-flavor 
* transductor
