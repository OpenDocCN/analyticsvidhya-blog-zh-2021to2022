# 使用 Python 进行交互式 Azure 广告认证

> 原文：<https://medium.com/analytics-vidhya/interactive-azure-ad-authentication-with-python-64df3173a81c?source=collection_archive---------3----------------------->

新年快乐希望每个人都有一个伟大和健康的一年。一月一日是新的一天，新的一天有新的挑战。最近在用 [Azure Databricks](https://docs.microsoft.com/en-us/azure/databricks/scenarios/what-is-azure-databricks) 做数据工程、科学和机器学习平台。一个有趣的挑战是关于从[data bricks Notebook](https://docs.microsoft.com/en-us/azure/databricks/notebooks/)(Python)到 [Azure SQL 数据库](https://docs.microsoft.com/en-us/azure/azure-sql/database/sql-database-paas-overview)的认证方法。

从官方[文档](https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/sql-databases)来看，最简单的方法是使用 PySpark 和 SQL 用户 ID 和密码通过 JDBC 驱动进行认证。Azure SQL 数据库也受支持 [Azure AD](https://docs.microsoft.com/en-us/azure/azure-sql/database/logins-create-manage) (用户、组…