# Koa - Asynchronous Decorators as Middleware
## Speaker: Jonathan


我是John，我要跟大家講koa，如我大家不知道呢，事實上可以講說這是下一個版本的express，在node基本上就是callback，如果你在用node callback的時候，你必須要把你的error hand over，我們好多的callback有好多error和bug，solution就是你要有一個code是沒有bug，可是這很難學很難做，那麼我們在講callback你可以用try catch，在這例子我們可以看到在callback這個地方，我們不知道這個callback可是你試著想要走try catch是行不通的，在call process的時候你的app會壞掉，所以我們不要他發生這個事情，如果你用callback太多次，你會跟pyramid，基本上我們會有很多callback不好的function，這樣也不好玩，同時也有條件的，我們講callback，基本上if 跟 callback在一起是很難的，所以我們這邊有一些callback，我們要做refactoring，這是非常難做的，同時要花很多知識才能學起來，接下來我們還有control flow library還很多async的，這邊還有一些domain，但這邊是非常複雜的，我們不要去用，這裡有些library不知道怎做的。

最後的解決方案是promise，也就是async，所以我來說明一下promise，我有這個async，要直接把object pass過去，promise好處在於大部分的瀏覽器有很多的promise，那在node也有promise，但不穩定，我們這版本，想要再兩個禮拜知道他是什麼，在javascript的概念上promse是最難學習的，那我們的es7明年會出來，我們終於可以寫async像在寫sync，所以這邊可能對大家很困惑，這邊有個async function，你會知道這有個promise，他們不會很快的去做什麼事情。

可是在做promise是可以做，你可以去等他的value出來，好像是script code一樣。如果是來自於C#，我想沒有人用這個吧？有很多類似的IBS在很多的語言上，功能也有一個，這是code channel 的 basic learning，就像是sync一樣，好的是事實上你可以用try catch，不用每次都error hand over，catch就可以了，在async這邊我們講到不一定要用promise，創造promise，非常困惑，用起來也是，不一定要用promise api。

所以我們快要講到這邊，就是generator的地方，我們在講koa的技術，事實上是xxx寫的，那我也可以看這個地方，如果你看到copy fool基本上是類似的，但是他不用async function，你有一個generator function，這個code wrap不用await，而是用yield的，這就是我們講promise，必須要用同樣的code，你可以不用promise，看起來像是sync，你當然也可以去catch你所有的error，所以這地方大家可以看到，這個地方我TJ其他人，一個中國人我不知道他的名字，用這個code，用同樣的module來做express，大家可以看到這是low level的module，維持的是express的team，大家知不知道kao，他這個callback跟middleware，generators跟middleware之間，我們看到他的樣子。

就是node api把全部包起來，變成一個promise，基本上我們讀一個file，他就變成一個return promise，我就把它yield然後把它變成一個response value，在這邊我們跟express來比或跟其他的framework來比這很簡單，跟其他framework不一樣，我們可以try catch，如果你來自於比方說ruby或者其他地方的話，你可以就用try catch，所以這有個例子，我們講說，body是一個file，但這file因為某個原因他不存在，也許他hover error，我們可以自己save body，他這邊try catch，所以我們在kao的目標我們不需要這麼多kof，最後你不需要用這個code，所以這個例子就是到底未來我們在講koa的地方，我們之後再來講怎麼做

為什麼我們要去等，因為沒有更多callback的需要，我們不要再用manual error handling，你終於可以用catch，不需要第三方library，所以大家在講說到底library最好的control是什麼？這是我認為這個地方希望這部份不要再繼續爭吵了。到這個async的路徑是什麼，我現在要做的大家都可以用library用promise，很難，不是大家都喜歡promise，我不喜歡promise，但我還是會去應用，，因為這是另外一個版本的async code，我們就不要用它的方式，所以這個library的話你就去用promise，所以這就是part1，也就是koa的目標，我們接下來看如何做koa。

我不知道大家知不知道express，express就是一大堆的callback，基本上他的方式就是第一個你的middleware，然後error handler。就像我之前講的，這個callback你必須要每個error都去handle，你有async function你就要去處理middleware，這就是我去處理error的方式，要不然你就這樣做。有很多express使用上與callback的限制，首先你不能去改變它的response，否如說你要去暫停一個middleware類似的反應很難做到，比方說，user loggin很難做，特別說是限制的反應，還有middleware compression，session等等，如果你是新人，很難去學習。

另外我們可以做的事情在這個地方，我給他一個response，你有一個session，我們要去看database，現在我們在express你就把它存起來，然後他就error，如果你沒有error handling，如果發生事情比方說你把它連到stop，如果沒有error handling，因為他就是沒有壞掉，送一個message給你，你現在是login error，很多這種事情發生，很多人問我hapi，kao跟express是什麼關係，還有其他framework他們差別在哪裡，主要差別是hapiJS是個plugin，你不是做logic，你是做plugin，然後你用很多options，hapiJS有很多options，overhead很多，很難support，你自己創造的plugin他只會在hapi面可以使用，domain也是很複雜的。

為麼麼有hapiJS，他們很喜歡，你可以做很多事情，express很多限制，他們主要的目標是把express的問題解決，koa也是一樣，express做不到可以用koa，但是koa的方式跟hapi不同。

