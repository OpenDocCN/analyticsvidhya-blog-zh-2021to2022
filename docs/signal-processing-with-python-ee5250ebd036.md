# 用 Python 进行科学数据分析的信号处理:第 3 部分

> 原文：<https://medium.com/analytics-vidhya/signal-processing-with-python-ee5250ebd036?source=collection_archive---------2----------------------->

## 去除异常值的中值滤波器

![](img/4a02f2db2bf5a9aed30bcde0136681cd.png)

图片由[阿尔图纳·阿卡林](https://al2na.co/)

在使用 Python 进行信号处理的第三部分中，我将讨论如何使用中值滤波器去除大的尖峰信号。下面是 [**第一个**](/analytics-vidhya/signal-data-processing-for-scientific-data-analysis-with-python-part-1-90a90cb7f81) 和 [**第二个**](/analytics-vidhya/signal-processing-time-series-analysis-for-scientific-data-analysis-with-python-part-2-bd263fdf8196) 部分的链接。首先，我们再澄清一次，一个数列的均值和与中位数的区别是什么。