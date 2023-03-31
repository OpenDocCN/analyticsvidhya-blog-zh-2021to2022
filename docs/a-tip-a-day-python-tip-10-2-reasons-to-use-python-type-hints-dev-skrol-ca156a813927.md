# 一天一个技巧——Python 技巧#10:使用 Python 类型提示的 2 个理由

> 原文：<https://medium.com/analytics-vidhya/a-tip-a-day-python-tip-10-2-reasons-to-use-python-type-hints-dev-skrol-ca156a813927?source=collection_archive---------6----------------------->

![](img/5b2e5373fd1df3377295d96c129339e1.png)

类型提示用于说明变量的预期数据类型。

即使在 Python 3.6 之前，开发人员也在注释中提供了首选数据类型。以便在进一步的增强中，同一变量不能用于不同的数据类型。

Python 3.6 之后，引入了类型提示。

# 为什么我们需要使用类型提示？

# 原因 1:

**类型提示可以用来提及变量的数据类型。**

**语法:**

```
<variable_name>: <expected data type>
```

**举例:**

```
amount: int = 1000
```

我们也可以只使用类型提示来声明一个变量，甚至不用定义值。

**举例:**

```
total_amount: int 
sales_1 = 100 
sales_2 = 200 
total_amount = sales_1 + sales_2
```

类型提示永远不会强制变量的数据类型。类型提示仅供用户参考。

因此，在以后的版本中，相同变量的使用将由不同的开发人员来处理，这将使您对变量的期望值有所了解。

这将减少运行时错误，如字符串值与“*”运算符一起用于乘法时的“类型错误”。

这只是一个提示，当一个整数提示变量被赋予字符串值时，在运行时不会产生错误来强制数据类型。

# 原因 2:

类型提示在函数中也非常有用。
这些帮助开发者知道参数的预期数据类型。
例如，在下面的例子中，参数必须是一个数字。

```
# Both the functions expect only numbers and not string valuesdef multiply(a, b):return a * bdef addition(a, b):return a + bprint(addition(5,6))print(addition("5","6"))
```

11
56

`print(multiply(5,6))`

`print(multiply("5","6"))`

```
30
**---------------------------------------------------------------------------**
**TypeError**                                 Traceback (most recent call last)
**<ipython-input-8-051e65134284>** in <module>
      1 print**(**multiply**(5,6))**
**----> 2** print**(**multiply**("5","6"))****<ipython-input-7-9af0c4484b3c>** in multiply**(a, b)**
      1 **def** multiply**(**a**,** b**):**
**----> 2     return** a ***** b**TypeError**: can't multiply sequence by non-int of type 'str'
```

在上面的例子中，参数的预期数据类型是整数。
但是传递了字符串值。
现在我们将为参数设置类型提示。

```
def addition(a: int, b: int) -> int:return a + bdef multiply(a: int, b: int) -> int:return a * bprint(addition(5,6))print(addition("5","6"))print(multiply(5,6))print(multiply("5","6"))
```

输出:

```
11
56
30 **---------------------------------------------------------------------------**
 TypeError                                 Traceback (most recent call last)
  in 
       1 print(multiply(5,6))
 ----> 2 print(multiply("5","6"))
  in multiply(a, b)
       1 def multiply(a: int, b: int):
 ----> 2     return a * b
       3 
       4 def addition(a: int, b: int):
       5     return a + b
 TypeError: can't multiply sequence by non-int of type 'str'
```

如您所见，结果没有变化。
类型提示只能通过提供关于变量预期类型的额外信息来帮助开发人员。这里，据说只发送整数值。
它不会强制编译器只允许指定的数据类型。
Python 的动态类型仍然有效。

我希望你喜欢学习类型提示。

让我们在未来的技巧中探索更多关于 Python 的内容。

# 更多有趣的 Python 技巧:

[一天一个技巧——Python 技巧 9:关于格式{}的 3 件趣事](https://devskrol.com/index.php/2021/09/03/a-tip-a-day-python-tip-9-3-interesting-things-about-format/)

[一天一个提示——Python 提示# 6——熊猫合并](https://devskrol.com/index.php/2020/10/25/a-tip-a-day-python-tip-6-pandas-merge/)

[一天一个提示— Python 提示#5 —熊猫串联&追加](https://devskrol.com/index.php/2020/10/20/a-tip-a-day-python-tip-5-pandas-concat-append/)

[所有其他提示](https://devskrol.com/index.php/category/python-tips/)

# 其他有趣的概念:

[SpaCy Vs NLTK —基本 NLP 操作代码和结果比较](https://devskrol.com/index.php/2021/04/17/spacy-vs-nltk-basic-nlp-operations-code-and-result-comparison/)

[在组内估算 NAN 的最佳方式—均值&模式](https://devskrol.com/index.php/2020/08/09/best-way-to-impute-nan-within-groups-mean-mode/)

*原载于 2021 年 9 月 11 日 https://devskrol.com*[](https://devskrol.com/index.php/2021/09/11/a-tip-a-day-python-tip-10-2-reasons-to-use-python-type-hints/)**。**