# 深度学习为什么需要 GPU？—白痴开发者

> 原文：<https://medium.com/analytics-vidhya/why-do-we-need-gpu-for-deep-learning-idiot-developer-483117bdfbcc?source=collection_archive---------15----------------------->

当前时代开始走向人工智能，人工智能凭借其实现人类梦想的任务的能力，对世界产生了巨大影响。所有这些成就主要得益于深度学习和神经网络领域的研发，这是人工智能的一部分。

# GPU 是什么？

图形处理单元(GPU)是专为并行处理而设计的单芯片处理器，可用于加速各种各样的任务，如视频渲染、游戏和机器学习。

GPU 是为处理专门计算而设计的，而 CPU(中央处理器)是为一般计算而设计的。

# 一些图形处理器:

*   [GTX 1050](https://amzn.to/3pk6PvV)
*   [GTX 1060](https://amzn.to/2MVwM89)
*   [RTX 2070](https://amzn.to/3dfP8v8)
*   [RTX 3090](https://amzn.to/3dbjb78)

# 为什么深度学习需要更多的硬件？

深度学习使用人工神经网络来完成各种任务，如[图像识别](https://idiotdeveloper.com/dog-breed-classification-using-transfer-learning-in-tensorflow/)，对象检测，[图像分割](https://idiotdeveloper.com/polyp-segmentation-using-unet-in-tensorflow-2/)，等等。在训练时，这些深度神经网络需要大量的计算能力，尤其是 GPU。

**数据集:**深度神经网络需要海量的数据集来调整数十亿个参数，以获得良好的性能。训练数据集的大小与训练时间成正比。通过利用 GPU 的并行处理，这种训练时间可以从数周减少到数天，从数天减少到数小时。

**内存:**GPU 包含名为 VRAM(视频随机存取存储器)的内存。在训练神经网络时，我们以批处理的形式提供数据集。这些批次的大小取决于 GPU 中 VRAM 的数量，因此高 VRAM GPU 可以支持高批次大小，从而减少训练所需的时间。较高的批量也有助于神经网络在给定的数据集上进行良好的归纳。

**并行性:**深度神经网络以统一的方式构建，每一层都由数千个执行相同操作的相同人工神经元组成。因此，深度神经网络的结构可以很好地处理 GPU 可以有效执行的各种计算。

**时间:**一个小数据集的较小的神经网络可以很容易地在一个 CPU 上进行训练。而具有数十亿参数和庞大数据集的深度神经网络需要几周或几个月才能在 CPU 上训练完成。通过在 GPU 而不是 CPU 上训练神经网络，可以很容易地减少这一时间。

# 结束的

在数据量非常小的情况下，可以使用 CPU 来训练模型。GPU 是长时间使用中型或大型数据集训练模型的绝佳选择。CPU 可以训练一个深度学习模型相当慢，而 GPU 可以加速训练过程。

因此，GPU 是高效和有效地训练深度学习模型的更好选择。

*原载于 2021 年 2 月 14 日*[*【https://idiotdeveloper.com】*](https://idiotdeveloper.com/why-do-we-need-gpu-for-deep-learning/)*。*