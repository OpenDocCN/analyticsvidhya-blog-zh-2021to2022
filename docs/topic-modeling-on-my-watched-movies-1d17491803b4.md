# 对我观看的电影进行主题建模

> 原文：<https://medium.com/analytics-vidhya/topic-modeling-on-my-watched-movies-1d17491803b4?source=collection_archive---------5----------------------->

![](img/82335dc510e830b5b4d6c8778657a64d.png)

谷歌

我给我在 IMDb 上看的所有电影打分，网站允许你下载一个好的。csv 显示您的所有评分。这个。csv 包含关于电影的基本信息。为了进行主题建模，我需要电影的情节和/或摘要。我将从维基百科中抓取这些信息，并用它来丰富 IMDb 数据集。然后，我将对电影的情节+摘要执行 LDA 主题建模，以找到 6…