# 为什么以及如何处理缺失值？

> 原文：<https://medium.com/analytics-vidhya/the-why-and-how-to-handle-missing-values-46ab8c1b9034?source=collection_archive---------2----------------------->

![](img/fc0cb62d5eff067d4d853d6fb6f1155e.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在当今时代，数据源自各种来源，通过多种流收集，并最终利用各种方法进行分析。在这篇文章中，我详细阐述了 ***为什么*** 数据会有缺失值，以及 ***如何使用 Pandas 库处理这些值。***

# 了解丢失数据的“原因”

1.  数据收集不正确
2.  收集和管理错误
3.  有意省略的数据
4.  可能是由于数据的转换而产生的
5.  人为误差
6.  物联网设备(如传感器)故障
7.  代码中的错误

# 我们为什么关心？

1.  有些模型无法处理缺失数据(空值/NaNs)
2.  缺失数据可能是更广泛的数据问题的迹象
3.  缺失数据可能是一个有用的特性

# 缺失价值发现

![](img/c59a2115a0f1bfc003f669e6b949a80e.png)

布里特妮·科莱特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

第一步是了解我们正在处理的数据类型

```
print(df.info()) ordf.dtypes
```

## 查找缺失值

为了找到缺失的值，我们可以运行 ***isnull()***

```
print(df.isnull())
```

这将返回一个二进制序列(True 或 False ),其索引与您试图在其上查找缺失数据的数据帧的原始索引相同。

## 求缺失值的总数

```
print(df.isnull().sum())
```

在您想要查找特定列的缺失值的场景中

```
print(df['column_name'].isnull().sum())
```

熊猫有它自己的一套被认为缺失的惯例，你可以在最新的[熊猫 1.2.4 文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/missing_data.html)中了解更多。通常，这些规则可能无法包含所有缺失值的符号。因为它们可以以各种符号出现，仅举几个例子。，——，_，娜，娜】。

那么，我们如何处理这些呢？

```
missing_values = ["NA", "n/a", "na", "?", "--"]df = pd.read_csv("filename.csv", na_values = missing_values)df.isnull() //now this gives an updated output
            // with the new missing values considered from the list
```

# 处理缺失值

**方法#1:** 删除至少有一个缺失值的所有行

```
df.dropna(how='any')
```

**方法#2:** 删除特定列中缺少值的行

```
df.dropna(subset=['column_name'])
```

上述方法的缺点

*   有效的数据点被删除。
*   这种方法依赖于随机性
*   减少信息

**方法#3:** 用给定的字符串替换特定列中缺失的值

```
df['column_name'].fillna(value='None Given', inplace=True)
```

**方法#4:** 估算缺失值类似于上面的单元格，并将该值复制到缺失值单元格中

```
df['column_name'].fillna(method='ffill', inplace=True)#Alternatively method = pad also has the same functionalitydf['column_name'].fillna(method='pad', inplace=True)
```

**方法#5:** 估算缺失值类似于下面的单元格，并将该值复制到缺失值单元格中

```
df['column_name'].fillna(method='bfill', inplace=True)
```

**方法#6:** 在记录值没有丢失的地方删除特定的列

```
#record where the values are not missing
df['recording_non_null_values'] =        df['column_to_be_dropped'].notnull()# dropping the column after it is recorded
df.drop(columns=['column_to_be_dropped']) 
```

# 处理数字列

我们可以通过使用集中趋势的度量来替换数字列中缺失的值

**方法#1:** 使用平均值替换缺失值

```
print(df['column_name'].mean())df['column_name'] = 
df['column_name'].fillna(df['column_name'].mean())df['column_name'] = 
df['column_name'].astype('int64')#Alternatively, we can also fill the missing values
#by rounding the meandf['column_name'] = 
df['column_name'].fillna(round(df['column_name'].mean()))
```

**方法#2:** 使用中值替换缺失值

```
print(df['column_name'].median())df['column_name'] = 
df['column_name'].fillna(df['column_name'].median())df['column_name'] = 
df['column_name'].astype('int64')#Alternatively, we can also fill the missing values
#by rounding the mediandf['column_name'] = 
df['column_name'].fillna(round(df['column_name'].median()))
```

# 手动处理缺失值

不建议使用这种方法，但是在列中的数据是一组连续值或系列数据的情况下，我们可以使用这种方法

```
df['column_name'].interpolate()
```

插值将填充该列所含数字范围内的缺失值。如果我们有更复杂的模式，比如，列有一系列数据，但是这个系列是 n 次多项式的，那么我们可以使用

```
df['column_name'].interpolate(method='polynomial', order=2)
```

# 结论

数据科学家/数据分析师大约 70%的工作都围绕着处理数据和清理数据。因此，处理缺失值是数据科学家/数据分析师应该能够完成的主要任务之一。希望，这篇文章有帮助。

如果你喜欢读这篇文章，你可以在 LinkedIn 上关注我

*参考文献:*

[1][https://pandas . pydata . org/pandas-docs/stable/user _ guide/missing _ data . html](https://pandas.pydata.org/pandas-docs/stable/user_guide/missing_data.html)

[https://pandas.pydata.org/docs/reference/index.html](https://pandas.pydata.org/docs/reference/index.html)

[3][https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isnull.html？highlight = null #熊猫。DataFrame.isnull](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isnull.html?highlight=null#pandas.DataFrame.isnull)

[https://pandas.pydata.org/docs/reference/api/pandas.[4]DataFrame.interpolate.html？高亮=插值#熊猫。DataFrame.interpolate](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.interpolate.html?highlight=interpolate#pandas.DataFrame.interpolate)