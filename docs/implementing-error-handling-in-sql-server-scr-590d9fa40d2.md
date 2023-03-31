# 在 SQL Server 脚本中实现错误处理

> 原文：<https://medium.com/analytics-vidhya/implementing-error-handling-in-sql-server-scr-590d9fa40d2?source=collection_archive---------16----------------------->

**简介**

正如 C++和 Java 等编程语言一样，SQL Server 也有自己的“错误处理”机制。什么是错误处理？错误处理通常用于通过一些操作来处理错误消息，以便 SQL 脚本继续运行而不会中途停止。当我们需要运行不同的存储过程来填充报告时，错误处理对于生产数据库非常重要。正如我们在 OOP 语言中有著名的“Try catch except”块一样，SQL Server 提供了两个选项