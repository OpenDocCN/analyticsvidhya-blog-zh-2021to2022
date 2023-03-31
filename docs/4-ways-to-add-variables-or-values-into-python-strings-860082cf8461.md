# 向 Python 字符串中添加变量或值的 4 种方法

> 原文：<https://medium.com/analytics-vidhya/4-ways-to-add-variables-or-values-into-python-strings-860082cf8461?source=collection_archive---------4----------------------->

有时，需要在字符串中包含变量或非字符串数据类型。

![](img/3b66379a6833d1ae48d08d5f5e6c2376.png)

f 弦的“你好，世界”

**1。逗号**

最常见的方法是用逗号分隔字符串和变量，如下所示。

```
>>> name = “John”>>> age = 16
>>> height = 1.72
>>> print(“Name:”, name, “; Age:”, age, “; Height:”, height, “m”)Name: John ; Age: 16 ; Height: 1.72 m
```

这并没有提供很多定制输出的能力，并且仍然保持一切良好。我们还需要注意，在输出字符串中，每个逗号分隔都是一个“空格”。

我们可以做得更好。

**2。%运算符**

这是一个古老的学校方法，但你仍然会经常看到它。事实上，我也经常使用它。

```
>>> print(“Name: %s; Age: %d; Height: %fm” % (name, age, height))Name: John; Age: 16; Height: 1.720000m
```

这给了我们更多的灵活性，但是有两个主要的限制。

1.您不能包含任何数据类型，如列表、日期时间等

2.%s 代表字符串，%d 代表整型，%f 代表浮点型。你不能把一个人错当成另一个人。

```
>>> print(“Name: %s; Age: %f; Height: %dm” % (name, age, height))Name: John; Age: 16.000000; Height: 1m
```

在上面的例子中，没有错误，因为数据类型是可转换的。

因此，python 自动将必要的数据类型转换为输入。

然而，有一些浮点数，例如“infinity”不能转换成 int。

```
>>> import math
>>> math.inf
inf
>>> int(math.inf)
Traceback (most recent call last):
File “<stdin>”, line 1, in <module>
OverflowError: cannot convert float infinity to integer>>> print(“Infinity %s” % math.inf)
Infinity inf>>> print(“Infinity %f” % math.inf)
Infinity inf>>> print(“Infinity %d” % math.inf)
Traceback (most recent call last):
File “<stdin>”, line 1, in <module>
OverflowError: cannot convert float infinity to integer
```

当%运算符有多个参数时，它必须是一个元组。最后，您可以指定一个浮点数的长度，如下所示(点后跟小数位数)。

```
>>> print(“Name: %s; Age: %d; Height: %.1fm” % (name, age, height))Name: John; Age: 16; Height: 1.7m
```

**3。字符串格式化方法**

Python string 有一个格式方法，可以让你插入任何数据类型。它非常类似于%运算符(用“{}”替换了“%s”或“%d”或“%f ”),但更好。

```
>>> print(“Name: {}; Age: {}; Height: {}m”.format(name, age, height))Name: John; Age: 16; Height: 1.72m
```

格式方法更好，因为

1.您不必确定数据类型。

2.您可以插入任何数据类型。

3.您可以通过索引重复使用某个值。

```
>>> from datetime import datetime>>> print(“{} test scores in a list {} on {}”.format(name, [23, 25.5, 28], datetime.now()))John test scores in a list [23, 25.5, 28] on 2021–03–15 13:30:39.754769# Indexing like a list
>>> print(“{0} is {3}. {0} loves {1}, {2} and {0}”.format(name, ‘Harry Potter’, ‘ice cream’, age))John is 16\. John loves Harry Potter, ice cream and John# Another form of index
>>> print("{fname} {lname} is {age}".format(age=16, fname="John", lname="Snow"))John Snow is 16
```

最后一个索引示例还说明了 format 方法中的值可以表示为 key=value 对的列表。

**4。f 弦**

另一个有吸引力的选项是格式化字符串(f-string)。

```
>>> print(f”{name} is {age}, and today is {datetime.now().strftime(‘%Y-%m-%d’)}”)John is 16, and today is 2021–03–15
```

主要的优点是，它让你很容易知道一个值在哪里，我只是认为它看起来🆒 😎。但是缺点是

1.它需要 python>=3.6。你必须确定这一点以避免错误。

2.表达式部分不能包含反斜杠(在` {}`内)。

3.第二点也意味着你不能在没有语法错误的情况下包含“”。

总之:

*   格式方法是我认为最好的方法。
*   f 弦很酷，非常酷😎。但要求 python≥3.6
*   %格式还不错
*   方法 1 非常适合快速操作。