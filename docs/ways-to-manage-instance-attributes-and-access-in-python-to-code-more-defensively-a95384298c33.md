# 在 Python 中更具防御性地管理实例属性及其对代码的访问

> 原文：<https://medium.com/analytics-vidhya/ways-to-manage-instance-attributes-and-access-in-python-to-code-more-defensively-a95384298c33?source=collection_archive---------25----------------------->

没有人需要介绍 Python 是一种优雅的语言，很多人都喜欢，它有一个伟大的开发社区作为后盾，它被广泛用于不同的应用程序，如数据、web、桌面 GUI、商业应用程序等。

但是有一些警告，理解 python 的内部机制如何工作将帮助我们更好地编码，更重要的是编写易于维护的通用库。

![](img/6c70cecd09d564d167184d8c41777001.png)

这篇文章的灵感来自于阅读 David Beazley 的一本关于 python 的书，我们使用他的方法在 Apache Spark 上编写了一个框架，用于创建可维护的应用程序。(使用 spark 的 ETL 作业更容易编写，但是很难维护)

# **理解类、实例及其属性**

我们将从理解类、实例和属性等基本概念开始。

由于我们的用例涉及到很多依赖关系，我举一个简单的例子，这个例子对每个人来说都很容易理解，并试图解释事情。假设我们正在用 python 创建一个简单的类和该类的一个实例。

```
class Person(object):
    def __init__(self, name, age, address, salary):
        self.name = name
        self.age = age
        self.address = address
        self.salary = salaryperson1 = Person("Erling", 22, 'US', 1000)
```

似乎我们有一个与创建的实例相关联的特殊属性，称为“__dict__”，它描述了对象，这意味着它包含了对象的所有属性。

```
person1.__dict__
{‘name’: ‘Erling’, ‘age’: 22, ‘address’: ‘US’, ‘salary’: 1000}
```

我们也可以使用属性名作为 dict 属性的键来访问属性。

```
person1.__class__
<class '__main__.Person'>person1.__class__.__dict__['name']
'Erling'
```

这是意料之中的。但是我敢肯定，作为一名 python 程序员，您可能至少曾经面临过这个问题。

```
person1.salary = '2000' # wrong type
person1.adres = 'Norway' # wrong attribute nameperson1.__dict__
{'name': 'Erling', 'age': 27, 'address': 'US', 'salary': '2000', 'adres': 'Norway'}
```

现在，我们的一个属性是错误的类型，由于一个打字错误，我们意外地创建了一个新的属性，python 没有抱怨，根据您的实现的复杂性，您的程序可能会变得疯狂，并且难以排除故障。这是众所周知的，因为 python 是解释性的。

有一些方法可以解决这个问题，下面是其中之一。

# **getter 和 setter**

与其他面向对象语言(例如 Java)相比，python 中 Getters 和 setters 的工作方式几乎没有什么不同。在 python 中使用它的主要目的可能是隐藏对类属性的直接访问，并可能在我们访问/修改实例的属性时添加一些自定义验证或其他逻辑。

回到我们的例子，我们希望避免为属性设置错误的类型，我们可以编写如下代码。(在 python 中，我们使用@property decorator，setter 来实现这一点)。

```
class Person(object):
    def __init__(self, name, age, address, salary):
        self.name = name
        self.age = age
        self.address = address
        self._salary = salary[@property](http://twitter.com/property)
    def salary(self):
        return self._salary[@salary](http://twitter.com/salary).setter
    def salary(self, newsalary):
    if not isinstance(newsalary, int):
        raise TypeError('Salary expected an Int value')
    self._salary = newsalary[@property](http://twitter.com/property)
    def age(self):
        return self._age[@age](http://twitter.com/age).setter
    def age(self, newage):
    if not isinstance(newage, int):
        raise TypeError('Age expected an Int value')
    if newage < 0:
        raise ValueError('Age should be more than zero')
    self._age = newage>>> person1 = Person('Erling', 21, 'Norway', 1000)person1.__dict__{'name': 'Erling', '_age': 21, 'address': 'Norway', '_salary': 1000}
```

程序员暗示以' _ '开头的实例变量是内部实现，不应该使用实例直接访问。(python 约定)

现在，我们可以将年龄和薪水作为常规属性来访问，这有我们的验证逻辑。

```
>>> person1.age = 25>>> person1.salary = 2000>>> person1.__dict__{‘name’: ‘Erling’, ‘_age’: 25, ‘address’: ‘Norway’, ‘_salary’: 2000}person2 = Person(‘Haaland’, ‘20’, ‘Norway’, 3000)Traceback (most recent call last):File “<stdin>”, line 1, in <module>File “<stdin>”, line 4, in __init__File “<stdin>”, line 21, in ageTypeError: Age expected an Int value
```

1.  但是这产生了另一个问题，有太多的锅炉板代码，很快就会变得令人讨厌。(考虑一下为每个创建的类编写这样的代码)
2.  我们还没有处理在实例中添加新属性的问题。

