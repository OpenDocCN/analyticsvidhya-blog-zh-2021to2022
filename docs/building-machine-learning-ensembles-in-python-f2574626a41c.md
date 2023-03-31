# 用 Python 构建机器学习集成

> 原文：<https://medium.com/analytics-vidhya/building-machine-learning-ensembles-in-python-f2574626a41c?source=collection_archive---------7----------------------->

![](img/7c0891e6274d2a7a48cf6946505185f5.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

集成学习是在深度学习中使用的过程，其中多个模型或专家或分类器被组合在一个集成中以改进预测结果。集合中的每个单独的模型，一旦被训练，就产生对一个看不见的数据点的预测。这些预测然后以某种方式汇总。对于回归问题，聚合通常是算术平均数，而众数的模式…