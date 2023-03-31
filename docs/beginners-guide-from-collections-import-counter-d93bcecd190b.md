# 初学者指南:从集合进口柜台

> 原文：<https://medium.com/analytics-vidhya/beginners-guide-from-collections-import-counter-d93bcecd190b?source=collection_archive---------2----------------------->

![](img/6410f7a4bc3da9c141a65e6db3abdbe2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

众所周知，Python 因其简单的语言而广受欢迎。然而，有很多模块是围绕 Python 构建的，这使得 Python 使用起来更加简单。模块基本上是别人为你编写的 Python 函数，工作起来完美无缺。知道使用哪个模块以及何时使用模块会使编程变得更简单，因为您不必编写代码来完成相同的任务。尽管如此，您确实需要知道方法背后的逻辑，以便正确地实现它们。

其中最受欢迎的模块是 C [集合](https://docs.python.org/3/library/collections.html)模块。Collections 是一组在容器中存储数据的工具(例如，列表、字典、集合、元组)。

在本文中，我将提到集合模块中我认为非常有用的两个容器:
- Counter
- OrderedDict

之所以重点关注这三个，是因为它们都是 dictionary 的子类。它们实现起来既快又容易。

# 计数器

你可以通过计数器内部的*字符串*、*列表*和*字典*或者通过计数器的方式传入数据(参见下面的减法示例)。

Counter 对传入的元素进行计数，并返回包含元素计数的键、值对。

要实现 Counter，首先需要从 collections 模块导入它。Counter 中的大写字母“C”需要导入，因为 Python 是一种区分大小写的语言。

```
from collections import Counter 
```

一旦导入，您可以简单地开始使用计数器。

```
numbers = [90, 95, 50, 55, 60, 20, 90, 85, 50, 90, 85, 95]
Counter(numbers)output:
Counter({90: 3, 95: 2, 50: 2, 55: 1, 60: 1, 20: 1, 85: 2})
```

或者实例化为您喜欢的变量。但是，您必须在计数器后传递“()”，如下所示。此方法类似于创建手动计数器。

```
# Counter 
c = Counter()
numbers = [90, 95, 50, 55, 60, 20, 90, 85, 50, 90, 85, 95]
for num in numbers:
    c[num]+=1
print(c)output:
Counter({90: 3, 95: 2, 50: 2, 55: 1, 60: 1, 20: 1, 85: 2})

       --------------------------------------------------# Manual
numbers = [90, 95, 50, 55, 60, 20, 90, 85, 50, 90, 85, 95]
grade={}
for num in numbers:
    if num in grade:
        grade[num]+=1
    else:
        grade[num]=1
print(grade)output:
{90: 3, 95: 2, 50: 2, 55: 1, 60: 1, 20: 1, 85: 2}
```

在计数器循环中，不需要创建新的字典或 *if-else* 条件来指定 num 不是数字时如何递增。

在 Counter 中，有两种方法了解起来很有用。
- most_common(n) —其中 *n* 是您想要的常用对数。
-元素()
-减法

*   最常见的(n)

```
from collections import Counter
numbers = [90, 95, 50, 55, 20, 90, 85, 50, 90, 85, 95, 55, 55, 55]
grade=Counter(numbers).most_common(2)
print(grade)output:
[(55, 4), (90, 3)] #prints a list with the most common elements and the number of counts as key, value tuple. 
```

*   元素()

```
from collections import Counter
numbers = {55:4, 90:3}
grade=Counter(numbers)
print(list(grade.elements()))output:
[55, 55, 55, 55, 90, 90, 90] #prints a list of all the elements in the dictionary
```

*   减去

正如我上面提到的，除了列表、字符串或字典之外，还有一种方法可以在计数器中传递数据。它只是为元素声明一个值，counter 函数读为“有 4 个‘a’，3 个‘b’和 2 个‘c’。”此外，当您从另一个元素(在本例中为“d”)中减去值，并且该值不存在于您传递给计数器的值中时，它知道该值为 0，并且不会抛出错误。

```
from collections import Counter
c=Counter(a=4, b=3, c=2, d=0)
d=['a', 'b', 'c', 'd', 'e']
c.subtract(d)
print(c)output:
Counter({'a': 3, 'b': 2, 'c': 1, 'd': -1, 'e': -1})
```

集合中还有其他函数:Deque、Chainmap、UserString 等等。然而，我发现自己使用计数器的频率最高。此外，从 Python 3.6 开始，字典实际上会记住元素插入字典的顺序