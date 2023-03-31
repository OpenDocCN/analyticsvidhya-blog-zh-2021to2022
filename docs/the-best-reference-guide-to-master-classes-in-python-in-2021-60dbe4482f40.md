# 2021 年 Python 中大师类的最佳参考指南。

> 原文：<https://medium.com/analytics-vidhya/the-best-reference-guide-to-master-classes-in-python-in-2021-60dbe4482f40?source=collection_archive---------8----------------------->

## 用新方法与 python 中的类交互。

![](img/fc03a648a758c4a246ef79218f2ce0a8.png)

我们在市场上看到许多不同的蔬菜。它们有不同的形状、颜色和大小。然而，它们都属于蔬菜类。我们可以用类似的方式思考 Python 中的类。一个类代表一种类型(蔬菜)，我们可以创建该类型的许多实例(市场上不同的蔬菜，如胡萝卜、西红柿等)。

面向对象编程(OOP)模型是围绕拥有属于特定类型的对象的思想而构建的。类型是解释对象的东西。

对一个对象的解释对理解 OOP 很重要。我们需要彻底了解:

*   一个物体意味着什么
*   该对象存储什么类型的数据
*   我们如何与物体互动
*   我们如何在代码中执行对象

构成对象解释的这些点是用类定义的。Python 中的一切都是不同类型的对象，如整数、列表、字典、函数等。我们使用类来定义这些类型的对象。

今天在这篇文章中，我们将回顾 Python 中的类的含义，如何在 Python 中创建和使用类，以及使用类能给我们带来什么样的好处。

***那么，没有任何进一步的到期让我们开始吧！！！***

类别拥有以下内容:

*   **数据属性**:创建类的实例时需要
*   方法:这显示了我们与类的实例交互的方式。

我们使用 Python 中的类最多。例如，当我们创建一个列表时，我们创建了一个类型列表的实例。

```
words = ['data', 'science', 'machine', 'learning']
```

我们实际上对 list 类是如何创建的并不感兴趣。我们只需要知道如何与列表交流，并在代码中有效地使用它们。这是一个**抽象**。

例如，我们也可以使用 remove 方法从列表中删除一个条目。

```
words.remove('data')print(words)
['science', 'machine', 'learning']
```

# 创建一个类

下面的代码创建了一个名为 Book 的类。

```
class Book():  
    def __init__(self, name, writer, word_length):
        self.name = name
        self.writer = writer
        self.word_length = word_length
```

__init__ 是一个特殊的函数，当创建一个类的实例时，它会自动执行。它也被称为构造函数。

__init__ 函数的参数表示类的数据属性。

自我指的是实例。你可以用任何词代替“自我”,我们用“自我”是因为它很常见。

创建实例

```
b1 = Book("Pandas", "John Doe", 100000)print(type(b1))
<class '__main__.Book'>
```

代码中的 b1 是一个属于 Book 类的对象。

我们也可以用下面的方法修改类的属性。

```
print(b1.name)
Pandasb1.name = 'NumPy' #updates the name attributeprint(b1.name)
NumPy
```

# 定义类方法

该类只有数据属性。我们应该添加一些方法来使它变得有用和实用。

例如，我们可以实现一个方法，返回给定字体大小的页数。我们通过计算字数来测量这本书的长度。该方法将根据长度和字体大小计算页数。

```
def number_of_pages(self, fontsize=12):
  word_length = self.word_length
  if fontsize == 12:
    words_in_page = 300
  else:
    words_in_page = 300 - (fontsize - 12) * 10
  return round(word_length / words_in_page)
```

我们在课堂上增加了页数。它根据字数和字体大小来计算一本书的页数。

如果一个函数是在一个类定义中声明的，那么它需要访问一个实例的数据属性，我们需要告诉这个函数如何访问它们。这就是我们在 number_of_pages 函数的第一行中所做的。我们可以从类中访问一个方法。

```
b1 = Book("Pandas", "John Doe", 100000)b1.number_of_pages()
Book.number_of_pages(b1)
```

我们需要为我们的类定义一些方法来使用 Python 的一些内置函数。考虑打印功能。

```
print(b1)
```

print 函数返回对象的类型和内存位置。但是，我们可以通过在我们的类中实现 __str__ 方法来改变它的行为。

```
def __str__(self):
  return "<" + self.name + ", by " + self.writer + ">"
```

我们在类定义中添加了 __str__ 方法。下面的代码是打印函数如何为我们的类工作的:

```
print(b1)<Pandas, by John Doe>
```

# 类与实例变量

类变量是在类内部声明的，但它们应该在函数外部。实例变量是在构造函数内部声明的。

类变量更加通用，可以应用一个类的所有实例。实例变量更加具体，并且为每个实例单独定义。在类和实例变量之间有一个对比差异是非常有用的。

考虑我们前面定义的 Book 类。如果我们将它们定义为类变量，我们就不必为每个创建的实例专门声明。

```
class Book():  
    page_width = 14
    cover_color = "blue"  
def __init__(self, name, writer, word_length):
    self.name = name
    self.writer = writer
    self.word_length = word_length
```

我们将 page_width 和 cover_color 实现为类变量，因为它们在类定义中。

让我们创建这本书的一个实例。

```
b2 = Book("Machine Learning", "Jane Doe", 120000)
```

创建此实例时，我们没有定义类变量。然而，b2 保存了这些变量，我们可以访问它们。

```
b2.page_width
b2.cover_color
'blue'
```

我们可以选择更改特定实例的类变量。

```
b2.cover_color = 'red'b2.cover_color
'red'
```

特定实例中的更改对类变量没有任何影响。

```
Book.cover_color
'blue'
```

# 结论

我们在本文中讨论的内容可以被认为是对 Python 类的全面介绍。我们已经提到了类对于面向对象编程的重要性，以及类如何展示抽象和继承等关键概念。

Python 类中包含了更多的内容。一旦你熟悉了基础知识，就可以随意进入更高级的话题了。

*原发表于*[*https://fitter chie . in*](https://fittechie.in/the-best-reference-guide-to-master-classes-in-python-in-2021/)*。*