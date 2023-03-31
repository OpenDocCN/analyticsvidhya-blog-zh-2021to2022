# 基于 Covid19 印度数据的 EDA

> 原文：<https://medium.com/analytics-vidhya/eda-on-covid19-india-data-e0fc4168e1a3?source=collection_archive---------24----------------------->

**探索性数据分析(EDA)** 是指对数据进行初步调查的关键过程，以便在汇总统计和图形表示的帮助下发现模式、发现异常、测试假设和检查假设。

![](img/7e26e4cae6b7e1fe4eb1c27bca68022c.png)

首先，你需要下载 [covid19 印度数据](https://www.kaggle.com/sudalairajkumar/covid19-in-india)。

让我们导入所有的库并读取数据。

![](img/d5fd0c553a68af46a618092b969e021c.png)

我们可以通过使用`head()`方法显示数据帧的前 5 行来检查数据是否成功导入。

![](img/099c9d022ccf9ea2e50b770397c21b6d.png)![](img/3f9d633b160f177558a019ee29e9daf7.png)

现在，让我们对这个数据集应用 describe()方法并查看结果。它显示平均值、标准偏差、四分位数和最大最小值的描述。

![](img/7f6bd5fe679e954ac776d5fcd2b9d789.png)![](img/4dd66fcaf56811b934e45cb0a1dffb0a.png)

**状态智能分析**

![](img/59b76478e20927d9e2ae7228cca38860.png)

确诊病例最多的 10 个州

![](img/9a79cd576c32a1a6c1c6088dde941269.png)![](img/52c4fcca3dd29100d72c96d27780aa72.png)

治愈病例最多的 10 个州

![](img/ca32c7dcea05bf0c86920842e679402e.png)![](img/04ce80534aac82855baa588a48876dee.png)

死亡人数最多的 10 个州

![](img/23e1ec65b5697cee1131ecea75f3edae.png)![](img/5c45b1a1a73f83741167a25d0da9dba2.png)

从以上图表中我们可以看出，马哈拉施特拉邦的确诊、治愈和死亡病例数最高。

**月明智分析**

![](img/4e6e6f58dff0d8c456025762eba8f132.png)

确诊病例比较

![](img/f91bfc69c8acfcde09a8f69c9875a46a.png)![](img/b8b0a9ebae52dfd902d82a055a74c5f7.png)

治愈病例对比

![](img/6aed31571a43b86e34e1996c88cb5e37.png)![](img/9186711628a8b14928efd85f948e0fb3.png)

死亡案例对比

![](img/51985aeba0b32b5625994b92c1c82fc8.png)![](img/69daa69e36c3918779b51985ef4ee077.png)

因此，通过使用 EDA 技术，我们可以完全了解数据集，我们可以从数据集中提取有意义的信息，还可以找出数据集中是否存在任何缺陷。

使用可视化图表，可以从数据中获得多种见解。在本文中，我们介绍了一些基本的可视化。使用这些数据可以得出更多的分析和见解。