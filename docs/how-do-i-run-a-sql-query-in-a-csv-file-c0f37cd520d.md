# 如何在 CSV 文件中运行 SQL 查询？

> 原文：<https://medium.com/analytics-vidhya/how-do-i-run-a-sql-query-in-a-csv-file-c0f37cd520d?source=collection_archive---------13----------------------->

如果您的查询非常简单，请使用 csvsql 或 q 命令行工具，它们允许在 CSVs(和任何其他表格文本文件)上直接执行类似 sql 的查询。如果您想要执行更复杂的查询，甚至想要直接查询 excel 文件，请尝试 esProc。

# 如何使用

# 在 esProc 中执行 SQL

您可以轻松地将 SQL 查询结果导出到新的 Excel 文件或文本文件中。

![](img/d1162e7a7799b4e4424e4f8f33e5537a.png)![](img/eee87fb7c59118ca2777ba3968e2acb9.png)

# 在命令行执行 SQL

对于 Windows:

`esprocx -r select state, sum(amount) as sum_amount from d:/excel/orders.xlsx group by state`

![](img/91240f26238b20405720b6a4d0f0d73e.png)

对于 Linux:

`esprocx -r select state, sum(amount) as sum_amount from d:/excel/orders.xlsx group by state`

# 例子

esProc 支援 SQL92 标准中的大部分语法。

![](img/2947056b73a1bea5a253d3f8dc926e5a.png)![](img/7d97fd3437aada6ab5c4e8f449324429.png)

对于更复杂的场景，请参考[如何在 esProc 中使用 SQL](http://c.raqsoft.com/article/1603680137640)