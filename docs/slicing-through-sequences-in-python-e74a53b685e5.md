# Python 中的序列切片

> 原文：<https://medium.com/analytics-vidhya/slicing-through-sequences-in-python-e74a53b685e5?source=collection_archive---------15----------------------->

![](img/c4078bea8314e3f6986f8d672cc1ecd3.png)

使用 [canva](https://www.canva.com/) 创建

在 Python 中，从 iterable 中提取元素子集的最简单的方法之一是应用切片的概念。在这篇文章中，让我们来了解一下如何分割任何可迭代的对象，比如字符串、列表或元组，以便分割出一个子集！

在开始之前，让我们重温一下 Python 中索引的工作原理。

让我们声明一个包含从`0`到`9`的所有整数的列表。

```
num_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

由于 Python 遵循基于零的索引，为了访问列表的第一个元素，我们通过传入索引`0`来使用`index operator ([])`。

```
print(num_list[0])
```

输出:

```
0
```

在我们的列表中有`10`元素，范围从索引`0`到`9`，如果我们试图使用`num_list[10]`打印第 10 个索引处的元素，我们会得到以下错误:

```
IndexError: list index out of range
```

Python 还允许通过从列表末尾开始计数，使用负索引来访问序列中的元素。

例如，`num_list[-1]`访问列表末尾的第一个元素，即`9`。

这意味着:

```
num_list[0] == num_list[-10] # or 0 in the listnum_list[1] == num_list[-9] # or 1 in the listnum_list[2] == num_list[-8] # or 2 in the listnum_list[3] == num_list[-7] # or 3 in the listnum_list[4] == num_list[-6] # or 4 in the listnum_list[5] == num_list[-5] # or 5 in the listnum_list[6] == num_list[-4] # or 6 in the listnum_list[7] == num_list[-3] # or 7 in the listnum_list[8] == num_list[-2] # or 8 in the listnum_list[9] == num_list[-1] # or 9 in the list
```

因此，对于长度为`n`的序列`s`，可以将`s`的第`i`个元素作为，

```
s[i] **or** s[i - n]
```

既然我们已经更新了对 Python 中序列如何索引的理解，我们可以扩展这一知识，使用以下符号从序列`s`中分割出子序列:

```
s[start:stop:step]
```

在上述符号中，

1.  `start`代表`s`中我们应该开始切片的索引。`start`的默认值为`0`。
2.  `stop`代表`s`中我们应该切片到(但不包括)的指数。`stop`的默认值是`n - 1`，其中`n`是序列的长度。
3.  `step`代表从`start`到`stop`时，我们应该增加的索引值。`step`的默认值为 1。

如果我们对包含`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]`的`num_list`执行切片操作，而没有为`start`、`stop`和`step`提供值，那么切片操作将分别为这三个变量使用默认值`0`、`9`和`1`。

```
print(num_list[::])
```

输出:

```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

乍一看，好像是`num_list`印的。但是我们需要记住切片操作总是返回一个新的列表。在这种情况下，因为我们的子序列的`start`、`stop`和`step`的值与原始序列相同(因为我们使用默认值)，所以我们得到一个新的列表，它包含与原始列表相同的元素。

接下来，让我们看看可以从`num_list`中以升序提取包含所有偶数的子序列的不同方法。

# 1.提供开始、停止和步进

为此，让我们确定我们的`start`、`stop`和`step`变量:

1.  我们列表中的第一个偶数是`0`，它位于从列表开始的索引`0`处，因此我们可以将索引`start`设置为`0`
2.  我们列表中的最后一个偶数是`8`，它位于从列表开始的索引`8`处，所以我们可以将`stop`索引设置为`9`(这样我们可以切片到序列中第`9`索引处的元素，但不包括它)
3.  从起始索引开始，我们列表中的每一个备选数字都是偶数，所以我们可以将`step`设置为`2`，在每一步中递增`2`。

```
print(num_list[0:9:2])
```

当我们运行此代码时，从`0-9`开始按升序排列的偶数列表将作为输出打印出来:

```
[0, 2, 4, 6, 8]
```

# 2.提供开始和步骤

因为我们的子序列的`stop`索引的默认值已经是`9` ( `len(num_list) - 1`)，所以我们可以得到与上面相同的偶数列表，而不需要像这样为我们的片提供一个`stop`索引:

```
print(num_list[0::2])
```

# 3.提供停止和步进

类似地，由于我们的子序列的`start`索引的默认值已经是`0`，我们可以得到相同的偶数列表，而不需要像这样向切片提供`start`索引:

```
print(num_list[:9:2])
```

# 4.仅提供一步

最后，由于我们的子序列的`start`和`stop`索引的默认值与第一种方法中提供的相同，我们可以通过将`step`变量设置为`2`来获得偶数列表:

```
print(num_list[::2])
```

# 其他方式:

我们甚至可以使用负指数来获得相同的结果，如下所示:

```
# 1:  
print(num_list[0:-1:2]) # setting stop index to -1 to access the first element from the end
# or
print(num_list[:-1:2]) # using the default value for start index# 2:
print(num_list[-10:9:2]) # setting start index to -1 to access the last element from the end 
# or 
print(num_list[-10::2]) # using the default value for stop index# 3:
print(num_list[-10:-1:2]) # using negative start and stop indices
```

侧栏:

在上面提到的例子中，我们总是为`step`提供一个值，因为它帮助我们从开始索引到结束增加两步，以便找到所有的偶数。

如果我们只需要从`num_list`的开始提取前三个数字，我们不需要为`step`传递一个值，因为我们将递增`1`，这是默认的`step`值。

```
print(num_list[0:3])
```

输出:

```
[0, 1, 2]
```

常见应用:

切片的一个非常常见的应用是反转序列。

例如，如果我们想要反转`num_list`，我们可以通过将`step`变量设置为-1 来实现:

```
print(num_list[::-1])
```

输出:

```
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

当我们将`step`设置为`-1`时，Python 在内部交换了`start`和`stop`索引，从而给我们一个颠倒的列表。

因此，如果要求您在不使用 Python 内置的`reverse`方法的情况下反转字符串`"hello"`，您可以通过运行以下代码行使用切片来完成此操作:

```
print("hello"[::-1])
```

输出:

```
"olleh"
```

*最后，问你一个问题:*

你能想出不同的方法从`num_list`中提取奇数列表，并按降序排列吗？

本帖原载[此处](https://www.vasudhajha.com/post/slicing-in-python/)。如果你想阅读更多关于 Python 基础的帖子，你可以在这里找到它们。

感谢您的阅读！