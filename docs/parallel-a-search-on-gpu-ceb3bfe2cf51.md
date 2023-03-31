# GPU 上的并行 A*搜索

> 原文：<https://medium.com/analytics-vidhya/parallel-a-search-on-gpu-ceb3bfe2cf51?source=collection_archive---------4----------------------->

## A*搜索算法的并行变体，其中代理的搜索过程可以由 GPU 大规模并行化

搜索是人工智能中的一个基本主题。在本文中，让我们看看如何在图形处理单元(GPU)上并行实现这个了不起的算法。

![](img/792dc2f691cfe61d1b982896617a6d69.png)

图片来自[https://www . thebluediamonggallery . com/typewriter/a/artificial-intelligence . html](https://www.thebluediamondgallery.com/typewriter/a/artificial-intelligence.html)