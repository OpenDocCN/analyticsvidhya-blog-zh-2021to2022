# 如何在 SQL 中从另一个现有表创建一个表？

> 原文：<https://medium.com/analytics-vidhya/how-to-create-one-table-from-another-existing-table-in-sql-bccf73656a1c?source=collection_archive---------2----------------------->

这是面试中最常被问到的问题之一。让我们把它看作一个问题陈述，并尝试找到一些方法来解决这个问题。

![](img/a717da9458374659d82572b8a14d4d7a.png)

**问题陈述:**用从另一个表复制的结构和数据创建一个新表。

我们的数据库有一个名为 employee 的表，包含以下列 name、mgrname 和 doj。

![](img/e9dcb742c6295c5c82a6677a5f947dba.png)

SELECT * FROM 员工；

我们的目标是创建一个名为 dev_emp 的新表，它将具有与 employee 表相同的结构(与我们之前的表一样),但是将具有基于条件的数据。

表 dev_emp 只存储那些经理姓名(名为 mgrname 的列)为 ankit 的雇员的姓名。

**解决方案 1:创建表作为 SELECT** 结构

为了从现有的表中创建一个新表，我们将使用 **CREATE TABLE 作为 SELECT。**

> 将表 dev_emp
> 创建为 SELECT *
> FROM employee，其中 mgrname = ' ankit

![](img/e7e0da93d6f40dc26f83ae82e09b3087.png)

SELECT * FROM dev _ emp

**解决方案 2:选择进入**结构

为了从现有的表中创建一个新表，我们将使用 **SELECT INTO。**

> SELECT
> * INTO dev _ EMP
> FROM employee where mgr name = ' ankit '；

![](img/e7e0da93d6f40dc26f83ae82e09b3087.png)

SELECT * FROM dev _ emp

**结论:**

为了基于另一个现有表的结构和数据创建一个新表，我们可以使用 **SELECT INTO** 和**CREATE TABLE AS****SELECT**子句。

这些复制结构和数据的小技巧有时会非常有用。

如果发现有用，就鼓掌🙂。请分享您在数据库中使用的知识和技巧。

**“不断学习，不断分享知识”**