# 从 AWS 粘合作业在 AWS 粘合数据目录中添加新分区

> 原文：<https://medium.com/analytics-vidhya/add-new-partitions-in-aws-glue-data-catalog-from-aws-glue-job-79b0442b17af?source=collection_archive---------4----------------------->

假设在 AWS 粘合数据目录中有一个分区表，那么有几种方法可以用新创建的分区更新粘合数据目录。

1.  运行 MSCK 修理表<database>。<table_name>在 AWS 雅典娜服务。</table_name></database>
2.  重新运行 AWS 胶水爬行器。

最近，AWS Glue service 团队增加了一个新功能(或者说 Glue job 的参数),使用它您可以立即查看 Glue Data Catalog 中新创建的分区。