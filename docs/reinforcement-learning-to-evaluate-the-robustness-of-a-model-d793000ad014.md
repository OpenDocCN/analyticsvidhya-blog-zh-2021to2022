# 评估模型稳健性的强化学习

> 原文：<https://medium.com/analytics-vidhya/reinforcement-learning-to-evaluate-the-robustness-of-a-model-d793000ad014?source=collection_archive---------18----------------------->

对于机器学习模型来说，在众多任务中炫耀其令人敬畏的类似人类(或更好)的表现，目前的时代从未如此合适。在诸如语言建模、计算机视觉、图形建模等任务中已经实现了有希望的结果。

## 维度问题

然而，正如他们所说的，对任何事情都有一点怀疑是健康的，并且总是被鼓励的。因此，信任一个预测变得具有挑战性，特别是在医学领域、人口统计预测等，这些领域本质上是敏感的领域，并且在预测中不能有任何不一致的空间。特别是，如果模型最终只对某些类型的输入数据做出错误的预测，那么缓解这种情况会变得非常繁琐。

当我们考虑数据在非常高维的空间中时，不一致预测的问题可能是明显的。这有时被专家称为[“维度诅咒”](https://en.wikipedia.org/wiki/Curse_of_dimensionality)。本质上，它谈论了在这样的高维空间中，数据点之间的关系变化是多么奇怪。我所说的高维空间是指具有指数级大量特征的数据点的表示。在这样的高维空间中，距离的概念可能与低维空间中的概念不同。这反过来可能会导致回归、分类或任何其他预测任务的性能下降。这种现象被称为[休斯现象](https://www.researchgate.net/publication/283485444_Consequences_of_the_Hughes_phenomenon_on_some_classification_Techniques)。

## k 最近邻搜索

![](img/a2a828f507072a63659ad14d67e707c4.png)

来源:谷歌

KNNS 是一种简单而优雅的算法，用于查找数据点的邻居。这些年来，算法已经有了很大的发展，人们只能想象算法中可能出现的变化。本质上，对于任何给定的点，KNNS 搜索它的前 k 个最近的邻居来决定它的性质。这可用于分类、回归等应用，以及制造业等领域，以分析各种来源的数据、基因测序等。

当数据点的维数呈指数增长时，KNNS 的问题变得更加困难。例如:使用蛮力方法在 10000 个点(O(n))的空间中计算二维点的 5 个最近邻可能更简单。但是当同样的方法扩展到具有 1，000，000×1000 维点的空间时，在计算上要昂贵得多。因此，聪明人想出了处理这种高维空间的有趣和创造性的方法。一些方法涉及使用有效的数据结构来近似给定数据点的邻居，如 [KD-Tree](https://en.wikipedia.org/wiki/K-d_tree) 、 [Ball-Tree](https://en.wikipedia.org/wiki/Ball_tree) ，或数据修剪——对空间进行聚类，然后进行搜索、[用 Kedges 构建最近邻居图](https://en.wikipedia.org/wiki/Nearest_neighbor_graph)等。当这些算法不搜索整个空间来计算最近的邻居，而是搜索最可能的候选者的子集时，近似的概念就出现了。

## 问题

这种近似 KNNS 方法面临的一个问题是，它们对于大部分数据点是有效的/正确的，但是对于一些数据点却不能产生真正的邻居。

![](img/3b752558feac2e3f93e285c777d3fe33.png)

考虑二维空间的上图。假设基于点 ie 的颜色找到最近的邻居。将在红点区域中搜索红点的前 k 个邻居。但是当我们考虑黑点时，它们位于与 2 个不同颜色组的空间距离相等的边上。因此，如果我们想要搜索圈出的点的最近邻，结果集将包括红色和绿色组中的点。

现在，我们可以直观地想到上述 2D 空间的场景。但是当我们取 100 到 1000 维空间时，可能很难确定“真正的”邻居。因为我们是三维的存在，我们可能无法想象 100 和 1000 维空间的边界。这反过来可能导致无法找到真正邻居的问题，因此可能导致假邻居。

近似的 KNNS 方法在创建这种假邻居方面进行了折衷，并且实现了高检索速度。然而，对于关键应用程序，几乎没有假邻居是有意义的。因此，重要的是要评估模型，以认识到它们有多健壮，即模型在多大程度上可以抵制自己产生假邻居！稳健性检查又可用于决定哪个模型更可靠。

## 提议的方法

我们利用[强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning)来评估模型的健壮性。特别是[优势演员兼评论家架构](https://theaisummer.com/Actor_critics/)。

简而言之，强化学习(RL)是一种对问题的解决方案进行建模的方法，它通过不断地回顾当我们的智能体采取行动时环境是如何变化的。代理人玩赛车游戏到达终点是理解强化学习的最好例子之一。

演员-评论家架构是 RL 的一种风格，其中**演员**学习在给定的当前状态下选择最佳可能的行动，而**评论家**批评演员的行动。它在某种程度上结合了两种 RL 的优点([基于价值的](https://towardsdatascience.com/value-based-methods-in-deep-reinforcement-learning-d40ca1086e1)和[基于策略的](https://towardsdatascience.com/policy-based-reinforcement-learning-the-easy-way-8de9a3356083))。**Advantage Actor-critical**是 Actor-critical 框架的一个变体，它考虑一个行为与我们当前所处的状态相比有多好。这也有助于减少训练期间网络中的高方差。

![](img/682caadddc769f6eb40e98fb11bc4f2e.png)

资料来源:https://arxiv.org/pdf/2102.06525.pdf

上图显示了如何将 KNNS 鲁棒性评估建模为 RL 问题的设计。

对于我们的用例，代理被训练生成使 KNNS 模型失控的例子。当我们谈论高维空间时，我们假设对于每个给定的查询点，代理学习一个多维空间(为了简单起见，我们假设它是一个[多变量正态分布](https://en.wikipedia.org/wiki/Multivariate_normal_distribution)),然后我们可以使用它来采样点，这将使 KNNS 模型产生给定查询点的假邻居。

如图所示，我们将一个测试点和一个初始空间(均值和协方差作为多元正态分布的参数)传递给**参与者**，在我们的例子中，它是一个神经网络。行动者导航这个空间并重新配置它(通过偏移平均值和协方差)以相对于给定的查询点调整它。然后，可以针对抖动点(对立点)对这一重新配置的空间进行采样，抖动点由图中输入点周围的虚线圆圈表示，然后将其传递给目标 KNNS 模型，以评估模型的鲁棒性。这个质量由**评论家**预测为一个分数，它也是一个神经网络。稳健性的程度是通过计算被证明是敌对的样本的比例来衡量的。我的意思是，有多少抖动点会使 KNNS 模型产生虚假邻居。

![](img/0cfc6133fbe2d6e42a3cb53b3a0749bc.png)

来源:https://arxiv.org/pdf/2102.06525.pdf

上面的等式被用作奖励函数来加强我们的代理，其中 *FP* —假阳性， *Sample_Size* —来自预测空间的采样抖动的大小(在我们的例子中是 1000)。因此，当我们的代理使 KNNS 模型更加混乱时，我们会对其进行奖励。

**结果**

我们使用[手套](https://nlp.stanford.edu/projects/glove/)嵌入针对两种 KNNS 方法——[ball tree](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.BallTree.html)和 [FLANN](https://github.com/mariusmuja/flann) 测试了我们的方法。

简而言之，BallTree 是最近邻算法，它利用树结构和超球面来加速搜索。FLANN 是一种近似最近邻搜索算法，它利用[随机化 KD 树](https://www.vlfeat.org/overview/kdtree.html)和优先级搜索 KMeans 树来优化和加速最近邻搜索。

![](img/c1b537ae931b99a52dbd5336ee5c3e0f.png)

我们针对几个点训练了上述 KNNS 模型，并针对几个测试点测试了我们基于 RL 的鲁棒性评估框架。

涉及的步骤:我们将每个测试点传递给预测重新配置的空间的参与者—从该空间中采样 1000 个点—将样本传递给两个目标算法(BallTree 和 FLANN) —将 top-k 邻居与 1000 个点的真实邻居进行比较。

如前所述，为了简化起见，我们假设采样点来自多元正态分布，采样点的真正邻居通过搜索整个空间(以欧几里德距离作为距离度量)由 O(n)蛮力方法确定。

从图中可以看出，我们推断 BallTree 可能比 FLANN 相对更健壮，因为它试图抵抗 top-K 邻居值比 FLANN 相对更高的攻击。这可以从 BallTree 与 FLANN 相比缓慢增长的曲线看出。

**想法和结论**

本质上，我们试图理解为什么在 KNNS 算法中，某些点与其他点相比会产生错误的邻居。当我们处理高维空间时，我们依赖于一个基于 RL 的代理来试图找到导致 KNNS 模型失败的点。

作为未来的工作，也许可以通过将(对立点的)分布本身作为一个参数并让 RL 代理决定对立点的分布类型来进行更多的概括。更复杂的架构可能会带来更好的结果。共享的 Colab 代码被一般化以将任何目标算法作为超参数，因此系统可以针对其他算法进行测试。这可能是探索在非常高的维度上运行的系统的可解释性方面的一个步骤。

来源:

https://arxiv.org/pdf/2102.06525.pdf

代码:
[https://colab . research . Google . com/drive/1 tmp 0x 8 jaol TMS 7 hqabbbfdyyqxurcsp 1？usp =分享](https://colab.research.google.com/drive/1tMp0x8jaoLtms7hqAbbBFdYyqXuRcSp1?usp=sharing)