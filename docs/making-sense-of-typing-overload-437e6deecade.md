# 理解打字的意义。超载

> 原文：<https://medium.com/analytics-vidhya/making-sense-of-typing-overload-437e6deecade?source=collection_archive---------5----------------------->

让你的回报类型更精确！

![](img/0fe6569196c5e537324e80655e90f087.png)

[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 如果不使用`typing.overload`会发生什么

假设我们有一个函数，它有一个布尔参数`inplace`，并且它的返回类型依赖于`inplace`的值。如果是`True`，则返回`None`，否则返回一个整数:

通过检查函数，我们可以看到我们期望第 17 行的返回类型是`None`，而第 18 行和第 19 行的返回类型是`Cat`。然而，`mypy`在函数内部没有达到峰值，因此认为它们都是`Optional[Cat]`。实际上，运行上面的剪贴，我们得到:

```
main.py:17: note: Revealed type is 'Union[main.Cat, None]'
main.py:18: note: Revealed type is 'Union[main.Cat, None]'
main.py:19: note: Revealed type is 'Union[main.Cat, None]'
main.py:20: note: Revealed type is 'Union[main.Cat, None]'
```

为了使第 17 行和第 18 行的返回类型更加精确，我们需要`overload`。

# 如果使用重载会发生什么

它返回

```
main.py:31: note: Revealed type is 'None'
main.py:32: note: Revealed type is 'main.Cat'
main.py:33: note: Revealed type is 'main.Cat'
main.py:34: note: Revealed type is 'Union[main.Cat, None]'
```

请注意我们是如何需要重载的:

*   文字[假]，默认
*   字面[真实]
*   带默认值的布尔值

还要注意，我们需要从实现中移除返回类型。

这还不算太糟。当在我们重载的关键字参数之前有额外的关键字参数*时，事情就变得复杂了。*

# 使用带有默认值的额外参数重载函数

比方说，我们有:

运行 mypy，我们得到

```
main.py:29: note: Revealed type is 'Union[main.Cat, None]'
main.py:30: note: Revealed type is 'Union[main.Cat, None]'
main.py:31: note: Revealed type is 'Union[main.Cat, None]'
main.py:32: note: Revealed type is 'Union[main.Cat, None]'
main.py:33: note: Revealed type is 'Union[main.Cat, None]'
main.py:34: note: Revealed type is 'Union[main.Cat, None]'
main.py:35: note: Revealed type is 'Union[main.Cat, None]'
main.py:36: note: Revealed type is 'Union[main.Cat, None]'
main.py:37: note: Revealed type is 'Union[main.Cat, None]'
main.py:38: note: Revealed type is 'Union[main.Cat, None]'
main.py:39: note: Revealed type is 'Union[main.Cat, None]'
main.py:40: note: Revealed type is 'Union[main.Cat, None]'
main.py:41: note: Revealed type is 'Union[main.Cat, None]'
main.py:42: note: Revealed type is 'Union[main.Cat, None]'
main.py:43: note: Revealed type is 'Union[main.Cat, None]'
main.py:44: note: Revealed type is 'Union[main.Cat, None]'
```

为了使返回类型更加精确，我们可能会像以前一样尝试同样的技巧并重载:

*   文字[假]，默认
*   字面[真实]
*   带默认值的布尔值

然而，这是行不通的(试着运行上面的代码片段看看！).相反，我们需要重载我们想要重载的参数之前的默认参数的每个组合。

这将会回来

```
main.py:94: note: Revealed type is 'None'
main.py:95: note: Revealed type is 'main.Cat'
main.py:96: note: Revealed type is 'main.Cat'
main.py:97: note: Revealed type is 'Union[main.Cat, None]'
main.py:98: note: Revealed type is 'None'
main.py:99: note: Revealed type is 'main.Cat'
main.py:100: note: Revealed type is 'main.Cat'
main.py:101: note: Revealed type is 'Union[main.Cat, None]'
main.py:102: note: Revealed type is 'None'
main.py:103: note: Revealed type is 'main.Cat'
main.py:104: note: Revealed type is 'main.Cat'
main.py:105: note: Revealed type is 'Union[main.Cat, None]'
main.py:106: note: Revealed type is 'None'
main.py:107: note: Revealed type is 'main.Cat'
main.py:108: note: Revealed type is 'main.Cat'
main.py:109: note: Revealed type is 'Union[main.Cat, None]'
```

# 结论

我们已经看到了`typing.overload`如何帮助我们使函数的返回类型更加精确。现在请出去重载一些函数！