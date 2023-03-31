# Python 中的文档字符串

> 原文：<https://medium.com/analytics-vidhya/docstrings-in-python-a7418906585f?source=collection_archive---------15----------------------->

![](img/11031f521b6fce7e7952ebdc9594f01c.png)

Docstring 是“文档字符串”的简称。

文档字符串不过是我们在 Python 中使用的一种多行注释。它们用于描述您在代码中使用的函数、方法、类或模块。

当您在代码中添加一个 docstring 时，这个特定的描述必须用三重引号括起来，如下所示。

```
"""This is my docstring and I am writing it in between triple quotes"""
```

所以不就是又一个评论吗？❓

![](img/3f85e92d7e31b8220db316446e9233f4.png)

不要！文档字符串有一些特殊之处。

**位置！**如果你要在你的代码中添加一个 docstring，你必须在特定的函数、方法、类或模块的定义之后输入它。这就是 docstrings 不同于其他注释的地方。

让我给你看一个例子。

```
def addNumbers(num1, num2):
    """This function adds num1 and num2 and return the value"""
    return num1+num2
```

还是…不就是另一个评论吗？😧

![](img/f7484cf0099d0c0d220e919a668fc823.png)

没有。惊喜来了！

您可以使用一行非常简单的代码来访问 docstrings。怎么会？

> "文档字符串作为它们的`__doc__`属性与对象相关联."

这意味着 _doc_ 可以访问任何给定函数、方法、类或模块的 docstring。这就是为什么 docstring 的位置很重要。

我们用来访问 docstring 的语法如下。

```
function_name._doc_
```

让我们尝试访问上面例子中的 docstring。

```
def addNumbers(num1, num2):
    """This function adds num1 and num2 and return the value"""
    return num1+num2print(addNumbers._doc_)
```

输出:

```
This function adds num1 and num2 and return the value
```

自己尝试，享受男生，因为只要你喜欢你所学的东西，他们就会给你带来更多惊喜，让你感到舒服。

这是这篇文章的结尾，我希望你能学到一些新的东西。祝你们好运！