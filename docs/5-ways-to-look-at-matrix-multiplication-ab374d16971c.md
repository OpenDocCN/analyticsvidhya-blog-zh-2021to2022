# 看待矩阵乘法的 5 种方式

> 原文：<https://medium.com/analytics-vidhya/5-ways-to-look-at-matrix-multiplication-ab374d16971c?source=collection_archive---------19----------------------->

你好，我来解释一下机器学习和数据科学需要了解的矩阵乘法的 5 种解读。

![](img/8a06290d267861f1f66e0d2c5f39b2e1.png)

内森·弗蒂格在 [Unsplash](/s/photos/windows?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

关于矩阵符号的提醒:

![](img/af83407e3a18d6ecc4759703324ed660.png)

图片 1

我们正在考虑以下所有 5 个部分的一般情况。

![](img/9797b58c2ed273abeca669545abb5c92.png)

图片 2

第一种方式其实就是矩阵乘法的定义。

1.  矩阵 C 的每一项都是矩阵 A 的相应行和矩阵 b 的相应列的点积。

![](img/e92d9e0af2ebb264523a8bcde875290e.png)

图 3

点积提醒:

![](img/1d56a6a27582a5c99a0900674586c77f.png)

图 4

从现在开始，下面的一切只是对第一个的不同解释。

2.C 的每一列是 A 的列的线性组合，B 中相应列的值作为权重。

我已经把它记在图 5 和图 6 中了:

![](img/792e942f7f458db72271f063bb283074.png)

图 5

让我们用图 5 的透视来看看 C 的第一列:

![](img/356dcf8a82b2c81a5008c4cd0b9b4806.png)

图 6

3.当**矩阵 i** 是 A 的**列 I 和 b**的 r **行 I 的乘积时，矩阵 C 可以通过增加与 C 大小相同的 **n 个矩阵**来计算**

![](img/49cb8ebef22d139798d720faf4efbf36.png)

图 7

让我们用一个例子来阐明它:

![](img/92252dda05c6b27f09cf70a73d04ba33.png)

图 8

4.C 的每一行都是 A 乘以矩阵 b 的对应行。

![](img/44c8693e6e8243ca60d34f29d3b7f6c5.png)

图 9

5.我们可以将矩阵 A 和 B 分块，并按如下方式计算 C:

我们将 A 和 B 分成块的方式可能有不同的选择，但是我们应该考虑到得到的块的维数符合矩阵乘法的规则。

![](img/9ebcae59a599e0cfa30b60656e644408.png)

图 10

感谢阅读。祝你平安。如果你喜欢它，你可以按拍手图标。