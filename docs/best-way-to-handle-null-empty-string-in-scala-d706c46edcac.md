# Scala 中处理 NULL /空字符串的最佳方式

> 原文：<https://medium.com/analytics-vidhya/best-way-to-handle-null-empty-string-in-scala-d706c46edcac?source=collection_archive---------1----------------------->

Scala 在字符串处理方面类似于 JAVA。Scala 中有 4 种不同的技术来检查空字符串。

![](img/870cdfe5ae52e6aaab2b84c5a43b7afd.png)

瓦伦丁·拉科斯特在 [Unsplash](https://unsplash.com/s/photos/empty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

***手法 1*** :检查长度为 0

```
val str:String = ""if (str.length == 0){
   println(s"Length of Variable ${str} is 0")
}
```