# 如何从异步调用返回响应？

> 原文：<https://medium.com/analytics-vidhya/how-to-return-the-response-from-an-asynchronous-call-2ef494308423?source=collection_archive---------0----------------------->

![](img/78c1faca539d508b1ed9eddef4a0ee5a.png)

[https://www . teachhub . com/professional-development/2020/11/benefits-of-synchronous-vs-asynchronous-online-instruction/](https://www.teachhub.com/professional-development/2020/11/benefits-of-synchronous-vs-asynchronous-online-instruction/)

[Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)) 中的 **A** 代表 [**异步**](https://www.merriam-webster.com/dictionary/asynchronous) 。这意味着发送请求(或者更确切地说是接收响应)被从正常的执行流程中去掉了。在您的示例中，`$.ajax`立即返回，下一条语句`return result;`在您作为`success`回调传递的函数被调用之前就被执行了。

这里有一个类比，它有望使同步流和异步流之间的区别更加清晰:

# 同步的

想象一下，你打电话给一个朋友，请他为你查找一些东西。虽然这可能需要一段时间，但你在电话旁等待，凝视着天空，直到你的朋友给你想要的答案。

当您进行包含“正常”代码的函数调用时，也会发生同样的情况:

```
function findItem() {
    var item;
    while(item_not_found) {
        // search
    }
    return item;
}var item = findItem();// Do something with item
doSomethingElse();
```

即使`findItem`可能需要很长时间来执行，任何在`var item = findItem();`之后的代码都必须等待*直到函数返回结果。*

# 异步的

你又因为同样的原因打电话给你的朋友。但是这次你告诉他你有急事，他应该用你的手机给你回电话。你挂断电话，离开家，做你计划要做的任何事情。一旦你的朋友给你回电话，你正在处理他给你的信息。

这正是当您执行 Ajax 请求时所发生的情况。

```
findItem(function(item) {
    // Do something with the item
});
doSomethingElse();
```

不再等待响应，而是立即继续执行，并执行 Ajax 调用后的语句。为了最终得到响应，您提供了一个一旦收到响应就调用的函数，一个*回调*(注意到什么了吗？*回电*？).该调用之后的任何语句都在回调被调用之前执行。

# 解决方案

## 如果您没有在代码中使用 jQuery，这个答案适合您

您的代码应该是这样的:

```
function foo() {
    var httpRequest = new XMLHttpRequest();
    httpRequest.open('GET', "/echo/json");
    httpRequest.send();
    return httpRequest.responseText;
}var result = foo(); // Always ends up being 'undefined'
```

拥抱 JavaScript 的异步本质！虽然某些异步操作提供了同步副本(Ajax 也是)，但通常不鼓励使用它们，尤其是在浏览器环境中。

你问为什么不好？

JavaScript 在浏览器的 UI 线程中运行，任何长时间运行的进程都会锁定 UI，使其无响应。此外，JavaScript 的执行时间有上限，浏览器会询问用户是否继续执行。

所有这些都是非常糟糕的用户体验。用户将无法判断一切是否正常。此外，对于连接速度较慢的用户，效果会更差。

在下文中，我们将看看三种不同的解决方案，它们都是建立在彼此之上的:

*   **承诺与** `**async/await**` (ES2017+，如果使用 transpiler 或 regenerator，则在较旧的浏览器中可用)
*   **回调**(流行于节点)
*   **Promises with**`**then()**`(es 2015+，如果你使用众多 promise 库中的一个，可以在旧浏览器中使用)

**目前的浏览器都有这三个，还有 node 7+。**

# ES2017+:与`[async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)`的承诺

2017 年发布的 ECMAScript 版本引入了对异步函数的*语法级支持*。在`async`和`await`的帮助下，你可以用“同步风格”写异步。代码仍然是异步的，但是更容易阅读/理解。

`async/await`建立在承诺之上:一个`async`函数总是返回一个承诺。`await`“解包”一个承诺，如果该承诺被拒绝，则产生解决该承诺的值或抛出一个错误。

**重要提示:**你只能在`async`函数中使用`await`。现在，顶级的`await`还不被支持，所以你可能不得不创建一个 async life([立即调用函数表达式](https://en.wikipedia.org/wiki/Immediately_invoked_function_expression))来启动一个`async`上下文。

你可以在 MDN 上阅读更多关于`[async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)`和`[await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)`的内容。

下面是一个建立在上述延迟之上的示例:

```
// Using 'superagent' which will return a promise.
var superagent = require('superagent')// This is isn't declared as `async` because it already returns a promise
function delay() {
  // `delay` returns a promise
  return new Promise(function(resolve, reject) {
    // Only `delay` is able to resolve or reject the promise
    setTimeout(function() {
      resolve(42); // After 3 seconds, resolve the promise with value 42
    }, 3000);
  });
} async function getAllBooks() {
  try {
    // GET a list of book IDs of the current user
    var bookIDs = await superagent.get('/user/books');
    // wait for 3 seconds (just for the sake of this example)
    await delay();
    // GET information about each book
    return await superagent.get('/books/ids='+JSON.stringify(bookIDs));
  } catch(error) {
    // If any of the awaited promises was rejected, this catch block
    // would catch the rejection reason
    return null;
  }
}// Start an IIFE to use `await` at the top level
(async function(){
  let books = await getAllBooks();
  console.log(books);
})();
```

当前[浏览器](https://kangax.github.io/compat-table/es2016plus/#test-async_functions)和[节点](http://node.green/#ES2017-features-async-functions)版本支持`async/await`。你也可以在[再生器](https://github.com/facebook/regenerator)(或者使用再生器的工具，比如 [Babel](https://babeljs.io/) )的帮助下，通过将你的代码转换成 ES5 来支持旧的环境。

# 让函数接受回调

回调是将函数 1 传递给函数 2。函数 2 可以随时调用函数 1。在异步流程的上下文中，每当异步流程完成时，都会调用回调。通常，结果被传递给回调。

在问题的例子中，您可以让`foo`接受一个回调，并将其用作`success`回调。所以这个

```
var result = foo();
// Code that depends on 'result'
```

成为

```
foo(function(result) {
    // Code that depends on 'result'
});
```

这里我们定义了“内联”函数，但是您可以传递任何函数引用:

```
function myCallback(result) {
    // Code that depends on 'result'
}foo(myCallback);
```

`foo`本身定义如下:

```
function foo(callback) {
    $.ajax({
        // ...
        success: callback
    });
}
```

`callback`将引用我们传递给`foo`的函数，当我们调用它时，我们将它传递给`success`。也就是说，一旦 Ajax 请求成功，`$.ajax`将调用`callback`并将响应传递给回调(可以用`result`引用，因为这是我们定义回调的方式)。

您也可以在将响应传递给回调之前对其进行处理:

```
function foo(callback) {
    $.ajax({
        // ...
        success: function(response) {
            // For example, filter the response
            callback(filtered_response);
        }
    });
}
```

使用回调编写代码比看起来容易。毕竟，浏览器中的 JavaScript 是高度事件驱动的(DOM 事件)。接收 Ajax 响应只不过是一个事件。当您必须使用第三方代码时，可能会出现困难，但是大多数问题都可以通过考虑应用程序流程来解决。

# ES2015+:承诺与 [then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[Promise API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 是 ECMAScript 6 (ES2015)的新特性，但它已经有很好的[浏览器支持](http://caniuse.com/#feat=promises)。还有许多库实现了标准的 Promises API，并提供了额外的方法来简化异步函数的使用和组合(例如 [bluebird](https://github.com/petkaantonov/bluebird) )。

承诺是未来价值的容器。当 promise 收到值时(它是*解决的*)或者当它被取消时(*拒绝的*),它通知所有想要访问这个值的“监听器”。

与普通回调相比，它的优势在于允许你将代码解耦，并且更容易编写。

下面是一个使用承诺的例子:

```
function delay() {
  // `delay` returns a promise
  return new Promise(function(resolve, reject) {
    // Only `delay` is able to resolve or reject the promise
    setTimeout(function() {
      resolve(42); // After 3 seconds, resolve the promise with value 42
    }, 3000);
  });
}delay()
  .then(function(v) { // `delay` returns a promise
    console.log(v); // Log the value once it is resolved
  })
  .catch(function(v) {
    // Or do something else if it is rejected
    // (it would not happen in this example, since `reject` is not called).
  });
```

应用到我们的 Ajax 调用中，我们可以使用这样的承诺:

```
function ajax(url) {
  return new Promise(function(resolve, reject) {
    var xhr = new XMLHttpRequest();
    xhr.onload = function() {
      resolve(this.responseText);
    };
    xhr.onerror = reject;
    xhr.open('GET', url);
    xhr.send();
  });
}ajax("/echo/json")
  .then(function(result) {
    // Code depending on result
  })
  .catch(function() {
    // An error occurred
  });
```

描述 promise 提供的所有优点超出了本答案的范围，但是如果您编写新代码，您应该认真考虑它们。它们为您的代码提供了很好的抽象和分离。

关于承诺的更多信息: [HTML5 摇滚——JavaScript 承诺](http://www.html5rocks.com/en/tutorials/es6/promises/)

# 附注:jQuery 的延迟对象

[延迟对象](https://stackoverflow.com/questions/4866721/what-are-deferred-objects)是 jQuery 对 promises 的自定义实现(Promise API 标准化之前)。它们的行为几乎像是承诺，但公开的 API 略有不同。

jQuery 的每个 Ajax 方法都已经返回了一个“延迟对象”(实际上是一个延迟对象的承诺)，您可以从函数中返回它:

```
function ajax() {
    return $.ajax(...);
}ajax().done(function(result) {
    // Code depending on result
}).fail(function() {
    // An error occurred
});
```

# 旁注:承诺抓住了你

请记住，承诺和延期对象只是未来价值的容器，它们不是价值本身。例如，假设您有以下内容:

```
function checkPassword() {
    return $.ajax({
        url: '/password',
        data: {
            username: $('#username').val(),
            password: $('#password').val()
        },
        type: 'POST',
        dataType: 'json'
    });
}if (checkPassword()) {
    // Tell the user they're logged in
}
```

这段代码误解了上述异步问题。具体来说，`$.ajax()`在检查服务器上的“/password”页面时不会冻结代码——它向服务器发送一个请求，并在等待时立即返回一个 jQuery Ajax 延迟对象，而不是来自服务器的响应。这意味着`if`语句将总是获取这个延迟的对象，将其视为`true`，并像用户登录一样继续。不太好。

但是解决方法很简单:

```
checkPassword()
.done(function(r) {
    if (r) {
        // Tell the user they're logged in
    } else {
        // Tell the user their password was bad
    }
})
.fail(function(x) {
    // Tell the user something bad happened
});
```

# 不推荐:同步“Ajax”调用

正如我提到的，有些(！)异步操作有同步操作。我不提倡使用它们，但是为了完整起见，下面是您应该如何执行同步调用:

# 不使用 jQuery

如果你直接使用一个`[XMLHttpRequest](https://xhr.spec.whatwg.org/)`对象，将`false`作为第三个参数传递给`[.open](https://xhr.spec.whatwg.org/#the-open()-method)`。

# jQuery

如果使用 [jQuery](http://api.jquery.com/jQuery.ajax/) ，可以将`async`选项设置为`false`。请注意，从 jQuery 1.8 开始，此选项*已被弃用*。然后，您可以继续使用`success`回调或者访问 [jqXHR 对象](http://api.jquery.com/jQuery.ajax/#jqXHR)的`responseText`属性:

```
function foo() {
    var jqXHR = $.ajax({
        //...
        async: false
    });
    return jqXHR.responseText;
}
```

如果使用任何其他 jQuery Ajax 方法，比如`$.get`、`$.getJSON`等。，您必须将其更改为`$.ajax`(因为您只能将配置参数传递给`$.ajax`)。

**抬头！**不可能发出同步 [JSONP](https://stackoverflow.com/questions/2067472/please-explain-jsonp) 请求。JSONP 本质上总是异步的(这是不考虑这个选项的又一个原因)。