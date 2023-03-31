# 加权箱融合—详细视图

> 原文：<https://medium.com/analytics-vidhya/weighted-boxes-fusion-86fad2c6be16?source=collection_archive---------6----------------------->

## 一种结合模式集合预测的方法。

大家好，希望每个人都过得愉快，并积极追随你的抱负。在这篇文章中，我将讨论一种叫做“加权盒子融合”的技术。我们开始吧！

在[上一篇文章](https://towardsdatascience.com/non-maximum-suppression-nms-93ce178e177c)中，我们讨论了用于过滤来自物体检测模型的预测的 NMS 和软 NMS 技术。这些技术效果很好，是迄今为止[使用最多的](https://paperswithcode.com/method/non-maximum-suppression#:~:text=Non%20Maximum%20Suppression%20is%20a,below%20a%20given%20probability%20bound.)。这些技术对于过滤单个模型的预测非常有效…