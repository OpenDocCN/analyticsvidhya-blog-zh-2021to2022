# Python 中的多处理图像进化

> 原文：<https://medium.com/analytics-vidhya/multi-processing-image-evolution-in-python-be5e7eedded5?source=collection_archive---------11----------------------->

![](img/742a81738086243cb7c635a81c090900.png)![](img/85c79385d0cdf80cf74630ee086587c0.png)![](img/742a81738086243cb7c635a81c090900.png)

蒙娜丽莎重建

大家好！，这次我有非常迷人的动画！我们将发展使用少量多边形重建的图像，通过 Python 中的双重退火优化算法进行优化。

我想你在看到上面的 evolution GIF 后可能会感兴趣，事实上，我们将快速浏览一下它是如何产生的，以及它是如何表现为分布式作业以更好地利用 CPU 的。

起初，我主要受到两分钟论文频道的[大集的启发，其中将光栅图像转换为矢量图像的过程，而视频中提到的方法是一种强大的基于深度学习的方法，可干扰可区分的图形，还有另一种流行的方法，通常非常简单地使用多边形或图形形状来近似/重建图像，方法是将重建过程作为一个优化问题，使用无梯度优化算法(如进化算法)和不同种类的自然启发算法(如模拟退火)进行优化。](https://www.youtube.com/watch?v=JmVQJg-glYA)

![](img/24881ce673a5cb7b3c4efdd2722e1711.png)

光栅与矢量图像[我没有自己的权利]vector-conversions.com

可区分图形是一个活跃的研究点，因为[视频](https://www.youtube.com/watch?v=JmVQJg-glYA)围绕着这种新引入的方法，但尽管如此，我们仍然能够通过相当简单的方法来完成这一重建过程，并且仍然可以用几行代码获得某种可接受的结果，这就是我们在这里要做的。🏁🏁

当我写这篇文章的时候，在情人节，用这段代码重建你的灵魂伴侣形象是合法的，但不幸的是，我的圈子里没有这样的灵魂伴侣🤣🤣。

除此之外，让我们给这个问题一个正式的定义，我们希望找到一组最佳的形状，它们有相应的颜色和中心，尽可能地接近参考图像，在我们的例子中是蒙娜丽莎的图像。
这意味着在优化步骤中，渲染器应该以多边形或形状叠加的形式渲染图像(图像有一个允许叠加的 alpha 通道)。([在线直播演示](https://alteredqualia.com/visualization/evolve/)我不拥有)

值得注意的是，寻找具有相应颜色的形状的合理配置的过程可以使用各种算法和技术来解决，其中一些可能只基于图像分割和其他计算机视觉方面，这完全是算法，不涉及任何类型的优化。正如我们将要做的那样，我在搜索过程中遇到了[这个令人印象深刻的作品](http://staff.ustc.edu.cn/~xjchen99/lowpoly/lowpoly.htm)。但是正如上面提到的，在这篇文章中，我们将使用**全局优化器**来描述寻找最优配置的过程。

正如大多数人所知，优化一直是许多研究人员的兴趣所在，因为它在一般工程和运筹学中非常重要，例如调整 PID 控制器或神经网络的参数，这在当今非常流行，并彻底改变了我们的生活。
但对于那些熟悉深度学习和一般神经网络的人来说，确实知道神经网络大多是使用基于梯度的方法进行优化的，因为神经网络计算一个明确定义的函数(除了 RL ),可以使用链规则的直接应用找到该函数的权重梯度，从而我们可以使用基于梯度的优化器(用于深度学习的梯度下降优化器家族)来迭代优化和找到参数的良好配置。

**相比之下，**我们今天要处理的问题本身就涉及到不可微运算，即光栅化过程或者说是绘图画布中多边形的渲染。(可微分光栅化是一个热门的研究课题，如前面的**两分钟论文**频道的视频)

那么 **1)** 怎么可能优化一个函数并找到全局最小值或似是而非的局部最小值呢？如果这很有效， **2)** 为什么我们不能用它们来找到我最喜欢的神经网络的最佳参数🤗 🤗？

1.  有一组算法称为元启发式算法，包括模仿进化过程的进化算法，如遗传算法，以及模仿不同种类的动物和蜜蜂及蚂蚁的自然启发算法。
    这些算法是基于群体的算法，其中多个解决方案相互竞争并不断进化，反复应用自然法则，并根据类似于解决方案有多好的**适应度**函数或误差函数来寻找更好的解决方案。这些算法对文献中的某些问题非常有效，是数值优化和运筹学的一个活跃的研究领域。
    [免责声明:优化领域是一个广泛的研究领域，还有其他不属于元启发式算法的优化器]
2.  老实说，这些全局优化器/无梯度优化器可以用于优化神经网络，尽管经验表明基于梯度的方法是优化神经网络的途径，我猜想这是因为基于梯度的方法利用了我们对明确定义的函数的梯度的先验知识，而不是将其视为无梯度优化器的**黑盒**。

足够的术语，让我们给出一点细节，对于每个优化问题，我们应该以**数据结构**的形式表示**世界状态**以开始操作过程，在我们的情况下，我们作为人类要观察的是生成的矢量图像(世界状态)，正如你从基本的计算机图形学课程中所知道的，渲染图像是通过一组原始形状(如多边形)及其相应的颜色来完成的，这可以简单地表示为长矢量**编码**该数据的形式。

下图描述了光栅化状态的编码过程，即:1 .**红色**矩形棱柱表示一组大小为
[num_polygon x 2 x sides]的多边形，例如，我使用了 250 个多边形，每个多边形有 3 条边(矩形)，2 表示坐标 X Y，因为我们正在二维空间中绘制点。
2。**黄色**矩形代表每个多边形对应的 RGBA(四维)颜色，这就是它们之间共享高度(紫色)的原因。

您可能会猜测底部的长状态向量(从红色到黄色的渐变填充)是串联的黄色和红色张量的展平版本。

![](img/a15bbf5e5b97da44e1d60ee95e76b0bb.png)

状态向量编码的图示

将问题状态转换为平面向量的过程称为**编码**，反向操作称为**解码**，优化算法将编码状态作为平面向量进行扰动，然后将它们解码并发送到光栅化器进行渲染，并通过重建图像和参考图像之间非常简单的 L2 元素距离来获得误差。(下图展示了优化过程和损失意义，这是迭代完成的)

![](img/8cdff37756d24dbe7dec53e736fd1eb0.png)

左边是 iter:7，损耗为. 1875，右边是 iter:7200，损耗值为. 0636

你可以看看**00 . low poly _ distributed . py**文件中的那些操作，在函数 encode，decode，custom_fitness 中。

![](img/6e6dc19709b3d63744d1cb6591c0a32a.png)

用于编码、解码和计算每个状态向量的适合度的函数

使用上述函数的设置，我们准备使用任何无梯度优化算法来重建矢量图像，我首先使用遗传算法，它很好，然后求助于随机分形搜索(你可以查看我的[博客文章](/analytics-vidhya/stochastic-fratral-search-algorithm-a54ea41f0858)中的内容)，这是另一种基于分形概念的优化算法，但它很糟糕，然后我尝试了 scipy 附带的[优化器， 它真的有各种各样的优化器，有很多选项，我遇到了](https://docs.scipy.org/doc/scipy/reference/optimize.html)[双重退火](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.dual_annealing.html)优化算法，它工作得非常好！

根据 scipy 文档，双重退火优化算法是模拟退火的改进版本(受[冶金学](https://en.wikipedia.org/wiki/Annealing_(metallurgy))的启发，模拟材料的加热和受控冷却，以增加其晶体的尺寸并减少其缺陷),通过合并一种策略来改进由广义退火过程找到的解决方案。

在这种情况下，我们可以为双重退火优化提供我们的定制适应度函数，并做一些图形工作，以实时了解正在发生的事情，并开始演化图像，就这样😇。

但是，**多重加工是怎样有益的呢？
🎉在绘制到共享画布时，每一个都被提供给一个进程🤔这在 python 多处理中并不简单，因为默认情况下全局变量是不共享的，它们只是被复制，为此我在 stackoverflow 上找到了[这个很好的答案。](https://stackoverflow.com/questions/7894791/use-numpy-array-in-shared-memory-for-multiprocessing/)**

优化过程

# 额外乐趣🐶：

嗯，下一部分将使用这个低多边形表示来创建一些奇特的视觉效果。
随着优化过程的完成，pickle 文件包含为这 16 个子图像演化的所有多边形的字典。

1)为了生成上面的优化动画，我只需每隔几分钟将全局共享画布保存到磁盘，并简单地以视频格式或 GIF 呈现它们。
我在 **01.remove_duplicate.py** 文件([py image search tutorial](https://www.pyimagesearch.com/2020/04/20/detect-and-remove-duplicate-images-from-a-dataset-for-deep-learning/))
中做了一些基于哈希移除重复后续帧的实验，我还以衰减的方式移除图像(当你到达视频末尾时，丢弃更多图像)，如 **02。drop frames.py，**然后使用脚本**03 . generate _ video _ cmd . sh**和 **04.generate_gif.sh，**从图像序列中创建视频和 gif。

2)我还使用 **06.generate_svg.py，**创建了一个 SVG 格式的矢量图形和 PDF，正如你已经知道的，这不是一个图像，而是一组相互叠加的形状**。**

PDF 格式的蒙娜丽莎🐖

3)花式叠加动画
该动画由 **05 生成。动画多边形 effect.py，**其中演化的多边形通过遵循随机轨迹彼此重叠。

我知道那是一篇相当长的文章🏁😅，谢谢你的耐心。希望你喜欢🙏

更多详情请查看 github repo。

我使用 gifski 开源工具来生成高质量的 gif🎊🎊

# 参考资料:

1.github repo:https://github.com/mohammed-elkomy/low-poly-image-evolution

2.在 python 多进程中共享全局状态:(我们的情况不需要锁，因为每个进程写入一个特定的子映像，从而简化了实现)[https://stack overflow . com/questions/7894791/use-numpy-array-in-shared-memory-for-multi processing/](https://stackoverflow.com/questions/7894791/use-numpy-array-in-shared-memory-for-multiprocessing/)

3.为每个子流程重新播种[https://stack overflow . com/questions/32172054/how-can-I-retrieve-the-current-seed-of-numpys-random-num-generator](https://stackoverflow.com/questions/32172054/how-can-i-retrieve-the-current-seed-of-numpys-random-number-generator)

4.使用 dhashing 移除重复图像:https://www . pyimagesearch . com/2020/04/20/detect-and-remove-duplicate-images-from-a-dataset-for-deep-learning/

5.Scipy **双重退火**:[https://docs . scipy . org/doc/scipy/reference/generated/scipy . optimize . dual _ annealing . html](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.dual_annealing.html)

6.我使用了**贝塞尔**曲线来获得更平滑的轨迹(我遇到了这个很棒的解决方案)[https://stack overflow . com/questions/50731785/create-random-shape-contour-using-matplotlib/50732357](https://stackoverflow.com/questions/50731785/create-random-shape-contour-using-matplotlib/50732357)

7. **ffmepg** 视频创作项目:[https://ffmpeg.org/](https://ffmpeg.org/)

7.**吉夫斯基**:[https://gif.ski/](https://gif.ski/)