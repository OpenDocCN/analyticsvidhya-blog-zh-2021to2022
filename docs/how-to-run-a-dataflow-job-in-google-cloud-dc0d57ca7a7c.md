# 如何在谷歌云中运行数据流作业

> 原文：<https://medium.com/analytics-vidhya/how-to-run-a-dataflow-job-in-google-cloud-dc0d57ca7a7c?source=collection_archive---------7----------------------->

![](img/5a9b674b26736b8520fd621d5a9c8c56.png)

图片来自[https://unsplash.com/@qixna](https://unsplash.com/@qixna)

Google Cloud Dataflow 是一个完全托管的服务，用于在 Google Cloud Platform 生态系统中执行 Apache Beam 管道。这些管道可以是流管道或批处理管道，在下面的例子中，我解释了如何使用数据流在谷歌云中运行批处理管道。

Apache Beam 是运行流和批处理数据管道的统一编程模型。管道运行器可以…