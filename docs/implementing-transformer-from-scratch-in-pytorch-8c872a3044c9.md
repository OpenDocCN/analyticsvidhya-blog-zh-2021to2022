# 在 Pytorch 中从头开始实现 Transformer

> 原文：<https://medium.com/analytics-vidhya/implementing-transformer-from-scratch-in-pytorch-8c872a3044c9?source=collection_archive---------5----------------------->

![](img/4ff6c64253b504bda7ed687aa2a6827f.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

变形金刚是深度学习领域的一项改变游戏规则的创新。

这种模型架构已经取代了 NLP 任务中的所有 rnn 变体，并且显示出对视觉任务中的 CNN 做同样事情的前景。

然而，PyTorch Transformer 文档使得入门有点困难。

*   没有解释如何做推论