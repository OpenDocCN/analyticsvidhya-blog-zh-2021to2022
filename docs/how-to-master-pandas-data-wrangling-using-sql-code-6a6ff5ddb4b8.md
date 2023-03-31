# 如何使用 SQL 代码掌握熊猫数据争论

> 原文：<https://medium.com/analytics-vidhya/how-to-master-pandas-data-wrangling-using-sql-code-6a6ff5ddb4b8?source=collection_archive---------6----------------------->

在这个故事中，我假设你是一个人专家/熟悉 SQL，并了解一点 python。

**简介**

SQL 是一种从数据库中提取信息的编程语言。而且 SQL 在 python 和 r 之前就以分析著称，Python 是一种编程语言。它可以做很多事情。python 的能力之一是使用 pandas 进行数据处理。pandassql 是您可以在 panda 内部使用 sql 查询的地方。

先安装 panda SQL【https://pypi.org/project/pandasql/】T3
和

![](img/52a4185c4d8c3e4c28f66085bdaba7de.png)

让我们编码

1.  导入一些数据集和库设置

![](img/d100bf1295ed3c0fd9be0284b1eabda4.png)

2.图像数据

使用 sql 中的“select”来浏览数据和。head()限制熊猫的搜索结果。

![](img/2b46c5f3e96b660089b09bc5be9f8e5c.png)

3.过滤数据

使用 sql 中的“where”来过滤数据。

![](img/1fe2f9f553b8672c651dc86908b5afce.png)

4.日期过滤器

使用“strftime”过滤 sql 中的数据。

![](img/829f1e9650a5bbc3fb8e15e1c9cda45e.png)

5.数据聚合

对聚合数据使用 group by，对结果进行排序。

![](img/8c0888973edd29ae16f8376a44d45329.png)![](img/c71d75a28ce9b52b9f090b3bc3c14339.png)![](img/d39779e37e90030f24c3ca2e83d411a7.png)

6.行号

行号函数在分组值内执行排序过程。

![](img/3c02e4d0d6b9ecc6e2b8799f308a2881.png)

7.加入

![](img/a56d20413d706027a09c709f6701f322.png)![](img/28b9ff16c727967533bd84aca2dbfeac.png)

完整代码[此处](https://github.com/adamaulia/pandasql_tutorial)

结论，
我希望这篇简短的教程能帮助你理解在 pandas 中使用 sql 代码的数据争论。