# 特征选择—过滤方法

> 原文：<https://medium.com/analytics-vidhya/feature-selection-filter-method-43f7369cd2a5?source=collection_archive---------3----------------------->

![](img/9eda4f1aa505acda9da902caf2049121.png)

为了方便地研究数据、建立模型并获得好的结果，对数据进行预处理是很重要的，而最好的方法之一就是特征选择。

## 什么是特征选择？

选择和保留最重要特征的过程称为**特征选择。**它有助于降低模型的噪声和计算成本，有时还能提高这些模型的性能。

特征选择有三种方法，即:
滤波法；
包装方法；
嵌入式方法。

## 过滤方法:

这种方法通常用作预处理步骤。对于这种方法，根据各种统计测试或者基于单变量度量(例如
方差)来选择特征；
相关性；
卡方；
相互信息。

存在不同的相关性计算方法，下面提供了其中一些常用的方法。

*皮尔逊相关*:该方法用于计算连续变量之间的相关性。

*线性判别分析:*度量连续和分类特征之间的相关性。

*卡方:*计算两个或更多分类特征之间的相关性。

*ANOVA:* 除了有两个独立的分类特征和一个连续的相关特征之外，与线性判别分析相同。

**优点:**

计算速度非常快；
避免过拟合；
不依赖机型，只依赖特性；
基于不同的统计方法。

**缺点:**

不要删除多重共线性；
有时可能选择失败；

滤波方法可以分为两组，即:单变量滤波方法和多变量滤波方法。

在单变量过滤方法中，根据特定标准，例如 fisher 评分、互信息和方差，独立地观察每个特征。与此同时，还有一个重要的缺点。这种方法可以选择冗余特征，因为关系的无知。

另一方面，多元滤波方法可以处理冗余，同时用于去除重复和相关的特征。

## 移除常量功能:

常数是仅包含一个值的特征，因此它们对分类和建模没有影响。建议删除。

要删除 Python 中的常量，首先，必须将给定数据集分为训练数据集和测试数据集，然后按如下方式删除常量:

```
constant_features = [var for var in X_train.columns if X_train[var].std() == 0] 
X_train.drop(labels=constant_features, axis=1, inplace=True)
X_test.drop(labels=constant_features, axis=1, inplace=True) X_train.shape, X_test.shape
```

## 移除准常数特征:

准常数、几乎常数要素对于数据集的大部分具有相同的值。这些值在预测期间没有用，并且阈值的方差没有被适当地定义。然而，一般来说，99%的常数重复自己可以被删除。

要删除 Python 中的准常量，首先要导入库，并将数据集拆分为训练集和测试集。之后，开始定义函数以移除准常数，如下所示:

```
# Define the threshold as 0.01
q_remover = VarianceThreshold(threshold=0.01)# Find the values with low variance
q_remover.fit(X_train) 
sum(q_remover.get_support())# Apply to datasets
X_train = q_remover.transform(X_train)
X_test = q_remover.transform(X_test)
X_train.shape, X_test.shape
```

## **删除重复特征:**

重复要素是指重复多次的要素。这些值对数据集没有影响，但会延迟训练时间。因此，建议移除重复的特征。

```
duplFeatures = []
for i in range(0, len(X_train.columns)):
oneCol = X_train.columns[i]
for othCol in X_train.columns[i + 1:]:
        if X_train[oneCol].equals(X_train[othCol]):
            duplFeatures.append(othCol)
X_train.drop(labels=duplFeatures, axis=1, inplace=True)
X_test.drop(labels=duplFeatures, axis=1, inplace=True)
```

## 移除相关特征:

统计学和数据科学中的一个重要术语是相关性。如果特征在线性空间中是接近的，那么特征是相关的。因此，相关特征是重要的，应该被观察到。然而，有时这些特性也会造成冗余。这就是为什么，最好是删除它们。

为了移除重复的特征，应该导入适当的库，并且应该将数据集分成训练数据集和测试数据集。然后，下面的代码可以用来删除重复的特性。

```
correl_Feat = set() 
correl_matrix = dataset.corr()

for i in range(len(corr_matrix.columns)):
   for j in range(i):
       if abs(correl_matrix.iloc[i, j]) > 0.8:
       colName = correl_matrix.columns[i]  
       correl_Feat.add(colname)
X_train.drop(labels=correl_Feat, axis=1, inplace=True)
X_test.drop(labels=correl_Feat, axis=1, inplace=True)
```

在上面的代码中，0.8 是一个阈值，可以用任何方式定义。

特征选择是机器学习中一个非常重要的术语，因为它在任何 ML 模型的性能和训练中起着巨大的作用。本文介绍了特征选择的过滤方法，并给出了一些实例。

完整的代码可以在我的 [Github](https://github.com/zaurrasulov/Data-Science---Python/blob/main/FilterMethod.ipynb) 个人资料中看到。