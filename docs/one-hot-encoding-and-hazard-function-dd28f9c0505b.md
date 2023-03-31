# 一个热编码和危险功能

> 原文：<https://medium.com/analytics-vidhya/one-hot-encoding-and-hazard-function-dd28f9c0505b?source=collection_archive---------36----------------------->

一种热编码是一种过程，其中分类变量被转换成一种形式，该形式被馈送给机器学习算法以进行更准确的预测。

我在一个由我策划的虚拟数据上演示了这个概念。

![](img/2eef28a7e99b25dfa50314208afa8376.png)

在这个样本数据集中，“腹水”、“水肿”和“分期”是分类变量，而“胆固醇”是连续变量，因为它可以是任何大于零的十进制值。

在这个数据集中，我对水肿列应用了一个热编码，因为它有三个类别。这个列上的一个热编码将为每个可能的结果创建特征列。我也将这种技术应用于 stage 列，因为它们不是 0 和 1 的形式。

热编码'阶段'

![](img/487d44f2c5dcf7997bdee8d913691475.png)

查看上表，它显示其中一个要素是冗余的，因此为了避免多重共线性，我删除了其中的列。

![](img/77af2fe94696d30f81ac918d8ac107ed.png)

我将 one hot 编码值转换为小数，因为模型希望以某种数据类型将这些值提供给它。我将“stage_4”列重命名为“stage ”,然后使用 numpy 将值转换为 float64 数据类型。

![](img/66ddd60a7c0eafb6a04efe9d9db23294.png)

# 预测:

使用上述数据集，我使用风险函数预测了新患者的风险

![](img/586884ef9ab5402fc8cb0a10880d1ec0.png)

其中:

θ是 Xi 特征的系数，λ(t，x)是病人的危险度。

我用特征乘以系数。

![](img/228760e5b91de567c4805e0fe545b8df.png)![](img/c6a33cfe3094dc52a95d189a00734bae.png)![](img/8e855f3510f0ec061d1b0996965150f9.png)![](img/283836186e582d13addb28e8f83e4870.png)

该系数是一个 1D 数组，所以我转置了 X

![](img/f82d6c8b526627cc392b327225aac731.png)

最后我预测了三个病人的危害。

![](img/17ef6f3920d865963c25bd395505d36c.png)

# 结论:

我相信我已经理解了一个热编码意味着什么以及如何去做。这是[代码](https://github.com/Nwosu-Ihueze/ML-projects/blob/main/hazrads.ipynb)，你可以在 [LinkedIn](https://www.linkedin.com/in/rosemary-nwosu-ihueze/) 上与我联系。感谢您的阅读。