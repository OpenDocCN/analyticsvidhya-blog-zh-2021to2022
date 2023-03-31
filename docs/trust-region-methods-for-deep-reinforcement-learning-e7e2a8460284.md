# 深度强化学习的信赖域方法

> 原文：<https://medium.com/analytics-vidhya/trust-region-methods-for-deep-reinforcement-learning-e7e2a8460284?source=collection_archive---------5----------------------->

# 信赖域方法

*   机器学习的基本构建模块是优化某种目标函数，如均方损失或最大对数似然。最小化损失，并在此过程中优化权重和偏差，反复这样做，你将获得一个性能良好的模型。
*   权重的优化有两种方式。I)线搜索方法 ii)信赖域方法。在前者中，我们选择一个步长长度(称为学习率)来更新我们的权重。在信任域中，我们…