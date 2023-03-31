# ConvNets 的引擎盖下发生了什么？

> 原文：<https://medium.com/analytics-vidhya/whats-going-on-under-the-hood-of-convnets-94de1d4a6ec4?source=collection_archive---------18----------------------->

![](img/d3fac43fc6926d91875044d625750815.png)

一个典型的分类网络。

在过去的十年里，[卷积网络](https://towardsdatascience.com/an-introduction-to-convolutional-neural-networks-eb0b60b58fd7)经历了前所未有的发展，因为它们已经证明了自己在处理视觉数据方面的强大能力，并且随着时间的推移，我们目睹了这种网络的复杂性逐渐增加，这是广泛研究和随后在该领域出现的众多应用的结果。但是，当我们使我们的网络越来越深时，我们往往倾向于将它们视为黑魔盒，而我们可以从“隐藏”层解释太多东西，这些“隐藏”层可以用来理解我们的网络如何试图降低可怕的损失函数的值，此外，窥视网络的内部也可以让我们了解我们应该如何改变超参数，如卷积的大小或用于提高网络性能的池类型。

# 那么，如何看到正在发生的事情呢？

有几种方法可以直观地显示网络各层试图寻找的内容，在下一节中，我们将讨论其中一些最突出的方法。

## 1.可视化过滤器重量:-

在这种方法中，我们将权重归一化到 0 到 255 之间，并将这些值用作 [RGB 值](https://en.wikipedia.org/wiki/RGB_color_model)，以将这些值可视化为图像，并尝试解释哪种特征可能会导致它们最大程度地激活。

![](img/7c27d1a6232c743e03c9bd567195e03c.png)

这里显示了一个 ConvNet 的第一层滤波器，它们形成各种模式，当输入类似于这个特定模式时，它们将最大程度地激活。

这种方法背后的想法是，过滤器权重的可视化使我们能够解释在给定的示例中他们可能在寻找什么样的东西，例如，上图中所示的许多过滤器似乎在寻找直线或斜线，因为当输入越来越类似于仅看到的模式时，它们会最大限度地激活。我们可以用两个向量的[点积](https://en.wikipedia.org/wiki/Dot_product)来想象。它们越“相似”，即它们的方向越相似，点积的值就越高，我们对图像的卷积运算非常类似于矢量点积，这使得它成为理解更复杂的卷积运算的性质的理想候选。

这种方法的一个限制是，虽然第一层的过滤器可以被解释为寻找一些边缘、斑点等，但是这样的事情对于后面的层是不太可能的，因为它们不显示任何可能被人眼解释的图案，因此没有太多的意义或含义，这解释了为什么它被更不寻常地使用。

## 2.最近邻对象:-

在这种方法中，我们使用最后一层的激活来度量最后一层的特征空间中各种示例之间的距离。这种方法背后的基本思想是，在最后一层的特征空间中，两个对象越接近，它们在网络考虑的特征方面就越相似，这可以帮助我们理解我们的网络正在使用什么样的特征来执行手头的任务。

![](img/fbcbe0e1688752ec7c1e373bf5b5e401.png)

这里我们可以看到，在最后一层的特征空间中，图像的最近邻居是与其非常相似的其他图像，将其与像素空间(即每个像素代表一个维度的空间)中图像的最近邻居进行比较，我们可以清楚地看到网络可以很好地识别图像中的特征，即使就像素而言，它们在每个示例中变化很大。

这种方法的一个有趣的使用案例可以是分类，让我们借助一个简单的例子来理解它:-假设你想建立一个分类器网络来区分轿车和越野车，在这种情况下，很可能大部分越野车的图像是在泥泞的环境中，而轿车主要是在城市环境中拍摄的。现在，对于我们的网络来说，使用背景环境而不是实际车辆来对给定的示例进行分类是非常诱人的，但是我们希望将每个示例分类到正确的类别中，而不管背景环境是什么！

这里，我们可以对最后一个 conv 层使用最近邻方法来检查越野车在城市环境中是否更接近其他类别的对象。

因此，这个例子说明了这种方法如何帮助我们窥视网络的内部工作，并让我们发现一些可疑的东西。

## 3.使用降维:-

另一种将网络最后一层的激活可视化的流行技术是使用[维度缩减](https://en.wikipedia.org/wiki/Dimensionality_reduction)以二维方式查看网络的最后一层激活。这项任务可以通过多种方式完成，最流行的是线性变换的[主成分分析](https://www.youtube.com/watch?v=FgakZw6K1QQ)，同时也存在其他非线性方法，如 [t-SNE](https://towardsdatascience.com/an-introduction-to-t-sne-with-python-example-5a3a293108d1) 。

![](img/8aa5980b6e5cfe4d83e9687a5ca61047.png)

这种降维技巧有助于我们更好地形象化各种例子如何存在于网络的特征空间中。

## 4.最大化激活区域/闭塞技术:-

在这种方法中，我们试图通过去除或扭曲图像数据区域并观察由这种去除和/或扭曲引起的网络结果的变化，来识别对网络的决策过程贡献最大的图像数据区域。作为这种方法的自然延伸，我们可以创建[热图](https://en.wikipedia.org/wiki/Heat_map)，标出对决策过程贡献最大的区域。

![](img/0d7a3c6537b3c0257c8c0cf88ce37baf.png)![](img/00415ea457f25b94bcc608309cfb2431.png)

这里，我们在测试模式下通过网络传递一幅图像，然后传递同一幅图像的变形形式，以确定图像的哪个部分引起网络输出的最大变化，这也称为遮挡技术。

这种方法在最近邻居部分讨论的场景中也非常有用，因为它可以非常容易地帮助我们识别网络主要使用背景环境来分类图像，而不是车辆本身。

## 5.通过引导式反向投影了解功能:-

这种方法与[生成对抗网络](https://machinelearningmastery.com/what-are-generative-adversarial-networks-gans/)非常相似，因为这两种方法都采用网络中的特定神经元，并进行反向传播，以最大化该特定神经元的价值。虽然在 GAN 的情况下，这种神经元通常位于输出层，但为了理解特定神经元的作用或导致不同神经元激活的特定特征，我们也可以使用中间层的神经元。特别是在这种情况下，我们使用导向反向传播来实现。由于这种引导性反向传播技术并不普遍，如果我们借助下面的视频来理解它，可能会有所帮助

现在我们已经清楚了引导反向传播，我们可以看到当我们在一些一般的例子中使用这些方法的结果是什么。

![](img/ac7978182f01b6b05c84118907429777.png)

这里，在左侧我们可以看到生成的图像最大程度地激活了对象分类网络中的给定神经元，而在右侧我们可以看到最大程度地激活同一神经元的示例。

这种技术将有助于我们识别神经元在寻找什么，但左侧生成的图像肯定不是很能说明问题，因此我们可以在引导反向传播过程中添加一个正则化项，这将有助于我们“自然化”图像。

# 结论

通过这篇文章，我们看到了如何使用各种技术来窥视我们的网络，了解它的一点内部功能，并确定我们的模型是否正在做它应该做的事情，或者它是否正在使用由网络可能已被训练的劣质数据引起的一些漏洞。除了提高我们网络的性能，这些方法还帮助我们更深入地了解神经网络，在许多情况下，神经网络被认为只是一个具有魔力的黑盒。

# 链接/资源

 [## 重定向你-媒体

### 编辑描述

medium.com](https://medium.com/r?url=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-convolutional-neural-networks-eb0b60b58fd7) [](https://en.wikipedia.org/wiki/Dot_product) [## 点积-维基百科

### 在数学中，点积或标量积是一种代数运算，它采用两个等长的序列…

en.wikipedia.org](https://en.wikipedia.org/wiki/Dot_product) [](https://en.wikipedia.org/wiki/Dimensionality_reduction) [## 维度缩减-维基百科

### 降维，或称降维，是将数据从高维空间转换到多维空间

en.wikipedia.org](https://en.wikipedia.org/wiki/Dimensionality_reduction) [](https://towardsdatascience.com/an-introduction-to-t-sne-with-python-example-5a3a293108d1) [## 用 Python 例子介绍 t-SNE

### 介绍

以 Python 为例 Introductiontowardsdatascience.com 的 t-SNE](https://towardsdatascience.com/an-introduction-to-t-sne-with-python-example-5a3a293108d1) [](https://en.wikipedia.org/wiki/Heat_map) [## 热图-维基百科

### 热图(或热图)是一种数据可视化技术，以两种颜色显示现象的大小

en.wikipedia.org](https://en.wikipedia.org/wiki/Heat_map) [](https://machinelearningmastery.com/what-are-generative-adversarial-networks-gans/) [## 生成性对抗网络(GANs)的温和介绍-机器学习掌握

### 生成对抗网络，简称 GANs，是一种使用深度学习方法的生成建模方法…

machinelearningmastery.com](https://machinelearningmastery.com/what-are-generative-adversarial-networks-gans/) 

最后，我要感谢你们所有人！