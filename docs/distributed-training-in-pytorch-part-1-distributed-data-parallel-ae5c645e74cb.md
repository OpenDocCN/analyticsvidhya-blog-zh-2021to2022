# PyTorch 中的分布式培训(分布式数据并行)

> 原文：<https://medium.com/analytics-vidhya/distributed-training-in-pytorch-part-1-distributed-data-parallel-ae5c645e74cb?source=collection_archive---------1----------------------->

今天，我们将讨论 PyTorch 中的分布式数据并行，它可用于跨 GPU 分布数据，以训练具有多个 GPU 的模型。

![](img/fda68312e82c9a30ebf60aa866855e89.png)

泰勒·维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

就像我们在程序中使用线程以并行方式执行任务，以尽可能最快的时间完成任务一样，我们可以使用类似的技术在 PyTorch 中跨 GPU 并行训练深度学习模型。我们所需要的是一个有多个 GPU 的系统，或者每个系统中有多个 GPU 的多个系统。让我们看看如何在训练模型时实现并行性。

为了开始这样做，我们需要让自己习惯于一些我们可能会在前面的文章中用到的关键词。我们将按照给定的顺序讨论这些要点。

1.  PyTorch 中的数据并行
2.  全局解释器锁(GIL)
3.  PyTorch 中的分布式数据并行
4.  关键词
5.  履行

# PyTorch 中的数据并行

如果我们想快速开始分布式训练，我们可以在 PyTorch 中使用数据并行，py torch 使用线程来实现并行训练。

我们所需要做的就是在你的脚本中添加一行代码，PyTorch 会为我们处理并行性。我们基本上将在模型上添加一个包装器，让 PyTorch 知道它需要并行化。

```
model = torch.nn.DataParallel(model)
```

由于数据并行使用线程来实现并行性，它遇到了一个众所周知的主要问题，该问题是由 Python 中的全局解释器锁(GIL)引起的。按照 Python 解释器的设计方式，使用线程不可能在 Python 中实现完美的并行性。让我们看看 GIL 是什么。

# 全局解释器锁(GIL)

正如我前面提到的，Python 解释器的实现方式，使用线程实现完美的并行是非常困难的。这是由于一种叫做全局解释器锁的东西。

## GIL

> Python 全局解释器锁或 GIL，简而言之，是一个互斥体(或锁)，只允许一个线程控制 Python 解释器。在任何时间点，只有一个线程可以处于执行状态。

## 互斥（体）…

> 互斥体是一个互斥对象，用于同步对资源的访问。它是在程序开始时用唯一的名称创建的。互斥体是一种锁定机制，确保一次只有一个线程可以获得互斥体并进入临界区。

这基本上违背了使用线程的初衷。这就是为什么我们在 PyTorch 中有一些可以用来实现完美并行的东西。

# PyTorch 中的分布式数据并行

PyTorch 中的 DDP 做了同样的事情，但方式更加熟练，在实现完美并行的同时也给了我们更好的控制。DDP 使用多处理而不是线程，并通过模型作为每个 GPU 的不同进程来执行传播。DDP 跨多个 GPU 复制模型，每个 GPU 由一个进程控制。这里的进程可以称为在您的系统上运行的脚本。通常我们会生成进程，这样每个 GPU 都有一个单独的进程。

这里的每个流程都执行相同的任务，但使用不同的数据批次。每个过程与其他过程通信以共享梯度，该梯度需要在优化步骤期间被全部减少。在优化步骤结束时，每个过程都有平均梯度，确保模型权重保持同步。

# 关键词

> **【节点】**是你分布式架构中的一个系统。用外行人的话来说，拥有多个 GPU 的单个系统可以称为一个节点。
> 
> **“全局等级”**是我们架构中每个节点的唯一标识号。
> 
> **“本地等级”**是每个节点中流程的唯一标识号。
> 
> **“world”**是上述所有内容的联合，它可以有多个节点，每个节点产生多个流程。(理想情况下，每个 GPU 一个)
> 
> **“世界尺寸”**等于`number of nodes * number of gpus`

# 履行

让我们实现一个简单的示例，并浏览在跨 GPU 的分布式架构中训练模型所需的重要更改！我们将实现一个简单的分类器，该分类器使用非常著名的 MNIST 数据集对手写数字进行分类。我不会详细讲述编写神经网络的所有细节和其他基本内容，因为这篇文章假设您对在 PyTorch 中训练一个简单模型有一个基本的概念。事不宜迟，我们开始吧！

首先，导入必要的库。

让我们编写一个简单的 convnet，它可以将给定的输入分成十类。

