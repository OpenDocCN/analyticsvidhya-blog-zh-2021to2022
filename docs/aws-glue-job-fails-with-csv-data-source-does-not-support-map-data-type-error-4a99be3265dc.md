# AWS 粘合作业失败，CSV 数据源不支持地图数据类型错误

> 原文：<https://medium.com/analytics-vidhya/aws-glue-job-fails-with-csv-data-source-does-not-support-map-data-type-error-4a99be3265dc?source=collection_archive---------8----------------------->

![](img/f3f826c31657144a20bc8d11dbb5d6f0.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1424831) 的 [GraphicMama-team](https://pixabay.com/users/graphicmama-team-2641041/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1424831)

AWS Glue 是一个无服务器的 ETL 服务，用于处理来自各种来源的大量数据集，以进行分析和数据处理。最近，我遇到了一个新创建的胶合工作“CSV 数据源不支持地图数据类型”的错误。简而言之，工作按以下步骤进行:

1.  使用…从 S3 读取数据