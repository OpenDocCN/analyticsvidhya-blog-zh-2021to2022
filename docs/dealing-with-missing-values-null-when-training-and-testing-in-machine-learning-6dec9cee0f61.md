# 机器学习中训练和测试时丢失值(NULL)的处理

> 原文：<https://medium.com/analytics-vidhya/dealing-with-missing-values-null-when-training-and-testing-in-machine-learning-6dec9cee0f61?source=collection_archive---------14----------------------->

在本文中，我将介绍在训练和测试机器学习模型时处理缺失特征值的各种方法。

![](img/66b89cfce3960ab90fc3132dba714cb8.png)

[伊琳娜](https://unsplash.com/@sofiameli?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

在许多预测建模应用中，有用的属性值(“特征”)可能会丢失。例如，患者数据经常丢失…