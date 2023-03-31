# 从无组织到有组织—数据的分区和索引

> 原文：<https://medium.com/analytics-vidhya/from-unorganized-to-organized-partitioning-and-indexing-of-data-8222a3d9196d?source=collection_archive---------12----------------------->

以下是在对数据表进行分区时想到的一些问题-

1.  当表中的数据被分区时，索引会发生什么变化？

默认情况下，当在已分区表上创建索引时，索引是使用该表的相同分区模式进行分区的。你可以——

I .为索引选择单独的分区方案和分区函数

(或)