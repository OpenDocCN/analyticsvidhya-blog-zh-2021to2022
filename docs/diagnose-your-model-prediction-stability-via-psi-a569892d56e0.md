# 通过 PSI 诊断您的模型预测稳定性

> 原文：<https://medium.com/analytics-vidhya/diagnose-your-model-prediction-stability-via-psi-a569892d56e0?source=collection_archive---------4----------------------->

![](img/2c0c61d680167a7ca5cf954c8cb7f57d.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 动机

一旦训练了机器学习模型，流水线似乎就完美地完成了。一些隐藏问题的可能性导致预测概率的分布漂移，我们还没有注意到，但在 MLOps 中将是一个大问题。因此，本文讨论了种群稳定性指数(PSI)。