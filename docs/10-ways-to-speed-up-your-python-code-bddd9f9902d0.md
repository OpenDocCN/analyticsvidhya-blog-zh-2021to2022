# 提高 Python 代码速度的 10 种方法！🚀

> 原文：<https://medium.com/analytics-vidhya/10-ways-to-speed-up-your-python-code-bddd9f9902d0?source=collection_archive---------12----------------------->

![](img/ce8b66ea75ce1326c39f13ea71beac8c.png)

爱好者们你们好！Python 很可能是我们的第一种编程语言。
起初，一切都显得简单明了。然而，我们大多数人在处理一个复杂的算法问题时都经历过时间限制。然而，我们不能完全责怪 Python 的缓慢；程序员也可能会加快 Python 的速度(并不是说这种语言不慢)。所以今天，我们将看看 10 种让你的 Python 代码运行更快的方法！

# 1.使用列表理解

如果你能利用列表理解，不要为其他任何事情烦恼。例如，下面的代码列出了 1 到 1000 之间的所有数字，它们是 3 的倍数:

```
L = []
for i in range (1, 1000):
    if i%3 == 0:
        L.append (i)
```

使用列表理解，应该是:

```
L = [i for i in range (1, 1000) if i%3 == 0]
```

列表理解比使用`append()`方法更快。

# 2.使用生成器和用键排序

生成器通过允许您编写一个一次返回一个对象而不是一次返回所有对象的函数来提高内存效率。这是一个很好的例子，当你期待一个很长的数字列表，并把它们加起来。

此外，尽可能使用键和默认的 sort()技术对列表中的项目进行排序。在下面的内容中，请注意在每种情况下，列表是如何根据您指定为 key 参数的一部分的索引进行排序的。这种方法对字符串和数字同样有效:

```
import operatorsomelist = [(1, 5, 8), (6, 2, 4), (9, 7, 5)]
somelist.sort(key=operator.itemgetter(0))somelist#Output = [(1, 5, 8), (6, 2, 4), (9, 7, 5)]somelist.sort(key=operator.itemgetter(1)) somelist#Output = [(6, 2, 4), (1, 5, 8), (9, 7, 5)] somelist.sort(key=operator.itemgetter(2)) somelist #Output = [(6, 2, 4), (9, 7, 5), (1, 5, 8)]
```

# 3.记住内置函数。

Python 包含了大量的电池。您可以开发高质量、高效的代码，但是底层库很难被击败。这些都经过了微调和严格测试(毫无疑问，就像您的代码一样)。检查内置列表，查看代码中是否有重复的功能。

# 4.减少 for 循环的使用

因为 Python 中的 for 循环是动态的，所以它比 while 循环花费的时间更长。因此，不要使用 for 循环，而是使用 while 循环。

# 5.使用适当的数据结构

使用适当的数据结构对运行时有很大的影响。Python 中内置的数据结构有列表、元组、集合和字典。然而，大多数人在任何情况下都使用这个列表。但是，这不是最好的选择。根据您的任务，使用适当的数据结构。尽可能使用元组而不是列表。遍历一个元组比遍历一个列表更容易。

# 6.如果可能，使用`in`。

使用 in 关键字检查列表的 if 成员资格通常更快。

```
for name in member_list:
  print('{} is a member'.format(name))
```

# 7.在导入模块时偷懒。

当您第一次开始学习 Python 时，您无疑被告知要立即导入所有您需要的模块。也许你仍然按字母顺序排列这些。

这种方法使得跟踪程序的依赖关系变得容易。缺点是所有的导入将同时加载。

为什么不尝试不同的东西呢？模块只能在需要时加载。这种策略可以更均匀地分配模块加载时间，也许可以减少内存使用高峰。

# 8.使用`join()`来连接字符串。

Python 中的字符串连接是用+完成的。然而，Python 字符串是不可变的；因此，+操作包括在每一步构造一个新的字符串并复制旧的内容。使用数组模块编辑单个字符，然后使用 join()函数重新创建最终的字符串是一种更有效的方法。

```
new = "This" + "is" + "going" + "to" + "require" + "a" + "new" + "string" + "for" + "every" + "word"
print(new)
```

这段代码的输出将是，

```
Thisisgoingtorequireanewstringforeveryword
```

另一方面，这段代码:

```
new = " ".join(["This", "will", "only", "create", "one", "string", "and", "we", "can", "add", "spaces."])
print(new)
```

将打印:

```
This will only create one string and we can add spaces.
```

干净，更优雅，更快捷！

# 9.不要使用点运算

尽量避免点操作。参见下面的程序。

```
import math
val = math.sqrt(60)
```

代替上面的风格，像这样写代码:

```
from math import sqrt
val = sqrt(60)
```

因为当你使用`. (dot)`调用一个函数时，它首先调用`__getattribute()__`或`__getattr()__`，然后使用字典操作，这很费时间。所以，尝试使用从模块导入功能。

# 10.不要使用`global`变量

Python 中的 global 关键字用于声明全局变量。另一方面，全局变量比局部变量需要更长的运行时间。因此，除非绝对必要，否则避免使用全局变量。

# 额外收获:使用`while 1`进行无限循环。

如果你正在监听一个套接字，你可能想要使用一个无限循环。实现这一点最常见的方法是使用 while True。这个管用；但是，使用 while 1 可以在很短的时间内得到相同的结果。
因为是数值比较，所以这是单跳操作。

有了这些，现在你可以让你的代码飞起来🚀。

如果你有任何其他的建议，欢迎在评论中分享！