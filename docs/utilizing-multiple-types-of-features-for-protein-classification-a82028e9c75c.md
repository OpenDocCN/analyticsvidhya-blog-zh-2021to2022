# 利用多种类型的特征进行蛋白质分类！

> 原文：<https://medium.com/analytics-vidhya/utilizing-multiple-types-of-features-for-protein-classification-a82028e9c75c?source=collection_archive---------28----------------------->

用于蛋白质分类的大多数当前和标准方法依赖于用于蛋白质序列的递归神经网络；这些网络然后预测蛋白质分类。然而，这个特殊的项目是为了尝试看看我们是否可以使用其他容易实现的功能来帮助预测。结果表明还有很大的改进空间。

![](img/29c6532bcc761977f59846f271334770.png)

所用数据:[https://www.kaggle.com/shahir/protein-data-set](https://www.kaggle.com/shahir/protein-data-set)

现在，我们直接跳进去怎么样？

我们先稍微摆弄一下数据。为了做到这一点，我们首先需要获得数据。Kaggle 在这一点上做得很好，因为我们不需要为了使用数据集而将它们下载到我们的本地机器上。相反，由于它既是一个在线笔记本编辑器，也是一个分享任何有关数据科学(包括数据集)的社区，我们可以访问上面的链接，只需这样做:

![](img/d699909960028b349e7ee3e3c65cf400.png)

单击“新建笔记本”按钮，用已安装的数据创建一个笔记本！

准备工作结束后，我们终于可以开始编码了！

初始数据清理过程的大部分代码直接取自本文参考资料一节中链接的笔记本。

我们应该做的第一件事是把数据读入熊猫的数据框。你会注意到数据分为两部分。csv 文件。

![](img/b3632476422c6d15bc7238e422201fa3.png)![](img/b3c0ecdc54f211c417b03a9e9fa554cb.png)

现在让我们把这两个数据帧合并成一个！

![](img/1a072891673815bb741593429e4b812a.png)

在这之后，我们只是做一些基本的数据探索和清理之类的事情。我们将在本文中快速浏览这个过程，但是如果您想查看细节，请确保查看完整的笔记本。你可以在我的 GitHub 上找到，下面有链接！

首先，我们删除所有空值。然后，我们确保删除所有涉及非蛋白质大分子的行。

![](img/88007126099e9665f68931fd6f9edf0b.png)

以下是一些有趣的发现:

![](img/60c6a1b2d484137bccd7c4d19cff393c.png)

接下来，让我们将所有的数据合并成一个单一的特征！

![](img/b0c6c710cc6e3f49d622c59ce50b6daa.png)![](img/56b5ffd3b8e5c8fd9edb415ee90a28e7.png)

接下来，是时候为我们的最终模型准备数据了！

![](img/c90278b69e310edebbe7858639d93147.png)

记得进口我们需要的工具。

![](img/b89ffe6107ef03460d0b07491b7efc20.png)

现在，是你们期待已久的时候了…实际模型！

![](img/2847ee2764d543c45afff06871658ed7.png)

是时候训练它了！

![](img/0e95a169ba65fc20ead4a51b36d06221.png)![](img/be0762660dbf8e086e31037b78143171.png)![](img/6427b5204222943f26ebdf6e5481542e.png)

好了，现在我们需要弄清楚我们的模型表现如何。

![](img/4b6dfd0ff0f9b519ca92f7061e13e82a.png)

压轴戏！我们的困惑矩阵…

![](img/41321c1685063784f0e7aba326028f4a.png)![](img/1d0cb062392ee5e58e91f59308101531.png)

Welp。如你所见，我们的模型挣扎了很久。尽管如此，我还是希望你们能从中学到一些东西！

完整的笔记本可以在我的 GitHub 上找到:【https://github.com/AAbhi256 

# 参考

[](https://www.kaggle.com/helmehelmuto/cnn-keras-and-innvestigate) [## CNN keras 和 innvestigate

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自结构蛋白质序列的数据

www.kaggle.com](https://www.kaggle.com/helmehelmuto/cnn-keras-and-innvestigate) 

[https://towards data science . com/a-comprehensive-guide-to-correlation-neural-network-with-keras-3f 7886028 e4a](https://towardsdatascience.com/a-comprehensive-guide-to-correlational-neural-network-with-keras-3f7886028e4a)

[https://snap . Stanford . edu/snappy/doc/reference/multimodal . html](https://snap.stanford.edu/snappy/doc/reference/multimodal.html)

【https://open access . the CVF . com/content _ cvpr _ 2017/papers/Chen _ AMC _ Attention _ guided _ 2017 _ paper . pdf