# 承诺 101: Javascript 承诺解释[代码片段]

> 原文：<https://medium.com/analytics-vidhya/promises-in-javascript-explained-examples-b15882a5714a?source=collection_archive---------2----------------------->

![](img/441fbf9e4110a98cef927b0b7b92c1c3.png)

照片由[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/woman-computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

…用实际例子解释 Javascript 承诺

# 先决条件

在开始本教程之前，您需要具备以下条件:

*   IDE(本教程使用 VS 代码)
*   对 Javascript ES6 的基本理解
*   Nodejs 已安装，您可以在这里获得最新版本[。](https://nodejs.org/en/download/)

# 介绍

使用来自其他资源的数据带来了处理异步操作的需求，即不按顺序运行的操作。在这篇文章中，我用简单的术语解释了 Javascript 中处理异步操作的一种方式，**承诺**。在这篇文章结束时，你将会理解 Javascript 在代码和简单英语方面的承诺。

# Javascript 承诺是什么？

根据官方文件:

> *Promise 对象表示异步操作的最终完成(或失败)及其结果值。*

***语法:***

```
new Promise((resolve, reject)=>{})
```

更简单地说，承诺是一个对象，它将根据承诺的状态返回特定的值。承诺可以有三种状态之一

*   `fulfilled`
*   `rejected`
*   `pending`

**状态 1:承诺兑现**

当承诺被解决时，它有一个状态`fulfilled`，这意味着承诺中没有出错，也没有错误。下面的代码片段将返回一个已履行的状态，因为承诺已被解决:

```
new Promise((resolve, reject)=>{
    resolve(console.log("Hello"))
})RESULT:
"Hello"
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: undefined
```

**状态 2:承诺被拒绝**

当错误发生时，承诺有一个`rejected`状态。在这种情况下，承诺被拒绝。下面的代码片段将返回拒绝状态，因为承诺被拒绝:

```
new Promise((resolve, reject)=>{
    reject(console.log("Oh no, an error"))
})RESULT:
"Oh no, an error"
[[PromiseState]]: "rejected"
[[PromiseResult]]: undefined
```

**状态 3:承诺待定**

当承诺本身既没有被解决也没有被拒绝时，承诺具有状态`pending`。下面的代码片段将返回状态`pending`,因为承诺既没有被解决也没有被拒绝:

```
new Promise((resolve, reject)=>{})RESULT:
[[PromiseState]]: "pending"
[[PromiseResult]]: undefined
```

🛑 **休息时间:**

***PS:*** 如果你想在今年开始你的科技职业生涯，你可以下载[这份科技领域 14 种职业的清单](https://t.co/QhrUnNLVY0?amp=1)以及每种职业你应该首先学习的编程语言。

🛑

# 履行承诺

在处理承诺时，您可能会遇到如下的一些方法和函数

*   `then`方法:该方法从解析的承诺中获取返回值，并对它们执行一些操作。`then`方法通常与承诺联系在一起。请参见下面的示例:

```
function getDataFromDatabase(){
    return new Promise((resolve, reject)=>{
        // promise resolves with no errors
    })
}getDataFromDatabase().then((data)=>{
    // data is the returned value from the getFromDatabase function
    console.log(data)
})
```

当使用`then`关键词时，用简单的英语像这样想会很有帮助:

`First of all, get data from the database with the getDataFromDatabase() function, and then console.log all the data gotten from the database`

*   `trycatch`处理程序:trycatch 处理程序非常常用于处理承诺。因为您的承诺可能有一个`rejected`状态，所以添加一个捕获该错误的方法会很有帮助，这正是 trycatch 处理程序的工作方式。请参见下面的示例:

```
function getDataFromDatabase(){
    return new Promise((resolve, reject)=>{
       try {
           // if Promise resolves, return data from the resolve method
           resolve()
       } catch (error) {
           // if the promise rejects, return error data from the reject method
           reject(console.log(error.message))
       }
    })
}getDataFromDatabase().then((data)=>{
    // data is the returned value from the getFromDatabase function
    console.log(data)
})
```

当使用`trycatch`处理程序时，用简单的英语像这样想象会很有帮助

`In getting data from the database with the getDataFromDatabase() function, try this code inside this block first, if I successfully get the data, return data from the resolve() function. If I run into some error, return data from the reject function. Then run the function inside the THEN method`

# 代码示例

在本例中，我们将尝试并模拟对外部端点进行 API 调用。这个想法是模拟在进行 API 调用时获取数据的延迟。以下是我们需要考虑的步骤:

1.  创建数据存储
2.  创建一个函数来获取数据(想象一下 Javascript 中的 fetch 函数)
3.  当我们最终获得新数据时，向数据存储中添加新数据
4.  将结果记录到控制台

# 步骤 1:创建数据存储

在这个例子中，我们将使用一个简单的数组来存储项目，我们在这里模拟数据库。

```
let names = ['Kim Seon Ho', 'Park Seo Joon', 'Park Shin Hye', 'Shin Hye Sun']
```

# 步骤 2:创建一个函数来获取数据

这里，我们将模拟使用 setTimeout()函数从数据库中获取数据时的延迟。这就是我们将使用 Promise 的地方，因为我们希望它在我们的理论项目中作为异步函数运行。

```
function getData(dataItems) {
    return new Promise((resolve, reject) => {
        try {
            setTimeout(() => {
                resolve(dataItems)
            }, 3000);
        }
        catch (error) {
            reject(console.log(error))
        }
    })
}
```

**发生了什么:**在这段代码中，我们确保`getData()`函数返回一个 Promise，这样我们就可以使用 Promise 提供的链接方法来更改/编辑从数据存储中返回的数据。

# 第 3 步:当我们得到数据时转换它

```
getData(names).then(data=>{
    let newName = 'Han Yoo Joo'
    data.push(newName)
    return data
}).then(finalData=>{
    console.log(finalData)
})
```

**发生了什么:**这里，我们使用`getData()`函数获取数据。`getData()`函数返回我们存储在`then`方法的`data`参数中的信息。记住，`then`方法从 Promise 中解析的方法获取返回的数据，并能够在其上执行任务。正是这个`data`，我们随后使用`Array.push()`方法推送一个新名称。

# 步骤 4:将结果记录到控制台

最后一个`then`方法是不必要的，因为我们可以将`console.log()`方法添加到第一个`then`方法中，但是，我添加它是为了向您展示在 Javascript 中处理承诺时可以使用多少链接。在最后一个`then`方法中，我们获取从第一个`then`方法返回的数据，并将其记录到控制台。

# 结果

```
[
  'Kim Seon Ho',
  'Park Seo Joon',
  'Park Shin Hye',
  'Shin Hye Sun',
  'Han Yoo Joo'
]
```

# 结论

Javascript 承诺是在代码中执行异步功能的好方法。比起面对每个 Javascript 程序员的噩梦，回调地狱，这是一个更好的选择。您可以在这个 Github 资源库中找到完整的代码示例: [**代码示例**](https://github.com/Vanneka/asynchronous-javascript-examples)

感谢阅读，再见。

💜💜再见！瓦妮莎·奥

💜如果你觉得这篇文章有用，记得分享并帮助我发展我的社区。💜

如果你想在今年开始你的科技职业生涯，你可以下载科技行业的 14 个职业清单，看看你应该首先学习的编程语言。