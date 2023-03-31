# Python 101 : *args 和**kwargs

> 原文：<https://medium.com/analytics-vidhya/python-101-args-and-kwargs-e27f7a0b3de9?source=collection_archive---------24----------------------->

## 理解什么是*args 和**kwargs 以及如何使用它们

![](img/1149218de9322c06b22c7f1c1c6e8c74.png)

照片由[德拉诺尔 S](https://unsplash.com/@dlanor_s?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

假设我们有一个将数字相加的函数。

```
def adding(x,y):
  print(x + y)adding(2,3)
```

如果我们运行这段代码，我们将得到 5 作为输出，非常简单。但是如果我们在加法函数上传递 2 个以上的参数(要相加的数字),比如:

```
adding(2,3,4,5)
```

我们会得到这样的错误

```
TypeError : adding( ) takes 2 positional arguments but 4 were given
```

发生这种情况的原因是，该函数只需要将两个数字相加(x 和 y，我们在函数中作为参数传递)。

但是在这里，我们给了它 4 个值来添加，这根本不起作用。但是我们希望我们的“加法”可以添加任意多的数字。

## 我们如何解决这个问题？

在函数中使用 ***args** ，可以在函数内部传递任意数量的参数。如果我们现在运行加法函数，只打印*args:我们将得到 1 2 3 4 5 作为输出，因此*args 充当一个变量，存储我们作为列表传递给它的所有参数。

```
def adding(*args):
   print(*args)
adding(1,2,3,4,5)
```

现在我们可以使用加法函数添加任意多的数字了！我们不需要担心有多少参数需要传递给加法函数。

```
def adding(*args):
 sum = 0
 for n in args:
   sum = sum + n
 print(sum)adding(1,2,3,4,5)
```

这个函数看起来有点不同，因为记住*args 变量被当作一个列表，我们需要迭代它来添加所有的数字。

我建议你现在就亲自尝试一下。打开您的 IDE 并开始使用它。

****kwargs** 做一些类似于 ***args** 的事情，这里我们放“关键字参数”而不仅仅是“参数”

让我们看看这段代码，你会更好地理解。

```
def func(**kwargs):
   print(kwargs)func(Firstname = “Rishi”, Lastname = “Konapure”, Age = 25, Phone = 1234567890)
```

您将获得如下输出

```
{Firstname = “Rishi”, Lastname = “Konapure”, Age = 25, Phone = 1234567890}
```

**kwargs 返回一个字典，而不是像*args 那样返回一个列表。

好吧，*args 和**kwargs 很酷，但是我们在哪里使用它们呢？

我第一次遇到*args 和**kwargs 是在处理数据库查询的时候。很多时候你不知道你将得到的输入的性质，*args 和**kwargs 帮助我们解决这些问题。

我的建议是在项目中使用它们，只有这样你才能真正理解它们的用途。