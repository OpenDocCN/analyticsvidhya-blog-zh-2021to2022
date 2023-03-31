# 反向传播算法背后的数学是如何工作的？数学时间！！！

> 原文：<https://medium.com/analytics-vidhya/back-propagation-algorithm-170b5f16790a?source=collection_archive---------9----------------------->

## 反向传播方法

在本文中，我们将讨论向前传递和向后传播的逐步方法。我将使用背页的数学，因为它被深入讨论。

3 层神经网络将帮助我们以简化的方式理解传播。

![](img/4d53743afbbe46b18c7dd31efa3f74be.png)

三层神经网络

让我们从计算隐藏层和输出层的值开始。

***隐藏层:h1 和 h2***

![](img/9d54e54379a69f283ebf98539b91d570.png)

***输出层:y1 和 y2***

![](img/49f89014a37a17fa86deb3a031365804.png)

**节点 y1 和 y2 中的误差，相加得到总误差。**

![](img/d8d9732acd4fce8c19527dd190f7c234.png)

# 反向传播

输出图层的值(y1=0.01 和 y2=1.0)与计算值(y1 = 0.894606 和 y2 = 0.908053)之间存在差异。为了减少误差，我们必须相应地调整权重。

> 考虑重量 w5，应用链式法则。

![](img/738bf0e2fbc2d78889dcfe3be6cb5ed6.png)![](img/1ad4b535e166d1dcfd1fe2ec5d951600.png)

如果我们同时处理所有的偏导数，事情就变得复杂了。所以，我们会一一解决。

![](img/22a5457ffaba01cc88821203401ece2f.png)![](img/25d40582532ab4bfa9ab7ae8e203fdee.png)

代入方程 7 中的方程 8、9、10

![](img/d7c02ddd7b0e021daa608b510293f88f.png)

最后，更新权重 w5。这里假设学习率为η=0.4(可以从 0 到 1 变化)。

![](img/7ee390f7ec0507ad5a26c8a2e326ebe7.png)

> 现在，考虑一下 w6。

我们已经有了等式 8 和等式 9 的前两部分的值。求解方程的最后一部分，并代入方程 7。

![](img/bcd4129753e68971be6ebdbd45204f69.png)

以类似的方式，我们更新权重 w7 和 w8。

![](img/b67b7e56dbcd59ac19852704d6062e1f.png)![](img/a119c1811a63b519a959439e4b6efd17.png)

图片网址:[https://sef iks . com/2017/06/21/homer-Simpson-guide-to-back propagation/](https://sefiks.com/2017/06/21/homer-simpson-guide-to-backpropagation/)

在解决了隐藏层和输出层之间的层之后，我们将更新隐藏层和输入层之间的权重。

权重 w1 可以通过一系列等式来更新。因为它可能很复杂，我用箭头来表示方程的连续链。

![](img/731dce32ec38e54ca8c5b9bb2969f23e.png)

以相同的方式，计算由于等式 16 中的隐藏节点 h1 而导致的 out 节点 y2 中的误差。

![](img/51fb2f933795300abf42396c36b0a355.png)![](img/e3002d657c6ea05734bcf2a2eecac1a8.png)

将公式 23 替换为公式 22

![](img/1d99becac122c3be85268901570226fc.png)![](img/3faadaafde70c159dafe3369fe2d054d.png)

现在将等式 24 和 25 代入等式 21

![](img/e49468214b8d25233688fa2385383ca4.png)

我们现在有了等式的第一部分。通过将等式 26 和 20 代入等式 16

![](img/d544299b297bfeb2e99a417ca2da51d6.png)![](img/76dce3449676b56f8e9a10c0f88af860.png)

我们通过将等式 27 和 28 代入等式 15 得到最终结果

![](img/49fd7f1ceaf9c17edd32eba344bed471.png)

通过将误差乘以学习率来更新权重 w1。

![](img/380e2e5e985e10093b901936f9e20c58.png)

现在，计算更新后的权重 w2，如下所示。但是它更快，因为我们已经有了方程 27 和 28 的值。唯一的变化是等式的最后一部分。即重量。

![](img/8082fa6a4abc71a57405f1b3e573da0d.png)![](img/4d742578024e8e78ecd33d0797867135.png)

按照相同的方法计算更新的权重 w3 和 w4。

![](img/fc90af5983aafab0acfd2c12a0b6c6ba.png)

这就完成了第一次迭代。并且被迭代直到误差被最小化。

谢谢你的时间。愿 MxA 与你同在:)