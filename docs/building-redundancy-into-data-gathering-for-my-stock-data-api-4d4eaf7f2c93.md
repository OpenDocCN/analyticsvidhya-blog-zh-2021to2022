# 在我的股票数据 API 的数据收集中构建冗余

> 原文：<https://medium.com/analytics-vidhya/building-redundancy-into-data-gathering-for-my-stock-data-api-4d4eaf7f2c93?source=collection_archive---------16----------------------->

每个任务关键型系统都可以使用冗余级别来确保一切按预期运行。这篇文章概述了内置在我的股票数据 API 中的一个冗余级别。这里的想法可以扩展到任何具有类似危险的关键任务系统。

![](img/c7d4730da25fb0627833aba3f40b724d.png)

拉斯·金勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在以前的一篇文章中，我写了一个我创建的 API 来检索属于…