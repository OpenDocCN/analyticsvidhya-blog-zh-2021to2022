# 使用 Python 和 Google big query DIY 股票追踪器。

> 原文：<https://medium.com/analytics-vidhya/diy-stock-tracker-using-python-google-bigquery-and-data-studio-cb603b4f6f39?source=collection_archive---------8----------------------->

![](img/0ea0d8334311ca8440fcfa8749dde6a7.png)

[M. B. M.](https://unsplash.com/@m_b_m?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/technology-stocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

在这篇文章中，我们将编写一个 python 脚本，使用`[yfinance](https://pypi.org/project/yfinance/)`包获取股票市场数据，处理数据并将数据上传到 Google BigQuery 表中，该表可用于进一步的分析或可视化。

# **获取股市数据**

为了获取特定股票的数据，我们需要股票的股票代号。一旦你…