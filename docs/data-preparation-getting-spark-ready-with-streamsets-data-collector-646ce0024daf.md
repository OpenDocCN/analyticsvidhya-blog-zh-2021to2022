# 数据准备:使用流集数据收集器准备 spark

> 原文：<https://medium.com/analytics-vidhya/data-preparation-getting-spark-ready-with-streamsets-data-collector-646ce0024daf?source=collection_archive---------17----------------------->

Spark 可以分析以多种不同格式存储在文件中的数据:纯文本、JSON、XML、Parquet 等等。但是，仅仅因为您可以让 Spark 作业在给定的数据输入格式上运行，并不意味着您可以获得与所有这些相同的性能。实际上，性能差异可能相当大。

当您为 Spark 应用程序设计数据集时，您需要确保充分利用 Spark 可用的文件格式。

# Spark 文件格式…