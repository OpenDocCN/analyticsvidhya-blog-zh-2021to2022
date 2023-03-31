# Python 变量和常量

> 原文：<https://medium.com/analytics-vidhya/python-variables-and-constants-307cc7834178?source=collection_archive---------2----------------------->

![](img/6a687b49aecb13cf90d0a76b14dbecca.png)

# Python 变量

变量是一个命名的位置，用于在内存中存储数据。它基本上是一个保存数据的容器，这些数据可以在程序中使用和修改。

如果你还是不明白变量是什么，那也没关系！只要看看这个例子，继续前进！

```
myNumber = 15
```

在这里，我创建了一个变量，它的名字是“myNumber”。然后，我使用赋值操作符“=”将值“15”赋给变量。

![](img/7cb673ec38e9a988568213546ac1a992.png)

好吧，我这么解释吧！

假设变量是一个盒子，你可以在里面放一支铅笔，而且那支铅笔可以随时更换！

```
myNumber = 15
myNumber = 17.5
```

一开始，我赋予“我的号码”的值是“15”。后来我改成了“17.5”。这就是你在 Python 中使用变量的方式。小菜一碟！

![](img/6f778dc5c1db00465e7f694f4c476665.png)

现在，让我们看看如何在 Python 中给变量赋值。

正如我之前提到的，我们可以使用“赋值操作符 **=** ”给变量赋值，就像你在前面的例子中看到的那样。

## 为变量声明和赋值

```
myName = "Scooby-Doo"
print(myName)
```

我写了一个简单的程序，声明了一个名为“myName”的变量，并给它赋值“Scooby-Doo”。然后，使用“print”关键字，我将获得变量“myName”的值作为输出。当我执行并运行这个程序时，输出如下。

**输出**

```
Scooby-Doo
```

![](img/8a838439457dc086b02b6ac0a1c6bda8.png)

> “Python 是一种 [**类型推断**](https://en.wikipedia.org/wiki/Type_inference) 语言，所以你不必显式定义变量类型。它自动知道“Scooby-Doo”是一个字符串，并将“myName”变量声明为一个字符串。

## 更改变量的值

```
# initial assignment
myName = "Scooby-Doo"
print(myName)# Cahnging the value of myName
myName = "Scrappy-Doo"
print(myName)
```

在这里，我已经初步将“Scooby-Doo”赋给了变量“myName”。但是，后来我改变了它，将“Scrappy-Doo”赋给了变量“myName”。花几秒钟时间，试着理解输出。

是啊！你答对了！

**输出**

```
Scooby-Doo
Scrappy-Doo
```

![](img/97e8be97378ae1852b8421152262288a.png)

## 将多个值赋给多个变量

“Python 很容易”是事实！为了证明这一点，我给你看些有趣的东西。

如果您想仅使用一行代码将多个值赋给多个变量，您可以在 Python 中使用以下方法。

```
myName, myAge, myBestie = "Scooby-Doo", 5, "Shaggy"
```

另外，如果你想给多个变量赋予相同的值，你可以这样做。

```
myLastName, myMothersLastName, myFathersLastName = "Doo"
```

这就是你应该知道的关于 Python 变量的全部内容！

# Python 常量

在 Python 中，常量通常在模块中声明和赋值。

等等，什么！一个模块？那是什么？

![](img/d8acd7de5a5f2a4c914346fbf471fdec.png)

我给你说清楚！

这个模块只是一个包含变量、函数等的新文件，它被导入到主文件中。放心吧！一会儿你就知道是怎么做的了！

> 在模块内部，常量全部用大写字母书写，并且用下划线分隔单词。

## **声明常量并赋值**

第一步:创建一个 **constant.py**

```
PI = 3.14
TAU = 6.28
```

第二步:创建一个 **main.py**

```
import constantprint(constant.PI)
print(constant.TAU)
```

在上面的程序中，我已经创建了一个 **constant.py** 模块文件，并为 PI 和 TAU 赋值。然后我创建了一个 **main.py** 文件，并使用代码顶部的“导入常量”语句导入常量模块。

现在，我可以在 main.py 文件中调用我在 constant.py 模块中声明的变量了！就像我在例子中展示的那样。输出将会如下所示。

```
3.14
6.28
```

这就是 Python 的妙处！

# 变量和常量的规则和命名约定

*   常量和变量的名称应该是小写字母(A 到 Z)或大写字母(A 到 Z)或数字(0 到 9)或下划线(_)的组合。

```
camelCase
CapWords
snake_case
MACRO_CASE
```

*   如果你能创造一个有意义的名字，这是一个很好的实践。例如，如果要存储一个名字，“name1”比“n1”更有意义。
*   如果您想创建一个有两个单词的变量名，最好使用“snake_case”类型(使用下划线分隔单词)。

```
first_name
second_name
```

*   使用大写字母来声明常数。

```
TEMP
SPEED_OF_LIGHT
MASS
```

*   不要使用特殊符号，如！、@、#、$、%等。
*   不要以数字作为变量名的开头。

这就是关于 Python 变量和常量的内容。希望这篇文章能帮助你学到一些新东西！永远记住，开始做那些能提高你知识的事情永远都不晚！