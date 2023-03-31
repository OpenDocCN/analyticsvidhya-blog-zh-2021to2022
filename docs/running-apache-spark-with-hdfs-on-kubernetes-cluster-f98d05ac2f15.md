# 在 Kubernetes 集群上与 HDFS 一起运行 Apache Spark

> 原文：<https://medium.com/analytics-vidhya/running-apache-spark-with-hdfs-on-kubernetes-cluster-f98d05ac2f15?source=collection_archive---------3----------------------->

![](img/b99139db0a832909e646093a00df404c.png)

# 为什么在 Kubernetes 上？

从 *Kubernetes* 提供的众多优势中，我特别发现在 *K8s* 上部署 *HDFS* 是有益的，因为**易于扩展** K8s 提供并需要更少的管理任务。数据在不断增长，人们需要一个稳定且简单的水平可扩展架构来容纳数 TB 的数据，同时提供…