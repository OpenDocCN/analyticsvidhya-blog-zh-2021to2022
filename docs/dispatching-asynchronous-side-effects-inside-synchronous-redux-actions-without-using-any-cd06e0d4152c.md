# 在不使用任何中间件的情况下，在同步 Redux 动作中分派异步副作用——React。

> 原文：<https://medium.com/analytics-vidhya/dispatching-asynchronous-side-effects-inside-synchronous-redux-actions-without-using-any-cd06e0d4152c?source=collection_archive---------4----------------------->

![](img/ff696110cae8f2c0ed7abc4a07418123.png)

照片由 [Unsplash](https://unsplash.com/s/photos/timer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

异步的东西总是令程序员头疼。`setTimeout(), callbacks, promises, aync await,` 等。—我们有如此多的工具，但有时我们仍会迷失在编程的同步范例中。在过去的一年里，我一直在大规模使用 **React** 。开发高功能仪表板、UI 优化、控制数据流以及处理大量 API 集成是我日常工作的一部分。*最近我遇到了一个情况，当用户重置过滤器时，应用程序应该从前端进行两次 API 调用，其中一个 queryparams 来自于从另一个获取的数据。*在这篇博客中，我将解释我是如何通过使同步 **Redux** 动作异步来实现这一点的。所以让我们开始吧。

我在这里假设您对 React 和 Redux 的工作原理有一个基本的了解。根据 Redux docs —

> Redux 存储本身对异步逻辑一无所知。它只知道如何同步调度动作，通过调用根 reducer 函数来更新状态，并通知 UI 发生了变化。任何异步都必须发生在商店之外。

我用 JavaScript 工具武装自己，只做了一个简单的小把戏，就非常容易地绕过了上面的协议，达到了我的目的。

```
export const resetFilter = () => async (dispatch, getState) => {   
    dispatch({ type: RESET_FILTER }); await dispatch(callAPI_1());
    const {
        mainComponent: { api1_data },
    } = getState(); await dispatch(
        callAPI_2(api1_data.param1, api1_data.param2)
    );
};
```

这几乎是我所做的一切。`callAPI_1`和`callAPI_2`是已经定义的 redux 动作，我在这里调用每个 API。当用户重置过滤器时，resetFilter 操作被触发，应用程序需要用新的参数调用 API。这里的技巧部分是`callAPI_2` 的 **queryparams** 来自`callAPI_1`的**新状态**，由于 Redux 动作是同步的，整个系统在没有 **async-await** 的情况下失败。通过使用 async-await，我们等待第一个 API 调用完成，在触发它之前获取它的数据并将其传递给第二个 API 调用的 queryparams。像这样再次 aync-await 拯救世界！

对于复杂的情况，您可能需要使用一些 Redux 中间件，如 [redux-thunk](https://github.com/reduxjs/redux-thunk#redux-thunk) 来使动作异步。但是对于像这样的小用例，我的方法就足够了。这也可以使用 setTimeout()、回调或承诺来完成，但我更喜欢 async-await ( *es6 yaay！！*)。

可能有这样一个用例，你正在进行相同类型的 API 调用，但是你是作为一个副作用[而不是动作来做的。逻辑是相同的，但是我们需要像这样从一个 useEffect 钩子中调用`callAPI_1`和`callAPI_2`动作:](https://reactjs.org/docs/hooks-effect.html)

```
useEffect(() => {
    async function getData() {
        await callAPI_1();
        await callAPI_2(api1_data.param1, api1_data.param2);
    }
    getData();
}, [callAPI_1]);
```

记住，`promises`和`useEffect( async() => …`在 React 中不被支持，但是你可以在一个效果中调用异步函数。还有一个你应该永远记住的 React hooks [规则](https://reactjs.org/docs/hooks-rules.html):

> **不要在循环、条件或嵌套函数中调用钩子。相反，在任何早期返回之前，总是在 React 函数的顶层使用钩子。通过遵循这条规则，您可以确保每次组件呈现时都以相同的顺序调用钩子。这使得 React 能够正确地保存多个`useState`和`useEffect`调用之间的钩子状态。**

***附加提示*** *:总会有边界但是作为程序员的你总是可以使用已经可用的工具来创造魔法。*

感谢您通读。我将非常感谢您的评论和建议。如果你有其他的优化技术，请在评论中告诉我。

如果你觉得它有用，请给我一个掌声，并与你的开发伙伴分享。 ***不要硬编码，硬编码！***

# 苏巴扬·戈什

[LinkedIn](https://www.linkedin.com/in/realsubhayan/)|[Twitter](https://twitter.com/realsubhayan)|[insta gram](https://www.instagram.com/realsubhayan/)