# 如何在 Python 中处理日期和时间列|特征工程|机器学习

> 原文：<https://medium.com/analytics-vidhya/how-to-handle-date-and-time-columns-in-python-feature-engineering-machine-learning-254e45a031ae?source=collection_archive---------3----------------------->

我们将讨论一些使用这些列的有用函数。

我们还将学习如何派生新列

**用例-** 当您想要分析费用跟踪时，数据和时间字段起着主要作用。

1997 年 1 月 22 日——从这个单一的日期，我们可以提取几个其他列，如

1.第二天。年份，3。月份，4。周末，5。星期几，6。四分之一，7。半个季度，8。日名称，9。日、月、秒、年等的差异。

09:12:05-从这个时间，我们可以分别提取秒、分和小时。

现在让我们看看如何用代码实现这一点

![](img/f642269e4ef1ff809460b3b2dfa84e91.png)

加载数据

1.  加载数据
2.  检查日期列的数据类型

![](img/f383eee1b3cabe2be04c9a00b387d209.png)

数据类型

大多数超时日期时间列将属于**对象类型**。

我们需要将它转换成**日期时间对象**，这样我们就可以对它使用不同的方法。

3

![](img/57186b5d21c26c9c3b4517baba919cc7.png)

现在我们已经改变了数据类型。

现在，我们将使用不同的方法提取不同的有用列进行分析

**4a。提取年份**

![](img/f451ab708832d14125451349c77f4bb5.png)

**4b。提取月份**

![](img/82940abbba2e29fc658d3b12e8d34375.png)

**4c。提取月份名称**

![](img/e1b030ccef06f0ddce2e5290aeeb3a77.png)

**4d。提取日**

![](img/6533cfa9ee27ca09ac28f9c34311687e.png)

**4e。提取星期几**

![](img/539feade6e39f8ce07d50386a09d1921.png)

**4f。提取星期几名称**

![](img/24bb1d277234430dd7af83a7d15924b9.png)

**4g。检查当天是否是周末**

![](img/2d05d75e8e4394f1dd43936cc9548bc0.png)

**4h。提取一年中的一周**

![](img/d1a807e416f41ecb5ebfdfca7c48ca8c.png)

4i。提取季度

![](img/e7949c8e75dedea8e4c8a02f22e77487.png)

**4j。摘录学期**

![](img/648f4ee4e8e46ecd48ffeb52d5266b5e.png)

**5a。提取日期之间经过的时间**

![](img/55493fae819cddcd36cbde7a41d6e46d.png)

**5b。计算天数差异**

![](img/9db0978285c6681cbe19d57075fdf680.png)

**5c。计算月差**

![](img/aabde96ea70044e07bf5cbae20c86bd2.png)

# **只用时间工作**

**6a。提取小时、分钟、秒**

![](img/98562c0a1d1101ef63b7706aff5ede98.png)

**6b。提取时间部分**

![](img/fdeff3db3676ede38c35cb31549bade4.png)

**6c。时差**

![](img/30a0541830a105fe1054419cc9f76f81.png)

**6d。以秒为单位的时间差**

![](img/054a4d37d9d53011495c36785001020a.png)

**6e。以分钟为单位的时差**

![](img/89e78a728a4abc422c2ee9e3966b8a9d.png)

**6f。以小时为单位的时差**

![](img/0a8d1bd0280feaa10fad12cf1432fa10.png)

当您处理日期和时间列时，这将非常方便。

希望这对你有用。