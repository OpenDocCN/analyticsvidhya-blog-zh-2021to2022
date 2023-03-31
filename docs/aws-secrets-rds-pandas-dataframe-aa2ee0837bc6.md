# AWS 秘密->RDS ->熊猫数据帧

> 原文：<https://medium.com/analytics-vidhya/aws-secrets-rds-pandas-dataframe-aa2ee0837bc6?source=collection_archive---------11----------------------->

## 如何通过 Python 连接 PostgreSQL/Couchbase 数据库？

当你只想从数据库里找到一个熊猫的数据框时，那真是有趣的时光…

我写了一个类，教你如何用几行代码运行查询并将其转换成熊猫数据帧！

**步骤:**

1.  从 AWS 机密中提取数据库凭据
2.  连接到数据库
3.  运行查询
4.  将 JSON 转储结果转换为 Pandas 数据帧