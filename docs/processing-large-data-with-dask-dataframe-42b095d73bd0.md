# 用 Dask 数据框架处理大数据

> 原文：<https://medium.com/analytics-vidhya/processing-large-data-with-dask-dataframe-42b095d73bd0?source=collection_archive---------7----------------------->

在工作中，我们通常会可视化和分析非常大的数据。在典型的一天中，这相当于 6500 万条记录和 20 GB 的数据。在许多天的时间范围内分析大量的数据是一项挑战。数据的规模迫使我们在比预期更短的时间内进行分析。

我最近发现了 Dask 库，因此我想为任何想开始使用这个神奇工具的人写一篇关于它的文章。

我们使用典型的 Python 数据工具包来完成 ETL 工作。庞大的数据量对于我们的标准工具`numpy` / `pandas`来说太大了。有像 Spark 这样的分布式计算框架来处理繁重的工作。虽然 Spark 可以处理这项工作，但是从 Python 数据工具包迁移到 Spark 是一个彻底的改变。

达斯克来了！

# Dask 是什么？

Dask 的设计是为了扩展`numpy`和`pandas`包来处理那些太大而无法保存在内存中的数据处理问题。它将较大的处理任务分解成许多较小的任务，由`numpy`或`pandas`处理，然后将结果重新组合成一个连贯的整体。这发生在一个无缝接口的背后，该接口被设计成模仿`numpy` / `pandas`接口。

以下是它的一些属性:-

*   `dask.bag`:一个无序集合，有效地分布式替代 Python 迭代器，从文本/二进制文件或任意`Delayed`序列中读取
*   `dask.array`:具有类似`numpy`界面的分布式阵列，非常适合扩展大型矩阵运算
*   `dask.dataframe`:类似于`pandas`的分布式数据帧，用于高效处理表格化、组织化的数据
*   `dask_ml`:围绕`scikit-learn`的分布式包装器——类似机器学习工具

如果您有兴趣了解更多关于 Dask 的不同用例，请查看此链接:[https://stories.dask.org/en/latest/](https://stories.dask.org/en/latest/)

我将展示一个如何用 dask 加载数据集的例子，在本教程中，我使用的是一个 10GB 大小的 CSV 文件，pandas 在读取它时崩溃了。开始了。

# 安装 DASK

```
pip install dask[complete]
```

# 加载数据集

因此，我们将加载一个 10 GB 的数据，非常容易与 Dask！

关于`read_csv`函数需要注意的一点是，它实际上并没有将数据加载到内存中。相反，它创建了一个将数据加载到内存中所需完成的工作的[任务图](http://docs.dask.org/en/latest/graphs.html)。任务图在程序的后面部分执行。我们可以通过`print`来快速浏览一下`dask.dataframe`的样子:

```
Dask DataFrame Structure: datetime open high low close volume instrument npartitions=1 object float64 float64 float64 float64 int64 object … … … … … … … Dask Name: from-delayed, 43 tasks
```

在这一点上，我们看到`dataframe`知道它将加载的数据的结构，并且已经将工作划分为任务。数据包括列 Dask 将加载数据的工作分成 43 个任务。然而，它还没有完成任何任务。

# 打印数据

使用典型的 pandas 命令实际查看 dask 数据帧。这样，您应该能够像往常一样访问数据集。

```
print(ticker_data.head())
```

如果您对 dask 还不太熟悉，您可以用很少的语句将数据集转换成 pandas dataframe。

```
# Converting dask dataframe into pandas dataframe
result_df=df.compute()
type(result_df)pandas.core.series.Series
```

# 结论

Dask 是将数据处理工作负载从单台机器扩展到分布式集群的绝佳选择。对于标准 Python 数据科学工具包的用户来说，它似乎很熟悉，并且允许向分布式处理发展。你可以在这里了解更多关于 dask [的信息。](https://examples.dask.org/dataframe.html)