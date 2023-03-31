# 使用自动编码器的图像异常检测

> 原文：<https://medium.com/analytics-vidhya/image-anomaly-detection-using-autoencoders-ae937c7fd2d1?source=collection_archive---------0----------------------->

## 探索深度卷积自动编码器，识别图像中的异常。

本文是一项实验性工作，旨在检查深度卷积自动编码器是否可用于 MNIST 和时尚 MNIST 的图像异常检测。

## 简而言之，自动编码器

**功能:**自动编码器对输入进行编码，以识别重要的潜在特征表示*。*然后对潜在特征进行解码，以重建与输入值相同的输出值。