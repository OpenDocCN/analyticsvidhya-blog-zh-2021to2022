# scikit-learn 中混合类型数据的朴素贝叶斯

> 原文：<https://medium.com/analytics-vidhya/naive-bayes-for-mixed-typed-data-in-scikit-learn-fb6843e241f0?source=collection_archive---------4----------------------->

如果我们看看 scikit-learn 中的朴素贝叶斯(NB)实现，我们将能够看到各种各样的 NB。举几个例子…

*   高斯朴素贝叶斯
*   多项式朴素贝叶斯
*   分类朴素贝叶斯

所有的实现都是专为适应特定类型的数据或分布而设计的。高斯 NB 假设您的数据是独立的和正态分布的，类似于多项式和分类，其中分布是我们选择实现的原因。

但在实时 ML 问题中，我们的数据集可能包含混合分布，如**连续、分类、多项式和伯努利**。

![](img/53a06ca6fc2288e6e3231192e621ace3.png)

由[乌斯曼·优素福](https://unsplash.com/@usmanyousaf?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 在这种情况下，我们如何使用 NB？在 scikit-learn 中有它的实现吗？

Ans:没有！

## 但是我们自己能做到吗？

Ans:很明显！是的。

## 我应该从零开始实施吗？

Ans:没有！我们将利用现有的 scikit-learn 实现。

## 好了，让我们快速回顾一下 NB 的基本原理。

数据集中的每个特征都是相互独立的，因此 ***利用独立性的特性，我们将每个特征的后验概率*** 与标签的先验概率相乘，得到标签的预测概率。

*我们以多项式 NB 为例。此处*

*   **P(W)** —单词的先验概率(W= w，w …)
*   **P(L)** —标签的先验概率(L= l，l …)
*   **P(w^k/l^n)** —给定标签为 l^n.，找到单词 w^k 的后验概率(其中 k = 1…单词总数，n=1…标签总数)

假设我们有一个的单词序列

> " w1 w2 w3 w4 "

*通过*计算其属于标签 L1 的概率

> π(给定标签 L 的序列中每个单词的后半部分)*标签 L 的前半部分
> 注:π —的乘积

## P(L1)* P(w1/L1)* P(w2/L1)* P(w3/L1)* P(w4/L1)

我们增加了概率，因为我们假设一个单词的出现与另一个单词无关。因此，这些是同时发生的独立事件。

同样，在我们的混合数据挑战中，如果我们的数据同时包含连续值和分类值，我们遵循以下步骤。

> 1.假设数据是相互独立的。
> 
> 2.将连续数据拟合到高斯 NB。并得到该连续数据发生的概率/对数概率。
> 
> 3.将分类数据拟合到分类 NB。并得到该分类数据出现的概率/对数概率。
> 
> 4.让我们得到步骤(2)和(3)的对数概率。让它们成为 **Log_Proba_Cont** 和 **Log_Proba_Cat** 。
> 
> 5. **Log_Proba_Cont 和 Log_Proba_Cat 在计算中使用标签的先验概率。因此，我们将减去一次，同时增加两个概率**。
> 
> 6.即整个数据集的最终对数概率可以是
> ***Log _ Proba _ Cont+Log _ Proba _ Cat—P(L)***
> 
> 7.**这相当于 proba(cont)* proba(cat)/prior(Label)**。第(6)行中 probas 的 Log 将乘法转换为加法。

让我们试着在 [kaggle](https://www.kaggle.com/guruprasad91/titanic-naive-bayes) 的 Titanic 数据集中实现这个。

注意:在 scikit-learn repo 中有一个针对上述流程的持续公关。

参考:
[https://stackoverflow.com/a/14255284](https://stackoverflow.com/a/14255284)