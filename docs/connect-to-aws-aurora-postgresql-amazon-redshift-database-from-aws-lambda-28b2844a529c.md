# 从 AWS Lambda 连接到 AWS Aurora PostgreSQL/Amazon Redshift 数据库

> 原文：<https://medium.com/analytics-vidhya/connect-to-aws-aurora-postgresql-amazon-redshift-database-from-aws-lambda-28b2844a529c?source=collection_archive---------4----------------------->

![](img/a52d55aedc7db906b616baf05e27df5d.png)

图片由来自 Pixabay 的 Peggy und Marco Lachmann-Anke 提供

在这篇博文中，我将讨论以下从 AWS Lambda 函数连接到数据库的场景:

*   连接到私有子网 中的 Amazon Aurora PostgreSQL 数据库 ***，在同一个 AWS 帐户*** 中公共可访问性设置为否**。**
*   连接到公共子网 中的 ***交叉账户*** 亚马逊红移数据库 ***与公共…***