# 快速理解 JavaScript 中的承诺

> 原文：<https://medium.com/analytics-vidhya/quick-understanding-of-promise-in-javascript-b0444fcd44b1?source=collection_archive---------22----------------------->

就像我们向某人承诺某件特定的事情一样，在 JavaScript 中，我们也可以向用户承诺某件事情。

例如:“我保证我会为你从服务器上获取数据”

现在在这里，一个承诺可以实现，也可以不实现。正确吗？因此，在这里我们可以规定，如果一个承诺实现了，那么感到感激，或者如果没有实现，道歉。

好了，现在概念清楚了。让我们看看如果我们编码会是什么样子。

> function promiser(){
> 返回新的 Promise(function(resolve，reject){
> const error = true；//某 HttpRequest 到服务器
> if(！error){
> resolve()；//当承诺者得到数据时，承诺被保持/解析
> console . log(" Promiser:Promise fulfilled ")；
> } else {
> reject()；//当承诺者没有得到数据时，承诺被破坏/拒绝。
> console.log("Promiser:承诺未解决")；
> }
> })
> }
> 
> promiser()；//呼叫承诺者

现在让我们明白。创建了一个名为“promiser”的函数，它返回一个 Promise 对象。promise 构造函数接受一个有两个函数作为参数的函数。

第一个是 resolve()，第二个是 reject()。在这里，我们创建了一个错误变量来代替任何 HTTP 调用(为了便于理解)，如果没有错误，我们调用 resolve()并打印“Promise is fulfilled”，但是如果有错误，我们调用 reject()并打印“Promise is not fulfilled”。

现在，让我们心怀感激，因为我们信守了对用户的承诺；当我们不能做到时，让我们道歉。好吗？

> function promiser(){
> 返回新的 Promise(function(resolve，reject){
> const error = true；//某 HttpRequest 到服务器
> if(！error){
> resolve()；//当承诺者得到数据时，承诺被保持/解析
> console . log(" Promiser:Promise fulfilled ")；
> else {
> reject()；//当承诺者没有得到数据时，承诺被破坏/拒绝。
> console.log("承诺者:承诺未解决")；
> }
> })
> }
> 
> 承诺者()。然后(function(){ console.log("感恩我！") }).catch(function(){ console。log("我道歉。") });

现在我们已经为同一个调用使用了函数链。在这里我们调用 then()和 catch()。

当我们履行/解决我们的承诺时调用 then()，当我们不能履行或拒绝我们的承诺时调用 catch()。这些功能将在得到*承诺者*功能的响应后执行。这取决于调用的是 resolve()还是 reject()。

如果我们的承诺没有兑现，我们应该给出一个合理的理由。对吗？

为此，我们在 resolve 或 reject()函数调用中传递参数。让我们再看一遍这段代码。

> function promiser(){
> 返回新的 Promise(function(resolve，reject){
> const error = true；//某 HttpRequest 到服务器
> if(！error){
> resolve("服务器对我很好")；//当承诺者得到数据时，承诺被保持/解析
> console . log(" Promiser:Promise fulfilled ")；
> }else{
> reject("服务器对我没那么好")；//当承诺者没有得到数据时，承诺被破坏/拒绝。
> console.log("Promiser:承诺未解决")；
> }
> })
> }
> 
> 承诺者()。然后(function(msg){ console.log("感恩我！"+ msg) })。catch(函数(错误){ console。log("我道歉。"+错误)})；

在这里，每当调用 resolve()和 reject()时，我们都会在日志中获得相应消息。“如果没有错误，我感激不尽！服务器对我很好”将被打印到控制台。如果有错误，“我道歉。服务器对我不友好。”将被打印到控制台。

这是对 JavaScript 中承诺的一个非常基本的理解。你可以尝试用承诺来实现不同的事情。

感谢阅读这篇文章。希望你从这篇文章中有所收获。如果是这样，我不介意鼓掌😄