现在让我们实现`main`函数，它将接受定义节点 id、GPU 数量、等级和其他内容的命令行参数。我们需要这些参数来确保每个进程可以与主节点通信，以减少梯度并获得平均梯度。每个进程都需要知道使用哪个 GPU，以及它在所有正在运行的进程中的排名。

我们将传递这样的参数——带有 ***'-n'*** 的节点 id，带有*'****-g '***的 GPU 数量，带有 ***'-nr'*** 的全局秩，以及带有 ***' — epochs '的历元数。***

`world_size`是通过将*的 GPU 数量和节点数量相乘计算出来的。*`MASTER_ADDRESS`和`MASTER_PORT`被设置为环境变量，可以被节点上的所有进程直接访问。

最后，我们生成了`train`函数，它执行脚本中最重要的部分，即训练模型。`nprocs`定义了要派生的进程数，等于`args.gpus`。默认情况下，生成 train 函数时，它会获得一个参数，用于从所有生成的进程中标识单个进程。这个数字是一个从`0 to args.gpus — 1`开始的整数。该编号可用于识别 GPU 编号以及同一范围内的设备编号。

最后，让我们实现`train`函数，看看我们如何广泛地使用这些参数。

最初，我们使用`args.nr * args.gpus + gpu`计算工作者的等级。如果这是第二个节点上具有 2 个 GPU 的第一个工作者，这里的数字将是:`args.nr = 1`因为它是第二个节点，`args.gpus = 2`因为在该节点上有 2 个 GPU，并且`gpu = 0`因为这是第一个工作者在第二个节点上使用第一个 GPU。因此，这里计算的等级将等于 2，这就是我们在所有进程中的**局部等级**。

现在我们用[**nccl**](https://developer.nvidia.com/nccl)**初始化进程组，这是 NVIDIA 集体通信库，实现了针对 NVIDIA GPUs 和网络优化的多 GPU 和多节点通信原语。我们还传入了`init_method = 'env://'`，它基本上是说，像`MASTER_ADDRESS`和`MASTER_PORT`这样的信息应该从环境变量中获取。我们指定我们计算的`world_size`和`local_rank`。**

**在第 21 行，我们用 PyTorch 的`DistributedDataParallel`类包装我们的模型，它负责模型克隆和并行训练。**

**在第 31 行，我们初始化了一个采样器，它可以在不重复任何批处理的情况下，处理不同 GPU 上的批处理的分布式采样。这是使用`DistributedSampler`完成的。我们还在下一行中向数据加载器传递这个采样器，数据加载器可以在批处理数据时使用这个采样器。**

**这就是我们如何在多个 GPU 上用分布式数据训练我们的模型。你可以在这个[链接](https://gist.github.com/Praneet9/185fd5c50ea337e2ca5a3e3bf29334cc)上获得完整的代码！如果我需要阐述任何观点或需要任何更正，请评论下来。**

**你也可以参考 neptune.ai [这里](https://neptune.ai/blog/distributed-training-frameworks-and-tools)的一篇关于分布式培训可用的各种框架和工具的博客。**

# ****参考文献****

 **[## 分布式数据并行- PyTorch 1.8.1 文档

### class torch . nn . parallel . distributed data parallel(module，device_ids=None，output_device=None，dim=0…

pytorch.org](https://pytorch.org/docs/master/generated/torch.nn.parallel.DistributedDataParallel.html)** **[](https://yangkky.github.io/2019/07/08/distributed-pytorch-tutorial.html) [## Pytorch 中的分布式数据并行训练

### 2019 年 10 月 18 日编辑:我们需要在每个过程中设置随机种子，以便用相同的…

yangkky.github.io](https://yangkky.github.io/2019/07/08/distributed-pytorch-tutorial.html)  [## GlobalInterpreterLock-Python Wiki

### 在 CPython 中，全局解释器锁或 GIL 是一个互斥体，它保护对 Python 对象的访问，防止多个…

wiki.python.org](https://wiki.python.org/moin/GlobalInterpreterLock) [](https://www.tutorialspoint.com/mutex-vs-semaphore) [## 互斥与信号量

### 互斥和信号量都提供同步服务，但它们并不相同。关于互斥体和…

www.tutorialspoint.com](https://www.tutorialspoint.com/mutex-vs-semaphore) [](https://neptune.ai/blog/distributed-training-frameworks-and-tools) [## 分布式培训:框架和工具- neptune.ai

### 深度学习的最新发展已经带来了一些令人着迷的最新成果，特别是在以下领域……

海王星. ai](https://neptune.ai/blog/distributed-training-frameworks-and-tools)**