接下來我們講express，為什麼叫做express，希望能寫自己的framework不用依賴一framework，譬如說hapiJS要去依賴一個framework去支持你要做的事情，express不要依賴這麼多，但是做很多事情很難，尤其是你要做很複雜的apps，所以你不想要去依賴一個api或是等到framework去幫你開發出api來達到你要做的事情，所以kao後面跟express不一樣，你就callback，你就用decorater，這也是很難學，之前有談到這非常複雜，我有workshop，想要就去讀，他就是一個wrap 別的function的一個function，middleware就只是一個function，他不需要plugin到其他的地方，這邊就是一個koa middleware的樣子，每一個middle ware他都generator一個function，然後你有一個maps，沒有request parameter，不像express，因為他都接在上面。

你可能會困惑，javascript不是每個人都知道這裡面的這個，所發生的事情是，middleware去wrap around另一個middleware，每一個middleware又去後續的middleware包起來，所以你的下一個middleware function 叫做after 這個middleware，所以可以在kao裡找到response，你也可以用express但比較難。

基本上我們叫做unix sync，你要再做下一個事情就叫做unix，我們叫下游downstream，你在叫下一個middleware的時候，我們的upstream，也就是我們講的這個middleware，我們怎麼去decorate這個後續的middleware的方式。
每一個middle都是一個app，我在寫koa他有自己的route，每個route不管是app或是app的部份，每個app會做同樣的page，所以基本上那麼這樣你就是可以寫minify，compress。

這就是express跟koa不一樣的地方，express就是，path control ware還有middleware，koa會包起來。

express是error handling，發生就callback，kao用middleware包起來，這是error handling的一個樣子，你用try catch，如果app break這邊就會有response，所以這個app收到error就catch然後送一個error response。所以最大的問題node stream，stream基本上他是在catch error很難去處理，因為很多方式處理error，kao優點就是他會去處理stream，他看到error出現會log it跟送response，node很多streaming leaks，kao你可以看library，基本上他會看到這個topstream，然後就加入每一種node stream。

這些是各種不同的error handling，express會catch然後handle他，他不是很難做客製化，假設你有一個app他有很多mutiple apps，每一個app要做error handle他沒辦法，hapi他會幫你做，在koa你要怎麼做都可以，因為koa你在wrapmiddleware，你寫apps是不一樣的，你要有bussiness logic，express哪裡去放middleware，那你要怎麼做。

，在koa就放在邏輯上，我要bussiness邏輯能夠compression，所以你做的事情是在上面再加一個middleware，所以我想這個地方他好像是個聖誕樹，裝飾他，給他加一些features。我很討厭這個東西，在express每次有一個middleware都要煩惱到底要放在哪裡，很難去debug，在koa，可以很簡單思考，為何要用koa，

第一：Error handling，在express要做很多很討厭，有時候做錯就crash，在kao不用這樣做，只要不用stream，不用擔心可能的crash，

第二：非同步控制（Async/await -style control flow），他的功能不管，

第三：我們講他的decorate pattern，最後他有所謂的upstream，downstream，這是hapi沒有支援的。

對我來講koa比較容易了解好用，譬如說hapi要讀100頁的文件，koa也是不同的。所以如果要做什麼事情，要看一下他的module，如果你是新人這很難，你不知道他怎麼用，或是如何跟其他一起使用，如果你是新人的話，我們看到他是可以用同樣的module。如果是公司不想用一些不stable，可能會當掉，kao很纖細，他支援不多，hapi可以支援很多，hapi的目標很多人寫同樣的app，hapi試著支援所有東西，你不需要做很module，很多東西，koa必須要用很多的module結合起來，如果你是個團隊你就很難把每一個人都在同一個board上，就看情況吧，要用hapi或是express。

如果是新的人建議用express，因為比較簡單，文件比較多，koa文件比較少，所以新人就用express，koa好處是json，我想做的事情是在team中，model很重要，這就是我們的business。那麼refactoring的話用promise，如果你有很多business logic或很多async code，promise有幫忙這部份，refactoring我建議用koa，因為比較容易用。koa是好的，如果有個複雜的，如果要做一個比較soft的東西建議用kao，kao最大問題是學習曲線高，你要知道generator，我等下會來解釋，蠻混淆的，花一兩個禮拜才了解generator，一堂課是不能理解generator的，很模組化，你必須match很多moduel在學得好，好處是你可以客製化很多。koa是low level的，要做很多http，要解決很多的問題，express跟hapi可以幫你handle這些事，koa無法處理具體的file。

## Q&A

### Q1:看起來express跟koa有很多不同的原則，有非同步的問題，koa想做非同步當做procedule code，我想知道是何種decision讓koa變成這樣？

A1: 另外一個問題，他是conditionl，我不要show，因為他很難去理解，他會把分成很多 Function 很難去維護

### Q2:你怎麼認為，express strongloop 怎麼做？

A2: tj有把這個express賣給strongloop，有些公司為了品牌在競爭所以買express，tj預期會維護但他們並沒有，目前維護，確定每一個都是對的，現在她在維護，之前有些drama，他解決了，也有聘請人做文件，那麼他個人我認為這是個不致知舉，我不喜歡面對drama所以我不管這些事情

### Q3:如果新開發者，他們是要從koa或是express來用哪個呢？

A3:我建議express比較簡單，如果是新人，我建議hapi因為他們文件很多，express需要很多邏輯，如果你知道js，es6是新東西，koa是先進的，非常複雜，一旦知道做app就很容易了

## 關鍵字

* Towards ES7 Async/Await with Koa
* express
* callback errors
* try catch
* pyramid
* refactoring
* async
* ES6 promises
* async/await
* generators
* middleware
* 3rd party libraries
* ES7 Async/Await
* user logging
* session
* error handling
* HapiJS
* Decorator Pattern
* upstream/downstream
* minify
* compress
* Decorators
* philosophies
* --harmony-generators
* JSON
* refactor
* 
* control flip point
* RFC