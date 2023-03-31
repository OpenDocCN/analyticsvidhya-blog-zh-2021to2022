# 以秒为单位获取配置单元计数

> 原文：<https://medium.com/analytics-vidhya/get-hive-count-in-seconds-7efcb07c65d8?source=collection_archive---------11----------------------->

![](img/48c41d7e87490d44c7cfd13203cf1c75.png)

要想得到数据量的准确计数，在不能少也不能多的情况下，`COUNT(*)`是唯一的办法。但是有些时候你不需要一个确切的数字，但是你需要对表的大小有一个大概的估计，比如了解表不是空的，或者大概估计要迁移的数据的大小。这类任务有比`COUNT(*)`更快的方法。我们可以使用蜂巢统计。