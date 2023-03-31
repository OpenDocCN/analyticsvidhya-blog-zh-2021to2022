# 用最先进的深度神经网络建立可解释的预测模型

> 原文：<https://medium.com/analytics-vidhya/building-explainable-forecasting-models-with-state-of-the-art-deep-neural-networks-using-a-ad3fa5844fef?source=collection_archive---------0----------------------->

## 低代码解决方案

用民主化和可解释的人工智能框架重新审视几十年的老问题——我们能多精确地预测未来和理解因果因素？

![](img/39e6aa176c1a61f83c4454892383ed55.png)

作者图片:模型可解释性的直观表示&用 DeepXF 进行深度预测

朋友们好，

通过这篇文章，我们将讨论一个关键的永恒的商业问题，即**预测**。我们将看到一些关于一般预测的快速理论见解，随后是一个包含多个实际操作演示示例的分步教程。我们将看到 python 库 [**deep-xf**](https://github.com/ajayarunachalam/Deep_XF) 如何有助于轻松开始构建深度模型。此外，我们将了解如何估算模型偏差，以跟踪预测偏差&偏差。此外，我们将看到这个库的可解释性模块部分，它可以用来理解模型推理，并以简单的人类可理解的形式解释结果。关于快速介绍和所有可能被 **deep-xf** 包支持的用例应用，在博客文章[这里](https://ajay-arunachalam08.medium.com/introduction-to-deepxf-e90ce7c2858c)列出。此外，从[这里](https://ajay-arunachalam08.medium.com/interpretable-nowcasting-with-deepxf-using-minimal-code-6b16a76ca52f)查看我的另一篇密切相关的关于**临近预报**的博文。

# 重新审视预测问题

随着低成本硬件(传感器)、物联网(IoT)和云架构的出现，数据采集、数据传输和数据存储不再是一个关键问题。如今，我们遇到了一堆流媒体和可穿戴设备，可以轻松地全天候测量多个参数。过去，时间序列预测已经被线性、增强、集成和统计方法很好地解释了。随着深度学习算法的进步，人们当然可以充分利用它的潜在优势，因为到目前为止我们已经看到了与深度学习技术相关的巨大可能性。我们都知道几个与时间序列流相关的用例，其中[深度学习](https://arxiv.org/pdf/2005.11650.pdf)非常关键。让我们先用通俗的语言来理解预测的含义，其次是机器学习的语境中的预测问题公式化。**预测是对未来的预测或有根据的猜测。简单来说，就是像说未来有可能发生的事情**。下图展示了一个非常基本的预测模型结构，带有输入&输出符号。此外，我们看到如何预测问题可以制定给定的历史时间步长输入序列。

![](img/77f1ad108f49b8d60d456e2005cecb0d.png)

作者图片:简单预测结构和预测问题公式

要更直观地了解时序数据的详细说明，请点击查看[。并且，对于来自时间序列数据的无监督特征选择，检查](https://ajay-arunachalam08.medium.com/multidimensional-multi-sensor-time-series-data-analysis-framework-5c497d8d106b)[这里的](https://ajay-arunachalam08.medium.com/multi-dimensional-time-series-data-analysis-unsupervised-feature-selection-with-msda-package-430900a3829a)。

## 用于预测的传统方法/技术

**定性预测**基于纯粹的意见、直觉和假设。而**定量预测**使用统计、数学和机器学习模型，根据历史数据进行预测。

从上下文到**财务建模**首选的简单方法如下表所示(但不限于此)。

![](img/b9d3110a92ec56f31572ad12babf2674.png)

作者图片:预测未来值的预测方法的基本类型

我不会过多地讨论预测的理论概念，因为这根本不是这篇博文的重点，而且我也不会在这里谈论不同的最先进的深度学习网络架构，因为已经有大量有用的材料可供使用。那么，让我们直接进入演示。

# 入门指南

我们将看到如何轻松地使用 [**Deep-XF**](https://pypi.org/project/deep-xf/) 库构建深度预测模型。对于安装，遵循给定的步骤[此处](https://github.com/ajayarunachalam/Deep_XF#installation) &对于任何其他手动先决条件安装检查[此处](https://github.com/ajayarunachalam/Deep_XF#requirements)。我们从导入库开始，如下面的步骤所示。接下来，我们为基于深度学习的预测管道设置定制预配置，使用 **set_model_config ()** 函数设置参数，方法是选择适当的用户指定算法[即，RNN、LSTM、GRU、BIRNN、BIGRU]、输入缩放器(如:MinMaxScaler、StandardScaler、MaxAbsScaler、RobustScaler)，并使用过去的单个周期窗口作为输入，依次预测未来值。接下来， **set_variables ()** 和 **get_variables ()** 函数用于设置和解析时间戳索引，指定感兴趣的预测结果变量，对数据进行排序，删除重复项等。此外，我们还可以使用**hyperparameter _ config()**函数，从**学习率**、**漏失**、**权重衰减**、**批量**、**时期**等中选取一堆关键超参数，选择自定义超参数来训练预测模型。

## 导入 Deep-XF 库

## 定制用户特定的预配置

## 设置自定义模型超参数

# **探索和准备数据**

设置好所有配置后，我们导入数据，执行探索性数据分析，预处理数据，训练深度模型，用一系列关键的重要指标评估模型性能，然后将预测结果保存并输出到磁盘，并将其绘制为用户提供的预测窗口的预测结果的迭代绘图。接下来，如果我们的模型没有超出/低于预测，我们估计预测偏差以进行验证。最后，可解释性模块帮助我们理解/解释用户输入的预测样本的推断预测的模型结果。

我们将在这里看到两个不同的用例。第一种是使用阿里云虚拟机的数据，这是一个多变量数据集，具有秒频率间隔(即每 10 秒)。原始数据集可从[这里](https://github.com/ajayarunachalam/msda/blob/main/data/m1.csv)获得。而第二个是销售&营销数据，它也是一个多变量数据集，具有月频率间隔。原始数据集可以从[这里](https://github.com/ajayarunachalam/Deep_XF/blob/main/data/Sales_and_Marketing.csv)获取。

> 秒频率数据示例—

让我们首先通过显示几行来查看数据。正如所见，我们有 6 列，包括时间戳索引。

![](img/9c3973d678e5deabd0a3ec510de26009.png)

作者图片:显示阿里云虚拟机数据集的前几行

接下来，让我们使用简单的熊猫 **info()** 函数来获取数据类型信息

![](img/bd06c5a3a8ca575cf69aa9dbaf255724.png)

按作者分类的图像:获取列信息

接下来，我们使用直观的 **plot_dataset ()** 函数，借助如下所示的 plotly 交互式图表，可视化结果列 wrt 时间指数的趋势。

![](img/da454200e72cc41f42f5b51a1dfa6012.png)

作者图片:交互式趋势可视化图

此外，我们继续做特征工程。[特征工程](https://en.wikipedia.org/wiki/Feature_engineering)是机器学习生命周期的关键组成部分之一。简单来说，它是指使用领域知识来选择、转换最相关的变量，并沿着该过程从原始数据中导出新特征的过程。我们首先使用**generate _ date _ time _ features _ hour()**函数从时间戳生成简单特征，然后使用**generate _ cyclic _ features()**方法从日期、时间生成循环特征，还使用**generate _ other _ related _ features()**方法生成与时间序列数据上下文相关的其他特征。在这里，人们也可以使用滞后的观察作为特征。此外，还可以导出一系列其他有用的特征，包括各种聚合窗口中的值的趋势:即平均值的变化和变化率，标准偏差。特定窗口内的偏差、变化率、增长率和标准差。偏差、随时间的变化、随时间的变化率、增长或衰减、增长或衰减率、高于或低于平均阈值的数值计数等。如这里的[所示](https://ajay-arunachalam08.medium.com/multidimensional-multi-sensor-time-series-data-analysis-framework-5c497d8d106b)。

> 月度频率数据示例—

下图显示了数据的摘录。如图所示，它包括两个属性，即“销售额”和“营销费用”,其中“时间段”作为时间戳索引。

![](img/6218ed1aead0e370664d74032b9c263d.png)

按作者分类的图像:显示销售和营销数据集中的前几行

接下来，我们将数据可视化，如下图所示，并带有结果属性(例如，本例中的销售额)

![](img/9eb1b27935edd1846f6b237d3f865022.png)

作者图片:关于预测属性的交互式图表

接下来，对于特征工程，我们使用**generate _ date _ time _ features _ month()**函数，然后使用**generate _ cyclic _ features()**方法从日期生成循环特征，还使用**generate _ other _ related _ features()**方法生成与时间序列数据的上下文相关的其他特征。在这里，人们也可以使用滞后的观察作为特征。此外，还可以衍生出一些其他有用的特性，如这里的[所示](https://ajay-arunachalam08.medium.com/multidimensional-multi-sensor-time-series-data-analysis-framework-5c497d8d106b)。

# 为预测模型定型

一旦特征工程部分完成，接下来我们继续使用 **train ()** 方法训练模型，或者使用可用的 CPU/GPU 资源，同时提供已处理的数据帧、结果列，即，使用 **set/get** 方法重命名为“值”,训练-验证-测试集分割的分割比，以及所有其他先前预配置的参数作为该方法的输入。相应的训练结果如下所示。我们还绘制了模型预测值与实际值的对比图，如下图所示。这里描述的结果只是基于几个时期，没有进行任何超参数调整。人们当然可以进一步微调模型以提高性能。我们还使用广泛使用的评估指标来评估该模型，并绘制它们以进行并排比较。还为用户提供的时间窗口绘制了交互式预测图。预测模型结果也可以输出到磁盘，因为平面 csv 文件以及交互式可视化绘图可以轻松保存到磁盘。

![](img/40f8f66b215ea6697716329de0cd7351.png)

作者图片:显示训练-值-测试与可用计算资源的分割，以及训练/值损失。

**结果**

![](img/dc1f4ddee999d571355faf0c5865d826.png)

作者图片:模型训练-验证损失图

![](img/1976d7baef8e09882f31bf32248e7569.png)

作者图片:显示线性模型和深度模型预测的交互式可视化绘图

![](img/a23de974a79c76ff66b1c246231d167e.png)

图片作者:阿里云虚拟机数据集 cpu 利用率结果的预测模型验证

对于广泛使用的回归度量的简单直观的解释，请查看这个博客[这里](https://ajay-arunachalam08.medium.com/i-always-believe-in-democratizing-ai-and-machine-learning-and-spreading-the-knowledge-in-such-a-5343dab282aa)。此外，您还可以查看这个库“[**regressormetricgraphplot**](https://github.com/ajayarunachalam/RegressorMetricGraphPlot)**”**，它可用于简化几种广泛使用的回归评估指标的计算，并针对不同的广泛使用的机器学习算法轻松地用单行代码绘制它们，还可以进行并行比较。

让我们快速浏览一下默认的几个估计指标中的一个估计指标，如 r、MAE、RMSE、MAPE、RMSRE、方差得分等。**对称平均绝对百分比误差** (SMAPE)只是一种基于百分比(或相对)误差的精度测量。总的来说，百分比误差易于理解和解释。通俗地说，它给出了高于实际值的预测无限性的估计。

SMAPE 的计算如下面的 python 代码片段语法所示。

![](img/65b1c352a18999e9d96e7d45dccfbb6c.png)

按作者分类的图片:带有广泛使用的关键评估指标的模型性能条形图

![](img/f10cd4d86c291e446e1de63fe57137ac.png)

作者图片:预测的交互式可视化绘图

![](img/eaab478886ffd50819fb359c8c6c6fa9.png)

作者图片:针对销售结果的预测模型推论

**偏差估计&模型解释能力**

预测偏差可以描述为过度预测(即，预测大于实际)或预测不足(即，预测小于实际)的趋势，从而导致预测错误。预测偏差可以使用如下所示的公式进行估算。

![](img/3411ea91b1febfce3d81f3ece0de615d.png)

作者图片:预测偏差数学公式

该算法使用下面的 python 代码片段语法计算预测偏差，输出估计偏差(如下图所示),同时估计几个关键指标并绘制成条形图。

> **fc _ bias**':(NP . sum(NP . array(df . value)-NP . array(df . prediction))* 1.0/len(df . value))

![](img/6a3c879ae48c0e6d6607f112842b85ec.png)

作者图片:预测偏差估计

![](img/63c766a42f295ada46b4f01b6047e043.png)

按作者分类的图片:带预测模型评估指标的条形图

最后，我们继续使用**explable _ forecast()**方法，为所需的特定预测输入样本获取模型的解释。我们还绘制了 [**匀称值**](https://christophm.github.io/interpretable-ml-book/shapley.html) **s** 以了解特征值对特定模型推断的贡献。

![](img/10573f46bb49b4fbb15ed54a411cd43b.png)

作者图片:基于预测数据点的派生特征的模型可解释性

上图是预测数据样本的结果。可以看出，对基于导出的日期时间、周期和其他生成特征的结果列的相应模型推断有显著贡献的特征尤其是对特定月份的引用、一年中特定周的重要性、暗示相对于一周中相应日的重要性的特征、特定假日是否有任何重要意义等。

**注意:-** 这个包大体上仍然是一个正在进行的工作，所以总是欢迎您的评论，并且您认为应该包括的东西也应该包括在内。此外，如果你想成为一个贡献者，你总是最受欢迎的。请。如果你有兴趣以任何身份做出贡献，请联系我们。

RNN/LSTM/GRU/BiRNN/BiGRU 已经是初始版本发布的一部分，而 Spiking Neural Network、Graph Neural Network、Transformers、GAN、甘比、DeepCNN、DGCNN、LSTMAE 和许多其他软件正在测试中，一旦测试完成，将很快添加。

**结论**

我们已经看到了如何通过用户特定的自定义输入，只需几行代码就可以轻松构建基于直观深度学习的预测模型，而无需从头开始实际设计深度网络架构。我们浏览了预测场景的两个不同用例。

DeepXF 利用时间序列数据的内置工具，帮助设计复杂的预测和临近预报模型。使用这种简单、易用、低代码的解决方案，人们可以轻松地自动构建可解释的深度预测和临近预报模型。有了这个库，人们可以轻松地进行探索性的数据分析，包括数据分析、过滤异常值、创建单变量/多变量图、plotly 交互图、滚动窗口图、检测峰值等。并且还通过诸如查找缺失、输入缺失、日期-时间提取、单个时间戳生成、移除不想要的特征等服务来简化对他们的时间序列数据的数据预处理。人们还可以快速获得所提供的时间序列数据的描述性统计数据。这也是功能工程与服务的可能性，如生成时间滞后，日期-时间功能，一键编码，日期-时间循环功能等。此外，它还迎合了[信号数据](https://ajay-arunachalam08.medium.com/introduction-to-deepxf-e90ce7c2858c)的需要，在这里可以快速找到两个同类时序信号输入之间的相似性。它提供了从时序信号输入等中去除噪声的便利。

在给定的这种时间空间设置中检测峰值，并用**卞和学习规则**建立预测模型将是一个有前途的方向。如果你感兴趣，那么一定要看看我的另一项工作，从[到](https://github.com/ajayarunachalam/pynmsnn)用脉冲神经网络建立预测模型。

这里是本帖中讨论的 [**用例-1**](https://colab.research.google.com/drive/1hP34ZJBdO0w3zoXmWZO_upniNqFtogI_?usp=sharing) 和 [**用例-2**](https://colab.research.google.com/drive/1b8mnVNLPlNrgf8TpHf5L38Gzr-RO58-w?usp=sharing) 的完整 **Google Colab** 笔记本链接。

# 让我们连接起来

通过 [Linkedin](https://www.linkedin.com/in/ajay-arunachalam-4744581a/) 联系；或者，你也可以打*ajay.arunachalam08@gmail.com*的电话联系我(抱歉，由于我的日程安排很紧，我的回复可能会有点慢。但是，我一定会回复)

继续学习，干杯:)

**关于我**

我是一名 AWS 认证的机器学习专家兼云解决方案架构师。目前，我在瑞典 AASS 担任高级数据科学家兼研究员(人工智能)。在此之前，我在通信集团 True Corporation 担任数据科学家，处理数十亿字节的数据，在生产中构建和部署深度模型。我真的相信，在我们完全接受人工智能的力量之前，人工智能系统的不透明性是当前的需要。考虑到这一点，我一直在努力使人工智能民主化，并且更倾向于构建可解释的模型。我的兴趣是应用人工智能、机器学习、深度学习、深度强化学习和自然语言处理，特别是学习好的表达方式。根据我处理现实世界问题的经验，我完全承认，找到好的表示是设计系统的关键，这个系统能够解决有趣的、具有挑战性的现实世界问题，这些问题超越了人类的智力水平，并最终为我们解释我们不理解的复杂数据。为了实现这一点，我设想了学习算法，可以从未标记和标记的数据中学习特征表示，在有和/或没有人类交互的情况下被引导，并且在不同抽象层次上，以便在低级数据和高级抽象概念之间架起桥梁。

**参考**

[https://en.wikipedia.org/wiki/Deep_learning](https://en.wikipedia.org/wiki/Deep_learning)

[](https://demand-planning.com/2021/08/06/a-critical-look-at-measuring-and-calculating-forecast-bias/) [## 测量和计算预测偏差| Demand-Planning.com

### 我不能在不提及 MAPE 的情况下讨论预测偏差，但是因为我在过去写过关于这些主题的文章，所以在…

demand-planning.com](https://demand-planning.com/2021/08/06/a-critical-look-at-measuring-and-calculating-forecast-bias/) [](http://www.apicsforum.com/ebook/forecast_error_%2526_tracking) [## 预测错误与跟踪

### 监控预测反馈和测量预测性能是预测过程的一部分。监控预测…

www.apicsforum.com](http://www.apicsforum.com/ebook/forecast_error_%2526_tracking) [](https://pytorch.org/) [## PyTorch

### 推动自然语言处理和多任务学习的发展。利用 PyTorch 的灵活性高效地研究新的……

pytorch.org](https://pytorch.org/) [](https://www.tensorflow.org/) [## TensorFlow

### 一个面向所有人的端对端开源机器学习平台。探索 TensorFlow 灵活的工具生态系统……

www.tensorflow.org](https://www.tensorflow.org/) [](https://corporatefinanceinstitute.com/resources/knowledge/modeling/forecasting-methods/) [## 预测方法

### 财务分析师有四种主要类型的预测方法财务分析师工作描述用于…

corporatefinanceinstitute.com](https://corporatefinanceinstitute.com/resources/knowledge/modeling/forecasting-methods/) [](https://towardsdatascience.com/building-rnn-lstm-and-gru-for-time-series-using-pytorch-a46e5b094e7b) [## 使用 PyTorch 为时间序列构建 RNN、LSTM 和 GRU

### 用新的工具包重新审视长达十年的问题

towardsdatascience.com](https://towardsdatascience.com/building-rnn-lstm-and-gru-for-time-series-using-pytorch-a46e5b094e7b)