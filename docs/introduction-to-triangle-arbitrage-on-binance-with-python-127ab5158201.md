# 用 Python 实现币安三角套利简介

> 原文：<https://medium.com/analytics-vidhya/introduction-to-triangle-arbitrage-on-binance-with-python-127ab5158201?source=collection_archive---------1----------------------->

在本文中，我想展示如何使用 python 代码从币安获取实时数据，并使用它们来定义套利策略。下面编码的策略只是为了展示费用等于零的理想情况。

![](img/92c84d9d5d8286bc95889df798c9e5d1.png)

贝南·诺鲁齐在 [Unsplash](https://unsplash.com/s/photos/trading?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 三角形套利

一般来说，有三种主要的套利方式，其中最常用的是三角形…