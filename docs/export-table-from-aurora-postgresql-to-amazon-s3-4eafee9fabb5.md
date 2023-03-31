# 将表从 Aurora PostgreSQL 导出到亚马逊 S3

> 原文：<https://medium.com/analytics-vidhya/export-table-from-aurora-postgresql-to-amazon-s3-4eafee9fabb5?source=collection_archive---------1----------------------->

![](img/89150b1dc01ec914b51e9d48332528dc.png)

图片由[蒂姆·希尔](https://pixabay.com/users/timhill-5727184/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2443085)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2443085)

在这篇博文中，我将讨论如何将 100GB 的未分区表从 Aurora PostgreSQL 导出到亚马逊 S3。我将向您介绍两种可以用来导出数据的方法。首先，我将演示如何使用 Aurora PostgreSQL 提供的 PostgreSQL 扩展`aws_s3`，然后使用`AWS Glue`服务。这篇文章还介绍了使用 AWS 导出表格时的性能和伸缩挑战…