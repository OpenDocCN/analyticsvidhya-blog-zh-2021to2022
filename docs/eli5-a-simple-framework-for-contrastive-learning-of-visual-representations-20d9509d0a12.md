# ELI5:视觉表征对比学习的简单框架

> 原文：<https://medium.com/analytics-vidhya/eli5-a-simple-framework-for-contrastive-learning-of-visual-representations-20d9509d0a12?source=collection_archive---------9----------------------->

这篇文章将是我将带来的一系列 ELI5s，以解释导致我在 2020 年获得一些最佳见解的论文。每篇文章将是一个

A Simple Framework for Contrastive Learning of Visual Representations or just SimCLR has arguably been one of the best papers in 2020 for me. The reason? Extreme simplicity and maximum impact. So, let’s dive deeper!

![](img/02885f166ba1f4eac47403d234221b29.png)

Working of SimCLR. (Source: Google AI)

This is it. This 10s gif (pronounced gif or gif??) is it! While being this simple, its impact has been EXCEPTIONAL in terms of the performance it could give with a small amount of labeled data.

So, here’s how it works: What happens in this process is, we are taking **只有图片**(没有标签)的狗，也是一把椅子。然后，我们通过在原始图像上应用随机增强，从每个图像生成两个不同的图像。例如，我随机裁剪出狗的图片，应用颜色抖动和一些随机的颜色扭曲来得到最左边的图像。这些图像然后通过 CNN，这是我的编码器模块，这些网络为这些图像中的每一个输出特定的表示。使用 MLP 投影头，这些大小为 2048 的表示被进一步映射到 128 维向量的潜在空间。现在神奇的是，狗的图片的两个变换使用对比损失变得相似，就像它们相互吸引，而狗和椅子的图像被制成相互排斥，因为它们是负面的一对。在这种情况下，对比损失(NT-Xent)只不过是一种美化的交叉熵损失，通过温度缩放来帮助模型学习硬负面特征(帮助区分狗与大象、椅子和宇宙飞船)。

![](img/ffb4cb13a7ffbe2c6cc727b8e2b59971.png)

交叉熵损失与对比损失

完成这个数据扩充步骤主要是因为我想将我的未标记数据与类似的数据进行比较，还因为通过这样做，我的模型学会了查看我的图像的全局和局部方面，反过来，与我以前的方法相比，它已经证明学会了更好的表示。

编码器模块是卷积神经网络，在这里接受训练，在我完成训练后，我可以将它用作我其他任务的特征提取器。与监控的架构相同，唯一不同的是要装载的重量。Ez。

既然我们已经看完了这篇论文的特点，让我们检查一下它的优点和缺点。

# **优势**

1.  工作过程一直**极其简单**。选择模型、数据、增强，执行预训练，将模型用于监督任务。
2.  这种技术**通过大部分未标记的数据**学习大部分特征。附属阶段不需要任何标签，编码器通过纯粹查看图像来学习表示。
3.  **增强**帮助编码器**理解局部和全局特征**。
4.  这种技术在 ResNet-50 上使用 1%的标记数据进一步训练时获得了高达 85.8% 的前 5 名准确度，使用 10%的标记数据进一步训练时获得了 92.6% 前 5 名准确度(准确地说是 4 倍)。
5.  作为一种半监督学习技术，这种**直接与可用于预训练的未标记数据量**成比例。

# 缺点:

1.  这里的预训练任务只涉及数据扩充。虽然是有效的，但是增强的选择和增强的程度必须由用户根据数据和他们希望模型学习的表示类型来完成。
2.  它适用于本地化任务吗？这又一次归结到增强的选择上，这有可能使模型变得更加具有局部性。
3.  极大的批量:论文建议使用 8192 的批量！嘿，好吧，谷歌谢谢你指出我们破产了。这仍然适用于较小的数据集，但对比损失已被证明适用于较大的批量。

**资源:**

既然你已经快速阅读了我的 2020 年十大杰出论文之一的概述，这里有一些资源可供你更深入地研究和测试:

1.  视觉表征对比学习的简单框架
2.  [官方 Tensorflow 实现](https://github.com/google-research/simclr)如果你是认真的承诺。
3.  [简化(但同样有效)的 Tensorflow 实现](https://github.com/sayakpaul/SimCLR-in-TensorFlow-2)如果一切都是为了随遇而安。
4.  对于那些喜欢采取强硬手段的人(包括我自己)