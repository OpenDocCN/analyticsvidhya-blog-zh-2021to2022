# 分析“我们真的取得了很大进步吗？对最近神经推荐方法的令人担忧的分析”

> 原文：<https://medium.com/analytics-vidhya/analysing-are-we-really-making-much-progress-c1694f60ed8f?source=collection_archive---------17----------------------->

![](img/fb76f168763c639145563e43080c9655.png)

[https://www.slideshare.net/AlanInWV/making-progress-2505638](https://www.slideshare.net/AlanInWV/making-progress-2505638)

**论文:** *我们真的取得了很大进展吗？近期神经推荐方法的一个令人担忧的分析* **作者:** [*毛里齐奥·法拉利·达克雷马*](https://arxiv.org/search/cs?searchtype=author&query=Dacrema%2C+M+F) *，* [*保罗·克雷莫内西*](https://arxiv.org/search/cs?searchtype=author&query=Cremonesi%2C+P) *，* [*迪特马尔·詹纳奇*](https://arxiv.org/search/cs?searchtype=author&query=Jannach%2C+D) **代码:**[*https://github . com/MaurizioFD/recsys 20UTM _ source = catalyst ex . com*](https://github.com/MaurizioFD/RecSys2019_DeepLearning_Evaluation?utm_source=catalyzex.com)

人工智能研究主要是为了创造超越以前模型的模型和算法，以达到“最先进”的状态。许多研究人员将他们的工作建立在以前的研究基础上，并试图使用不同的方法来改善结果。这导致了近年来深度学习的巨大进步；然而，这篇论文强调了把新的研究建立在以前的“有缺陷的研究”基础上的相关问题。在这种情况下，有缺陷的研究是指难以复制的研究工作，或者根据作者/研究人员选择的薄弱基线进行评估的研究工作。

本文指出了一个与不同领域的研究同义的问题，那就是可复制性危机。它显示了研究的系统性问题，即有发表论文的动机，但没有数据、代码或足够的信息来帮助其他人成功复制实验。这意味着许多研究最终只是发表论文，因为它们不能扩展到行业中去解决现实生活中的问题。在本次实验中选择用于评估的 18 个顶级推荐算法中，只有 46%是可重复的，因为研究的人工制品(数据、代码、模型等)不可用或文档不完整。作者无法就所遇到的问题做出澄清。此外，对于许多已发表的研究工作，作者倾向于选择复杂的神经算法基线，而忽略经典的机器学习算法，如 KNN( K 近邻)、基于图的模型等

当将报告的结果与其他基线进行比较时，作者使用论文中报告的相同条件和超参数进行了复制。在相同的条件下，只有一个模型“多重 VAE”与不太复杂的机器学习算法的结果相匹配。这是因为研究人员专注于在特定的实验条件下超越一些选定的基线。本文的结果也证明了一些研究者使用了不恰当的方法来得出他们的结果；例如，MCRec(基于推荐上下文的元路径)和 NCF(神经协同过滤)论文在调整超参数时使用了测试集。

其中一些问题可以通过更好的研究实践来减少，如尽可能使用公共存储库来存储代码，使用虚拟化来管理包和依赖项，提供足够的文档来证明评估方法，以及添加简单的算法基线。

# 参考

## 我们真的取得了很大进展吗？最近神经推荐方法的令人担忧的分析-[https://arxiv.org/abs/1907.06902](https://arxiv.org/abs/1907.06902)