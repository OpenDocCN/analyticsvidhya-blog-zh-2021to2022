# PySpark 入门

> 原文：<https://medium.com/analytics-vidhya/getting-started-with-pyspark-251a0af0f2c4?source=collection_archive---------3----------------------->

## Spark 分布式计算

## 使用 Python 连接到 Spark 集群

![](img/41da82813b0aae026af3cee55a9aa0e6.png)

沃伦·王在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在过去的十年里，人们对快速可靠的工具产生了前所未有的需求，以处理海量数据。解决这个问题的方法之一是[MapReduce](https://en.wikipedia.org/wiki/MapReduce#:~:text=MapReduce%20is%20a%20programming%20model,distributed%20algorithm%20on%20a%20cluster.&text=MapReduce%20libraries%20have%20been%20written,with%20different%20levels%20of%20optimization.)——然而，这不仅花费大量时间，而且无法处理实时数据。火花来救援了！Spark 由加州大学伯克利分校实验室推出，并于 2010 年开源。它提供了一个既快速又通用的解决方案。Spark 的一个主要优势是它在内存中运行计算。另一个是[惰性计算](https://data-flair.training/blogs/apache-spark-lazy-evaluation/)机制，简单地说就是除非被明确要求，否则不采取任何行动。这有助于更快的数据处理，从而大大减少时间。

Spark 的神奇之处在于它的 rdd——弹性分布式数据集。如果你熟悉熊猫数据框，这可能会更容易。RDDs 的一个抽象层次是 Spark DataFrame，它使用起来更简单，并且可以自动优化。

## 首先，什么是火花？

Spark 是一个集群计算框架，它将一个任务划分到一个称为节点的计算机集群中，以实现快速高效的处理。这种数据分割使得处理大型数据集变得更加容易。每个节点处理并行分配的计算，这些计算稍后被组合以返回最终结果。

**那 PySpark 是什么？**

很高兴你问了——这是 Python 中 Apache Spark 的一个接口，允许你使用 Python APIs 编写 Spark 应用程序。简单来说，PySpark 是一种使用 Python 连接 Spark 集群的方式。

**有熊猫数据框为什么还要 Spark 数据框？**

问得好——如果你必须思考你是否需要火花，或许你根本不需要火花。老实说，除非你正在处理非常大的数据集，熊猫工作得很好。这两者之间的主要区别是 pandas 数据帧不是分布式的，而是在单个节点上运行。Spark DataFrame 接口的优点之一是可以在 Spark 集群中的表上运行 SQL 查询。

## 熊猫数据框列与 Spark 数据框列

值得注意的是，更新 Spark 数据帧与在`*pandas*`中工作有些不同，因为 Spark 数据帧*是不可变的*。这意味着它不能被改变，因此列不能被就地更新。

## 装置

你可以在这里安装 Spark。值得一提的是，spark 是用 Scala 编写的，使用 py4j 与 JVM 接口交互。因此，要运行 PySpark，您还需要将 [Java](https://www.java.com/en/download/) 与 Python 和 Apache Spark 一起安装。

为了让您熟悉 spark 生态系统，我建议在配置 spark 在集群上运行之前，在本地运行 spark，因为它可以独立运行。

既然我们已经看到了如何获得 *Spark* ，我们现在可以连接到一个集群并完成一些基础工作。事不宜迟，让我们继续—

*   **使用 PySpark** 连接到 Spark 集群——首先，我们需要一个到集群的连接。这是通过创建一个`SparkContext`类的实例来完成的。验证您运行的是哪个版本的 Spark。试试这个:`print(SparkContext.version)`
*   **使用 Spark 数据帧**——一旦我们连接到集群，我们就可以使用它从您的`SparkContext`创建`SparkSession`对象。您可以将`SparkContext`视为您与集群的连接，将`SparkSession`视为您与该连接的接口。为了避免多个连接和会话，最好使用**spark session . builder . getor create()**

```
#Import SparkSession
from pyspark.sql import SparkSession
#Create Session
spark = SparkSession.builder.getOrCreate()
```

*   您可以使用`spark.catalog.listTables()`查看集群中的所有表。要列出表格内容，可以使用 show()。Ex- `Table1.show()`将显示表 1 中的 20 行和所有列。
*   `.createDataFrame()`方法接受一个`pandas`数据帧并返回一个 Spark 数据帧。请注意，创建的数据框将不会保留在会话目录中，因为它们存储在本地。这意味着您可以在它上面使用所有的 Spark DataFrame 方法，但是不能在其他上下文中访问数据。
*   `.select()`方法——用于从 Spark 数据帧中选择列。此外，当您使用`df.colName`符号选择列时，您可以执行任何列操作，它将返回转换后的列。例如，见下文。

```
# Weekly Salary when you have monthly salaryWeek_Salary = (df.select(df.salary/4)).alias(“weekly salary”)
```

*   `.filter()`方法——正如您已经猜到的，这是 SQL 的`WHERE`子句的 Spark 等价物。将 SQL 表达式的`WHERE`子句之后的表达式作为字符串，或者作为布尔(`True` / `False`)值的火花列。

```
#SQL
SELECT * FROM table WHERE column > 120#PySpark
table.filter("column > value").show()
```

*   `.createTempView()` Spark DataFrame 方法可以帮助你建立一个临时表，并接受你想要注册的临时表的名称作为参数。该方法将数据帧注册为目录中的一个表格，但由于该表格是临时的，只能从用于创建 Spark 数据帧的特定`SparkSession`中访问。
*   `spark.read.csv(file_path)`可以用来读取 PySpark 中的文件。如果你的文件有标题，你可以传递一个参数告诉 spark 第一行是标题。`spark.read.csv(file_path,header = True)`
*   `.withColumn()`采用两个参数的方法，可用于在数据集中创建新列。首先是一个包含新列名称的字符串，其次是新列本身。例如，将价格提高 100。`df.withColumn("price",col("price")+100).show()`
*   有时，使用像`pandas`这样的工具在本地使用该表是有意义的。使用`.toPandas()`火花数据帧使这变得简单
*   另一个常见的数据库任务是聚合。也就是说，通过将数据分成几部分并对每一部分进行汇总来减少数据。也可以通过`(.groupBy())`对多列进行分组。如果 group by 中有两列，它们的唯一组合将出现在输出中。

就这样了，伙计们！

# 外卖食品

在本文中，我们通过一些常用的基本查询来了解 PySpark。本质上，PySpark 是让 Python 与 Spark 集群对话的一种方式。如果您对 SQL 和 Python 有一点了解，您可以跳到 PySpark ship 上🚢很快。

Spark 已被广泛采用，并作为大规模分析处理引擎广泛用于数据科学和机器学习社区。该堆栈支持 Spark 流、Spark 机器学习库(MLLib)和 Spark SQL 的实时和批量数据处理。在这里阅读更多关于他们的信息！

根据你的喜好和问题，也许值得看一看 [Dask](/swlh/parallel-processing-in-python-using-dask-a9a01739902a) 。有任何问题，请喊我一声！

我很高兴你能走到这一步！谢谢你的时间。不管你在忙什么，祝你过得愉快😄