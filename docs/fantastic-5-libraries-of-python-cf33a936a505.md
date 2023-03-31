# 5 个精彩的 Python 库

> 原文：<https://medium.com/analytics-vidhya/fantastic-5-libraries-of-python-cf33a936a505?source=collection_archive---------11----------------------->

## 快速浏览 Python 中被低估的库，这将使您的工作毫不费力

![](img/7d3bc2006a1834b868f134c6e4629311.png)

[丹尼尔](https://unsplash.com/@setbydaniel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://www.unsplash.com/) 上的照片

> Python 不仅仅是神奇，不是吗？无论我们要做什么任务，python 都有它的解决方案，无论它是与机器学习、数据可视化、图像处理相关，还是任何简单的任务，如寻找单词的含义。Python 为各种简单和困难的任务提供了大量的库。

大多数对数据科学感兴趣的人都知道 ML 中使用的库，比如 Pandas、Numpy、Matplotlib、Seaborn、OpenCV、Plotly 等等。但是有许多其他的库对于小的需求来说同样令人惊奇。

今天，我将分享我在一个项目中遇到的一些库。这些库是如此的可移植，以至于只需几行代码就能适合您的项目。我将用一段代码提供每个库的简介。

## 让我们立即开始吧！

## 1.py 笑话

您可能听说过基础设施即服务(Iaas)、软件即服务(Saas)和平台即服务(Paas)。但是你听过笑话作为服务吗？？

Python 库**py 笑话库**提供笑话服务。这是一个机智的图书馆，为我们提供了一行古怪的笑话。

*   使用下面一行代码安装 py 笑话。

```
pip install pyjokes
```

*   导入 pyjokes 并调用 get_joke()。

上面的代码片段将为您提供一个随机的极客笑话！

```
#Output"Why do Java programmers have to wear glasses? Because they don't see sharp."
```

这个库也提供不同的语言和类别。这里分类为*中性*(包含编程相关笑话)*绕口令*(绕口令)*全部*(全部笑话)。您还可以使用 get _ jokes()一次获得多个笑话。

## 2.py 字典

在我们的项目中，我们经常会遇到字典的需求。你会怎么处理？你会废弃这些数据吗？啊哈，这里有一个简单的方法！

PyDictionary 提供了一个只需两行代码就可以在任何项目中使用的字典。

*   使用 pip 安装 PyDictionary。

```
pip install PyDictionary
```

*   导入并创建它的一个实例。

```
#Output{'Noun': ['a creation spoken or written or composed extemporaneously (without prior preparation', 'an unplanned expedient', 'a performance given extempore without planning or preparation']}
```

它为这个词提供了所有不同的现有含义。

## **3。自动校正器**

我们的手机都有自动纠错功能。如果我们在编程中需要它来检查语法单词以外的拼写错误呢？嗯，我们也有图书馆！

顾名思义，auto_corrector 做的是同样的事情。它纠正任何给定文本中的拼写。

*   使用 pip 安装 auto_corrector。

```
pip install auto_corrector
```

*   导入它并通过拼错一个单词来检查它。

```
#OutputPython is an amazing language.
```

## 4.皮肖特纳人

我们经常使用开源的 URL 缩写，为什么不用 python 呢？

pyshorteners 为我们提供了一个简单易用的工具来缩短 URL。

*   使用 pip 安装。

```
pip install pyshorteners
```

*   导入它，输入您想要缩短的 URL，并遵循给定的代码行。

```
#OutputEnter the url:https://www.github.com/mahisha-patel/
[https://tinyurl.com/y7rt6nxs](https://tinyurl.com/y7rt6nxs)
```

## 5.PyQRCode

如今，二维码被广泛用于各种目的。PyQRCode 为任何给定的输入字符串生成一个 QR 码。

*   使用 pip 安装 pyqrcode。
*   另外，安装 pypng，将生成的二维码保存为 png 格式。

```
pip install pyqrcode
pip install pypng
```

*   导入两个已安装的库，通过给出任何所需的输入字符串来创建 QR 码，并将其保存为 png 格式。

```
#Outputmahisha
QR generated
```

![](img/536aa2179f6efeac19232320582778b7.png)

使用上述代码生成的 QR 代码

因此，通过使用这些库，您可以构建高效而简单的项目。除了上面提到的这些，我们在 Python 中还有更多这样有趣的库。

感谢您的阅读！祝你今天开心！玩的开心！！😉