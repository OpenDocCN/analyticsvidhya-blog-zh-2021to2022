# 使用 Alpaca 和 Streamlit 的金融数据流

> 原文：<https://medium.com/analytics-vidhya/financial-data-streaming-using-alpaca-and-streamlit-88aa21c75f27?source=collection_archive---------0----------------------->

本教程旨在使用 Alpaca stream API 流式传输金融数据，特别是开盘价、最高价、最低价、收盘价和成交量数据，并使用 streamlit 实时绘制这些数据。

所以教程可以分为三个部分

*   [羊驼流媒体](https://github.com/alpacahq/alpaca-trade-api-python) API v2
*   [看门狗](https://python-watchdog.readthedocs.io/en/stable/)用于在新数据进来时跟踪文件变化，并重新运行 streamlit。这项功能是 streamlit 自带的。
*   [Streamlit](https://streamlit.io/) 为所有股票代码绘制烛台图表