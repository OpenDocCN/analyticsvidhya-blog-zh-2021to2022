# PySpark CheatSheet 及更多

> 原文：<https://medium.com/analytics-vidhya/pyspark-cheatsheet-and-more-a41b338c9d3e?source=collection_archive---------0----------------------->

![](img/4d9a4f66bf0a7383c1da6c10f2e54877.png)

安娜·克鲁兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

Apache Spark 和 Apache Hadoop 都是大数据处理的开源框架。对于大规模数据处理，Spark 可以比 Hadoop 快 100 倍，但是，Hadoop 有分布式文件系统(HDFS)，而 Spark 是建立在弹性分布式数据集( **RDDs** )上的。通常，我们在 Hadoop 之上使用 Spark 进行数据计算。

在本文中，我们将重点关注 **RDD** 的数据操作。

所有代码都可以在 [github](https://github.com/chiang9/Medium_blog/blob/main/pyspark/spark_cheatsheet.ipynb) 上找到。

> 数据集:
> 
> 我们将使用 [movielens](https://grouplens.org/datasets/movielens/) 数据集(ml-100k)进行演示。
> 
> Datacamp 还为 pyspark 提供了一个清晰的备忘单。
> 
> 更多信息:[https://www . data camp . com/community/blog/py spark-cheat-sheet-python](https://www.datacamp.com/community/blog/pyspark-cheat-sheet-python)

# 1.初始化火花环境

每当我们想要使用 Spark 引擎时，我们需要首先初始化 Spark 环境。

## 火花背景

驱动程序将使用 SparkContext 与集群进行连接和通信，并与资源管理系统协调作业。

## 火花会议

从 Spark 2.0 开始，SparkSession 构建了不同数据源的网关，比如 SQL 或 Hive。在 Spark 1.x 中，我们需要定义 SQLContext 或 HiveContext 来与相应的源进行通信。

*简而言之，如果我们想要使用 Spark 数据帧或数据集，我们需要定义 SparkSession，否则 SparkContext 可以执行 RDDs*

# 2.加载数据

我们能够用可迭代数据或外部数据源创建 *rdd* 。我们将首先以文本字符串的形式加载 movielen 数据文件，稍后再进行操作。

*注意:在访问****rdd . todf()****函数之前需要定义 SparkSession，否则会抛出异常。*

```
+------+---+
|    _1| _2|
+------+---+
| apple| 10|
|orange| 20|
| peach|100|
+------+---+
```

# 3.资料检索

通过使用以下命令，我们能够检索 rdd 中的数据信息。

*   getNumPartitions()
*   计数()，计数键()，计数值()
*   collect()、collectAsMap()、take( *num* )、top( *num* )
*   max()、min()、mean()、stdev()、variance()、histogram( *箱号*)、stats()

```
rdd_movie.take(3)>> ['196\t242\t3\t881250949', '186\t302\t3\t891717742', '22\t377\t1\t878887116']
```

# 4.数据处理

在 PySpark 中处理数据可能会让你想起**熊猫**数据帧。与 Pandas 类似，PySpark 也提供了对*分组、聚集、排序和归约*的功能。然而，有一些功能是相似的，但执行方式不同。

## 地图与平面地图

地图和平面地图看起来相似，但它们是不同的，值得特别注意。

map 函数能够维护原始数据形状的列表结构，而 flatMap 会对列表结构进行解包，形成一个大的列表数据结构。

在下面的单元格中，我们将通过用适当的分隔符分隔 rdd_movie rdd 来直观显示这种差异。

## groupBy vs groupByKey

groupBy 函数将根据输入中函数的结果对数据进行分组，groupByKey 将根据原始 rdd 的键对数据进行分组。

```
groupby result:userid last digit = 6
['196', '242', '3', '881250949']
['186', '302', '3', '891717742']
['166', '346', '1', '886397596']
['6', '86', '3', '883603013'] groupByKey result:rating = 1
['22', '377', '1', '878887116']
['166', '346', '1', '886397596']
['181', '1081', '1', '878962623']
['276', '796', '1', '874791932']
```

## reduce vs reduceByKey

在深入研究代码之前，我们需要认识到 *reduce* 是一个动作，而 *reduceByKey* 是转换。

原始文档上的定义:[https://spark . Apache . org/docs/2 . 2 . 0/rdd-programming-guide . html](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html)

*reduce:使用 func 函数(接受两个参数并返回一个)聚合数据集的元素。该函数应该是可交换的和可结合的，这样它就可以被正确地并行计算。*

*reduceByKey:当在(K，V)对的数据集上调用时，返回(K，V)对的数据集，其中每个键的值使用给定的 reduce 函数 func 进行聚合，该函数的类型必须是(V，V) = > V。*

在本节中，我们将演示一些函数示例。

# 5.MovieLen 数据集上的示例用法

在这一节中，我们将使用 PySpark RDD 来处理电影的数据，并检索相应的流派。

```
[['1', 'Toy Story (1995)', ['Animation', "Children's", 'Comedy']],
 ['2', 'GoldenEye (1995)', ['Action', 'Adventure', 'Thriller']]]
```

感谢您的阅读，新年快乐。