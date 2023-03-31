# 如何使用 AWS Glue 将 JSONB 数据类型的表加载到 Aurora PostgreSQL 中

> 原文：<https://medium.com/analytics-vidhya/how-to-load-table-with-jsonb-data-type-into-aurora-postgresql-using-aws-glue-aebbb11ac962?source=collection_archive---------2----------------------->

![](img/8dc4c92bd0341ac6834beae79db80c47.png)

[南安](https://unsplash.com/@bepnamanh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/elephant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在这篇博文中，我将介绍什么是 JSON 数据类型，PostgreSQL 提供了哪些选项来存储 JSON 数据，如何创建 AWS Glue 连接到在私有子网中运行的 Aurora PostgreSQL 数据库，以及如何使用 AWS Glue 将 JSONB 数据类型的数据写入 Aurora/RDS PostgreSQL 数据库。