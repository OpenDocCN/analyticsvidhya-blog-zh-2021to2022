# 为什么“df.to_csv”可能是一个错误？

> 原文：<https://medium.com/analytics-vidhya/why-df-to-csv-could-be-a-mistake-f361cf6d40bd?source=collection_archive---------0----------------------->

![](img/7641126d636c8317e6627b7b131ebc47.png)

由[瓦尔瓦拉格拉博瓦](https://unsplash.com/@santabarbara77?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

作为一名数据科学家，在更全球化的数据分析领域，加载和保存数据(DataFrame)几乎是系统化的。

通常，我使用`df.to_csv('path/df.csv')`并且我认为几乎所有的熊猫用户都这样做或者至少做过一次。

所有 Python 用户都知道以 CSV 格式保存数据是非常实用和容易的，但它也有一些缺点，我们将详细说明…