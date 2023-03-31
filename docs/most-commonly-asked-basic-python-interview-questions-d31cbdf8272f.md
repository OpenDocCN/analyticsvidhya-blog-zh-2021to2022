# 最常见的基本 Python 面试问题

> 原文：<https://medium.com/analytics-vidhya/most-commonly-asked-basic-python-interview-questions-d31cbdf8272f?source=collection_archive---------1----------------------->

![](img/353f8e6c8cab23c0e3a0157bf5b3c7af.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

嗨，朋友们，在这个博客中，我们将讨论一些面试中经常被问到的 python 问题，看看你是否能回答所有的问题。

# 1.Python 中的数据类型？

这里面试官对基本的数据类型不感兴趣，比如字符，整数，字符串。相反，他想更多地了解列表、元组、集合和字典。

**列表**类似于数组，是数据的有序集合。它非常灵活，因为列表中的项目不需要属于同一类型。

list1 = ['A '，1，2，' AB '，[1，2，3]]

一个**元组**也是 Python 对象的有序集合，类似于列表。元组和列表之间的唯一区别是元组是不可变的，即元组在创建后不能被修改。

元组 1 = (1，2，3，4)

tuple2 = (1，)

只有一个元素的元组应该在右括号前用逗号指定，如上例所示。

集合是数据类型的无序集合，它是可迭代的、可变的，并且没有重复的元素。可以使用 set()内置方法创建集合

set1 = set("APPLE")
{'L '，' A '，' E '，' P'}

**Python 中的字典**是数据值的无序集合，用于以键值对的方式存储数据值。使用内置的 dict()函数可以创建字典。使用{}可以创建一个空词典。

dict1 = { } #空字典

dict2 = { 1 : 'A '，2 : 'B '，3 : 'C' }

# **2。list 和 Tuple，Set 的区别？**

**列表**是可变的，可以存储任何类型的元素。

**tuple**是 python 中不可变的序列，可以存储任何类型的元素。因为它们是不可变的，所以它们在系统中需要较少的内存。他们是

集合是可变的，但是，只有不可变的对象可以存储在其中。

# **3。我们可以用这个列表作为字典关键字吗？**

我们可以使用整数、字符串、元组作为字典的键，但是不能使用列表作为字典的键。这主要是因为→列表是可变的。

Dictionary 在字典中查找一个键需要 O(1)时间，它为每个键生成一个哈希值，通过这个哈希值，它可以跟踪它的元素。但是由于列表是可变的，改变列表会改变哈希值，而哈希值在字典中是找不到的。

# **4。贴图、过滤和减少的区别？**

**MAP —** *map(function，iterables)* —此函数接受另一个函数和一系列“iterables”作为参数，并在将函数应用于序列中的每个 iterables 后提供输出。

```
def cube(a):
    return a*a*a
x = map(cube, (1,2,3,4))  #x is the map object
print(x)
print(list(x))OUTPUT:
[1, 8, 27, 64, 125]
```

**FILTER—***FILTER(function，iterables) —* 该函数用于生成一个输出值列表，当调用该函数时返回 true。

```
def pos(x):
    if x>=0:
        return x
y = filter(pos, (-1,-2,3,4))  
print(y)
print(list(y))OUTPUT:[3, 4]
```

**Reduce**—*Reduce(function，iterables) —* 该函数指定应该将哪个表达式应用于“iterables”。必须使用功能工具模块来导入此功能。

```
from functools import reduce
reduce(lambda a,b: a+b,[1,20,40,100])OUTPUT:161
```

# **5。什么是发电机？**

Python 提供了一个称为生成器的特殊函数，它可以创建迭代器函数。它不返回单个值，而是返回一个包含一系列值的迭代器对象。在生成器函数中，使用 yield 语句而不是 return 语句。

在生成器中我们用 yield 语句代替 return，区别在于 yield 返回值并暂停执行，同时保持内部状态，而 return 语句返回值并终止函数的执行。

```
def gen():
   print('First')
   yield 1
   print('Second')
   yield 2
   print('Third')
   yield 3x = gen()
print(x.__next__());
print(x.__next__());
print(x.__next__());OUTPUT:First
1
Second
2
Third
3
```

在读取大量数据时，最好使用生成器来节省内存。由于元素是动态生成的，所以只有在第一个元素被消耗后，下一个元素才会生成，这比迭代器更节省内存。

# **6。什么是装修工？**

这是 python 中最好的特性之一。通过使用 decorators，我们可以向一个方法添加额外的特性，而不用永久地修改它。

```
# Actual method which will print the Cube of a Number
def cube(num):
    print(num*num*num)# Decorator/Wrapper for the above method, such that we always wants a positive value of Cube, even if the number is negative
def pos_cube(func):
    def inner(num):
        if num < 0 :
            num *= -1
        return func(num)
    return inner

pos_cube = pos_cube(cube)
pos_cube(-3)Another Way to call the decorators:def pos_cube(func):
    def inner(num):
        if num < 0 :
            num *= -1
        return func(num)
    return inner

@pos_cube
def cube(num):
    print(num*num*num)

cube(-3)OUTPUT :27
```

# **7。你对 ORM 了解多少？**

ORM 代表对象关系映射器。它充当关系数据库表和 python 对象之间的桥梁。它在关系数据库上提供了高级抽象，允许开发人员编写 Python 代码而不是 SQL 来创建、读取、更新和删除数据库中的数据和模式。开发人员可以使用他们熟悉的编程语言来处理数据库，而不是编写 SQL 语句或存储过程。

编写 Python 代码而不是 SQL 的能力可以加快 web 应用程序的开发，尤其是在项目的开始阶段。潜在的开发速度提升来自于不必从 Python 代码切换到编写声明性范例 SQL 语句。

使用 SQLAlchemy ORM，查询检查数据库中是否存在“电子邮件”,而不是编写典型的老式 SQL 查询。

```
db.session.query(Data).filter(Data.email == email).count()==0
```

# 8。Python 中的类和继承:

像任何其他语言一样，类提供了一种将数据和功能捆绑在一起的方法。

语法和示例:

```
class Node:
   def __init__(self,data):
       self.val = data
       self.left = None
       self.right = None
   def about(self):
       print("This is the example of class in python")n = Node(1)
n.about()
print(n.val)OUTPUT:
This is the example of class in python
1
```

我希望你喜欢这篇文章，如果这个博客对你的学习有所帮助，请鼓掌并关注我们。如有任何与内容相关的疑问，请随时致电**medha.rwt@gmail.com**与我联系。非常欢迎对这篇文章的任何改进提出建议。一定要让我知道你对这个博客的评论。告诉我你们想让我为你们写的下一个主题。

在那之前保持安全，继续阅读。