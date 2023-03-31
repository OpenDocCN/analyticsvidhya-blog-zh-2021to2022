# 想不通 Sklearn Tfidf 矢量器是怎么得到它的值的？以下是方法

> 原文：<https://medium.com/analytics-vidhya/how-sklearns-tfidfvectorizer-gets-its-numbers-vectors-60e4caf54a31?source=collection_archive---------6----------------------->

![](img/46f132f6e7bdecdec5bd13c01a1b811d.png)

如果你碰巧想知道为什么 Sklearn 的 TfidfVectorizer 没有返回与你使用教科书公式计算的相同的数字，我花了几个小时调查原因，这里是为了节省我们的集体时间。

```
from sklearn.feature_extraction.text import TfidfVectorizedocuments = […
```