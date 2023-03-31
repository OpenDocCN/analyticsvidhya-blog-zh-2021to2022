# 上下文 RCNN——用于每个摄像机对象检测的长期时间上下文

> 原文：<https://medium.com/analytics-vidhya/context-rcnn-long-term-temporal-context-for-per-camera-object-detection-1cc493176400?source=collection_archive---------4----------------------->

## 深度学习

## 将同一摄像机拍摄的其他帧动态合并到对象检测管道中。

你好。今天我们将看一看 [Context RCNN](https://arxiv.org/abs/1912.03538) ，这是谷歌研究院与加州理工学院联合发表的一篇论文。

## 语境

**目标:**静态摄像机上的目标检测。