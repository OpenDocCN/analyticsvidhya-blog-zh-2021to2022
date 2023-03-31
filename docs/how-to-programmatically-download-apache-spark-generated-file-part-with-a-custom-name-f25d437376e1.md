# 如何用自定义名称编程下载 Apache Spark 生成的文件(part*)？

> 原文：<https://medium.com/analytics-vidhya/how-to-programmatically-download-apache-spark-generated-file-part-with-a-custom-name-f25d437376e1?source=collection_archive---------6----------------------->

在端到端自动化过程时，一致的预定义文件名非常有用。

![](img/776cb59e5b27cbdac0199bfcb97af607.png)

照片由 [Cookie 在](https://unsplash.com/@cookiethepom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/pc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 Pom 拍摄

当数据帧保存为文件时，Apache Spark 会生成一组长文件和状态文件，以及一个以“part-000”开头的数据文件。许多开发人员被日志文件弄得不知所措，并想方设法避免它们…