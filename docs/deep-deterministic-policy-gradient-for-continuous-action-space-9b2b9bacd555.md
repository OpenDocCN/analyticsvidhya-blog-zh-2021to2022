# 连续动作空间的深度确定性策略梯度

> 原文：<https://medium.com/analytics-vidhya/deep-deterministic-policy-gradient-for-continuous-action-space-9b2b9bacd555?source=collection_archive---------4----------------------->

在之前关于策略梯度方法的[文章](/analytics-vidhya/policy-gradients-in-deep-reinforcement-learning-83d99575cfca)中，我们讨论了基于 PG 方法的缺点。它们不是样本有效的，因为它们在每次迭代中丢弃了先前学习的策略。我们可以说它们是基于策略的学习方法，因为相同的策略生成动作并更新价值函数。这些方法不利用价值函数信息，这需要非策略设置。所以我们有一套新的算法，叫做演员-评论家方法，DDPG 就是其中之一。

*   政策梯度是…