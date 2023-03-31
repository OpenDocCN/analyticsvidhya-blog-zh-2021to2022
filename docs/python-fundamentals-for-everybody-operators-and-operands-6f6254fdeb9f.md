# 面向所有人的 Python 基础——运算符和操作数

> 原文：<https://medium.com/analytics-vidhya/python-fundamentals-for-everybody-operators-and-operands-6f6254fdeb9f?source=collection_archive---------16----------------------->

![](img/b72263df589991dce1bc2fc271335f68.png)

照片由[在](https://unsplash.com/@freetousesoundscom?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上免费使用声音

这是关于每个人的 **python 基础**的第三篇文章，这是一个专注于 Python 基础的 Python 教程系列。

你可以参考下面这个系列的前一篇文章。

[](https://snehathomasmathew.medium.com/python-fundamentals-for-everybody-variables-b76fd2699b20) [## 面向所有人的 Python 基础——变量

### 这是面向所有人的 python 基础系列文章的第二篇，这是一个 Python 教程系列，主要关注…

snehathomasmathew.medium.com](https://snehathomasmathew.medium.com/python-fundamentals-for-everybody-variables-b76fd2699b20) 

在这篇文章的结尾，你将会知道什么是操作符，操作数以及我们如何使用它。

## **运算符和操作数**

运算符基本上是一种符号，如+、-、>、< etc., which are used to perform operations on values and variables. These values and variables are known as operands.

Operators are used to manipulate the operands.

For example: In this example, ‘a’ and ‘b’ are operands where as ‘+’ is the operator.

![](img/7302d0605d4fe7baeca5d857f0e9259c.png)

Image by author

Python supports the following Operators:

1.  Arithmetic Operators
2.  Comparison (Relational) Operators
3.  Assignment Operators
4.  Logical Operators
5.  Bitwise Operators
6.  Membership Operators
7.  Identity Operators

Let us look into each of these one by one along with examples.

> Arithmetic Operators

They are used to perform mathematical operations like addition, subtraction, multiplication etc.

Assume a = 2 and b = 3, and go through the following two images with example to understand better.

![](img/1208595828c14f3cd77d16cdd0a1c1c3.png)

Image by author

![](img/6f911fccf02d7581e6eafdb049b6defa.png)

Image by author

> Comparison (Relational) Operators

These operators are used to compare the operand values. It returns True or False depending upon the condition. They are also called as relational operators.

Assume a = 10 and b = 20, and go through the following two images with example to understand better.

![](img/ede14d64781cb56bf65557bf7a0892bd.png)

Image by author

![](img/96586ac071f9c23556670c23206837e7.png)

Image by author

> Assignment Operators

Assignment operators (symbol ‘=’) are operators used to assign values to the variables, in a program.

Assume a = 10 and b = 20, and go through the following two images with example to understand better.

![](img/d707c6d2e0b36a009af0ffe02be9b319.png)

Image by author

![](img/51f738f1282dce39c8cbc5ed1ce8f202.png)

Image by author

> Logical Operators

They are operators which are used to control the flow of the program. Most often, logical operators are combined with Boolean expressions. Sometimes, Logical Operators are also called as Boolean Operators. It holds the value of 0 or 1, where 0 means False and 1 means True.

*布尔/逻辑表达式是计算结果为真或假的表达式。*

假设以下示例中 a =假，b =真

![](img/98d2de423c15ec2f8ead57f02bb27a3c.png)

作者图片

![](img/2e06e25406f0c0d55f691a8f6cc68138.png)

作者图片

> 按位运算符

按位运算符类似于逻辑运算符，但它直接对位进行操作。

![](img/44df6d7955131fb17a039e7f3466f4a5.png)

作者图片

![](img/ee410a3fedb133401089a7e1257faab0.png)

作者图片

在上面的例子中，a << 2 is 40, since the binary representation of a = 10 is 00001010(2), which when we shift left two spaces, and fill up the empty spots with zeroes, we get 00101000(2) = 40(10).

The right shift operator works the opposite way, the value of a > > 2 的值是 2，因为 a = 10 的二进制表示是 1010(2)，当我们通过用零填充值来右移两个空格时，最右边的两位会下降。因此，我们得到 00000010(2) = 2。

> 成员运算符

Python 有两个成员操作符，分别是:中的**和【非】中的**。****

这些运算符用于测试值或变量是否存在。它适用于字符串、元组、列表和字典。

![](img/89c061f7088cf30d75680ad05b68605d.png)

作者图片

![](img/234c270b3350bb8edacba5456d1d9bae.png)

作者图片

> 恒等运算符

恒等运算符用于比较对象的内存位置。Python 有两个恒等运算符，即**是**和**不是**。

> 成员操作符用于检查列表中的内容等。，其中 as 恒等式运算符比较内存位置。

![](img/a102cfe3832378c2d097bc55ad3f21da.png)

作者图片

![](img/e9c14cd5271333f85e24db32715a399b.png)

作者图片

> 唷！那是相当多的。既然您已经到了这里，不妨去阅读一下运算符优先级和结合性。

> **Python 运算符优先级&结合律**

如果表达式是单一的，那么在评估解时不存在问题，但如果有多个操作呢？

在大多数情况下，当表达式是混合的，即包含几个操作时，Python 如何知道先执行哪个操作呢？

> 操作符的优先级和结合性，帮助 python 决定操作符的优先级。

![](img/3f259469b211c32a3103b44d5b8310a5.png)

作者图片

现在，您已经了解了各种类型的 are Python 操作符及其优先级。

在下一篇文章中，我们将看看类型转换和类型强制之间的区别。