# 深度强化学习中的软行动者批评算法

> 原文：<https://medium.com/analytics-vidhya/soft-actor-critic-algorithms-in-deep-reinforcement-learning-a11bedd9aa20?source=collection_archive---------1----------------------->

在之前的系列文章中，我们谈到了[政策梯度法](/analytics-vidhya/policy-gradients-in-deep-reinforcement-learning-83d99575cfca)、[、](/analytics-vidhya/deep-deterministic-policy-gradient-for-continuous-action-space-9b2b9bacd555)和[信赖域法](/analytics-vidhya/trust-region-methods-for-deep-reinforcement-learning-e7e2a8460284)。这里我们也讨论了每种方法相应的缺点。

*   像基于 PG 的方法是样本低效的，因为它们在一次梯度更新后丢弃样本。在复杂的任务中，丢弃样本会导致更新缓慢，并且经常收敛到次优策略。
*   DDPG 试图通过一个重放缓冲数据结构来解决这个问题，该数据结构存储转换元组。我们抽样了一批…