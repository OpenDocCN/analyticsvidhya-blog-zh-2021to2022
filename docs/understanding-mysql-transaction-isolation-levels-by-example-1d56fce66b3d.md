# 通过示例了解 MySQL 事务隔离级别

> 原文：<https://medium.com/analytics-vidhya/understanding-mysql-transaction-isolation-levels-by-example-1d56fce66b3d?source=collection_archive---------0----------------------->

![](img/d628afa89c31eeb83c480d3ec7e03ea4.png)

Georg Bommeli 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

大多数数据库系统使用 Read Committed 作为默认隔离级别(MySQL 使用 Repeatable Read 代替)。

选择隔离级别是为了找到当前应用程序需求的一致性和可伸缩性之间的平衡。

在本帖中，我们将深入探讨 MySQL 事务隔离级别。