# 用 Python 计算公司的违约概率

> 原文：<https://medium.com/analytics-vidhya/calculating-a-companys-probability-of-default-with-python-5a91cda82caf?source=collection_archive---------6----------------------->

![](img/51b69a81afc9cdf14d1e440227650745.png)

在这个分析中，我们使用了几种基于 Python 的科学计算技术和 [AlphaWave 数据股票分析 API](https://rapidapi.com/alphawave/api/stock-analysis/endpoints) 。详细描述这一分析的 Jupyter 笔记本也可以在 [Google Colab](https://colab.research.google.com/drive/1GbxEJ1JuZUEviLCpRU7b73engWZzYYzh?usp=sharing) 和 [Github](https://github.com/AlphaWaveData/Jupyter-Notebooks/blob/master/AlphaWave%20Market-Implied%20Probability%20of%20Default%20Example.ipynb) 上获得。

```
import time
import requests
import selenium
import numpy as np
import pandas as pd
from sympy import *
from datetime import date
from datetime…
```