# 通过理解“.”来管理属性

我们知道可以使用“.”来获取实例的属性但是这是如何工作的呢？

假设我们做到了

人员 1 .薪金= 2000

似乎 Python 会得到实例的类，检查它的 __dict__ 属性中的关键字“salary”，这将返回一个属性，然后检查该属性是否有“__set__”方法，如果有，则调用 set 方法。一个消除困惑的例子。

```
person1.__class__
<class '__main__.Person'>
person1.__class__.__dict__['salary']
<property object at 0x10349af90>
person1.__class__.__dict__['salary']
<property object at 0x10349af90>
property = person1.__class__.__dict__['salary']
>>> hasattr(property, '__set__')
True
property.__set__(person1, 1000)
```

同样的过程也适用于从实例中获取属性。

现在，我们能利用这一点来减少我们需要使用 property 和 setter 为我们创建的类编写的代码行吗？似乎我们可以。

```
class Integer(object):
    def __init__(self, name):
        self.name = name
    def __get__(self, instance, cls):
        return instance.__dict__[self.name]
    def __set__(self, instance, value):
        if not isinstance(value, int):
            raise TypeError('Expected an int value')
        instance.__dict__[self.name] = value

class String(object):
    def __init__(self, name):
        self.name = name
    def __get__(self, instance, cls):
        return instance.__dict__[self.name]
    def __set__(self, instance, value):
        if not isinstance(value, str):
            raise TypeError('Expected a string value')
        instance.__dict__[self.name] = value
```

在上面的例子中，我创建了一个整数和字符串类，它使用内部机制(使用 set 和 get 方法)来截取“.”当我们访问一个实例的属性时。

现在我们的代码看起来像这样，

```
class Person(object):

    age = Integer('age')
    salary = Integer('salary')
    name = String('name')
    def __init__(self, name, age, address, salary):
        self.name = name
        self.age = age
        self.address = address
        self.salary = salaryperson1 = Person("Erling", '27', 'US', 1000)Traceback (most recent call last):File "<stdin>", line 1, in <module>File "<stdin>", line 7, in __init__File "<stdin>", line 8, in __set__TypeError: Expected an int value
```

使用 getter 和 setter 实现相同的功能，我们编写的代码量大大减少了。为了进一步改进这段代码，我们将使用继承来创建一个通用类型类，以重用 get 和 set 方法。

```
class Type(object):
    expected_type = object

    def __init__(self, name):
        self.name = name

    def __get__(self, instance, cls):
        return instance.__dict__[self.name]

    def __set__(self, instance, value):
        if not isinstance(value, int):
            raise TypeError('Expected{}'.format(self.expected_type))
        instance.__dict__[self.name] = value

class Integer(object):
    expected_type = int

class String(object):
    expected_type = str

class Float(object):
    expected_type = floatclass Person(object):

    age = Integer()
    salary = Integer()
    name = String()
    def __init__(self, name, age, address, salary):
        self.name = name
        self.age = age
        self.address = address
        self.salary = salary
```

当我们创建我们的框架时，这确实帮助了我们，我们可以扩展这个功能来拥有像 expected_type 这样的自定义对象，如果你愿意，就像一个小的类型系统。

现在到了问题的最后一部分，我们需要限制实例来创建新的属性。

似乎有一种更通用的方法来获取和设置属性，使用神奇的方法 __getattr__ 和 __setattr__。当我们通过实例调用属性时，它被 __getattr__ 截获，当我们设置属性时 __setattr__ 方法被调用。我们可以覆盖 __setattr__ 方法并在那里添加我们的检查。

```
person1 = Person('Rick', 65, 'US', 10)>>> person1.age65>>> person1.name‘Rick’>>> person1.__setattr__(‘age’, 66)>>>>>> person1.__dict__
{‘name’: ‘Rick’, ‘age’: 66, ‘address’: ‘US’, ‘salary’: 10}
```

所以现在我们的 Person 类是这样的。

```
class Person(object):
    age = Integer()
    salary = Integer()
    name = String()
    def __init__(self, name, age, address, salary):
        self.name = name
        self.age = age
        self.address = address
        self.salary = salary
    def __setattr__(self, key, value):
        if key not in {'name', 'age', 'address', 'salary'}:
            raise AttributeError("No attribute {}".format(key))>>> person1 = Person("Erling", 27, 'US', 1000)>>> person1.age = 21>>> person1.money = 1000Traceback (most recent call last):File "<stdin>", line 1, in <module>File "<stdin>", line 12, in __setattr__AttributeError: No attribute money
```

最后，我们现在可以限制实例来添加更多的属性。希望这些想法能在某种程度上帮助你用 python 创建库/框架，使代码更易维护。

[1]: Python 食谱:掌握 Python 的食谱 3 第 3 版

[https://www . Amazon . in/Python-Cookbook-Recipes-Mastering-ebook/DP/b 00 dq v4 ggy](https://www.amazon.in/Python-Cookbook-Recipes-Mastering-ebook/dp/B00DQV4GGY)