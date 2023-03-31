# 如何从本地服务器启动、监控和完成 Databricks 作业管道。

> 原文：<https://medium.com/analytics-vidhya/how-to-start-monitor-and-complete-the-databricks-job-pipeline-from-your-local-server-35cf041f70ee?source=collection_archive---------12----------------------->

如果您有一系列必须在 Databricks 作业完成后完成的步骤，那么这篇文章就是为您准备的。这种编排类似于 Apache Airflow，但只使用 Bash 脚本来完成。

Databricks 提供了 REST API，可用于控制流程。

![](img/463ac0a9fcd57572eda85a67f7d54585.png)

由[约书亚·索蒂诺](https://unsplash.com/@sortino?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pipeline-data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片