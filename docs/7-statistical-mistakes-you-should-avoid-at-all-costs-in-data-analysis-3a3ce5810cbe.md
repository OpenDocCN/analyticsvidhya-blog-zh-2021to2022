# 在数据分析中你应该不惜一切代价避免的 7 个统计错误

> 原文：<https://medium.com/analytics-vidhya/7-statistical-mistakes-you-should-avoid-at-all-costs-in-data-analysis-3a3ce5810cbe?source=collection_archive---------9----------------------->

![](img/fdb132b9e8e96ce1769dcfc8f5b70c89.png)

[来源](https://unsplash.com/photos/7RI1YkIbCDI)

统计学是发现数据模式的优秀工具。然而，一些误解可能会在您的数据分析项目中导致头痛。正如理查德·费曼所说:

> “第一条原则是，你不能欺骗自己——而你是最容易被欺骗的人。”(**理查德·费曼)**

因此，在本文中，我们将看到一些在数据分析项目中最常见的错误。

# 1.垃圾进，垃圾出

有缺陷或无意义的输入数据会产生无意义的输出或“垃圾”，这是普遍真理。为了更清楚，让我们把建立好模型的过程比作烹饪美味食物的过程。

任何烹饪书都假设你买的不是腐臭的肉和腐烂的蔬菜。很明显，即使是最好的食谱也无法拯救一顿以有缺陷的原料开始的饭菜。数据科学也是如此:大多数书籍都假设你在使用好的数据来构建模型。你应该认识到**再多花哨的分析也无法弥补有缺陷的数据**。

![](img/4435289c2c766bccc1d2a11feca08054.png)

新鲜食材相当于好数据([来源](https://unsplash.com/photos/uQs1802D0CQ))

因此，在没有验证数据质量的情况下就直接进入建立复杂深度学习模型的过程将是一个可怕的错误。记住: ***不管你有多少数据，你知道什么算法，你尝试过多少模型或超参数:如果你的数据有缺陷，你就不会得到正确的答案。***

# 2.样本偏差

统计学中最有力的发现之一是，从相当大的、有充分代表性的样本中得出的推论，可以和从总体中得出的推论一样准确。

然而，研究设计或数据收集中的错误会导致一个大问题:大量的偏差。当总体中的某些成员比其他成员更有可能被系统地从样本中选择出来时，该样本被称为有偏样本。因此，来自有偏差样本的发现不能推广到整个人群。

为了获得选择无偏样本的挑战的直觉，你可以想象用一勺汤取样。如果你的汤没有搅拌好，一勺汤并不能告诉你整锅汤的味道。

![](img/7ee9db0312d30a6fbbc501d79069c207.png)

要用一把勺子品尝一口锅，你必须搅拌你的汤

如何避免样本偏倚？了解如何在数据科学项目中应用采样方法。有时候，从更大的人口中收集代表性样本的“最简单”的方法是随机选择该人口的某个子集*。*尽管如此，获得一份好的样品比看起来要难。

Darrell Huff 在他的书《如何用统计数据撒谎》中在讨论 pools(一种抽样方法)时对此做了很好的解释:

> “资金池的运作最终归结为一场与偏见来源的长期斗争。[……]报告的读者必须记住的是**这场战斗永远不会胜利”**。

# 3.德克萨斯神枪手谬论

德克萨斯神枪手谬论是一个使用完全相同的一组信息进行论证和“证实”的谬论。它是以一个关于一个德克萨斯人在栅栏边开枪的故事命名的。打完所有的子弹后，他走过栅栏，在洞的周围画了靶心，让他看起来像一个伟大的射手。

![](img/bc78e6c6f6b39219104e4eed5aec9602.png)

德克萨斯神枪手谬论。([来源](https://charliehankin.com/tag/bow-and-arrow/))

本质上，**因果颠倒**。人们可以相信德州人是一个伟大的射手，因为他的目标的出现，而事实上他的目标的出现是由他的投篮决定的，这没有什么特别的。

在数据分析中，当查看大量数据以识别小模式，然后根据这些模式得出结论时，这种谬误很常见。

如何避免德州神枪手谬论？**通过观察来识别数据中的模式并没有真正的问题，但是这些应该产生一个假设而不是一个结论**。然后应该用另一组数据来检验假设。延伸我们的比喻，射手要在画完目标后，回头瞄准，看能不能再打中目标。

这就是为什么我们在构建模型(也称为验证和测试集)时应该总是使用保留样本。

# 4.事后证明

你知道吗[巧克力的消费量与一个国家的诺贝尔奖数量正相关？](https://www.sciencedirect.com/science/article/pii/S2590291120300711)或者说人均消费马苏里拉奶酪与获得土木工程博士学位的数量相关？在你建议政府改变你国家的营养状况之前，你必须知道以下几点:**相关性并不意味着因果关系。**

网站[伪造的相关性](http://www.tylervigen.com/spurious-correlations)交叉统计信息，寻找数据中的巧合。这让我们只能得出一个结论:**两个现象重合的事实并不一定意味着它们彼此相关或者一个导致了另一个。**

[](http://www.tylervigen.com/spurious-correlations) [## 15 件相互关联的疯狂事情

### 为什么这些事情相互关联？这 15 个相关性会让你大吃一惊。(这个标题够煽情吗……

www.tylervigen.com](http://www.tylervigen.com/spurious-correlations) 

因此，在你将因果关系归因于一种现象之前，你必须学习[因果分析](https://en.wikipedia.org/wiki/Causal_analysis)。

# 5.报告没有上下文的数字

没有上下文的百分比是空数字！我们总是需要上下文来下结论。例如，让我们假设在三所学校进行了一项研究，结果如下:

*   学校 A 的学生成绩提高了 25%
*   B 学校的学生成绩提高了 10%
*   C 学校的学生成绩提高了 5%

根据这一数据，哪所学校的全球表现更好？

如果你的答案是“我不知道”，那么你已经走上了发展统计技能的正确道路。否则，你应该对你读到的东西更加怀疑。记住:**没有上下文，这个信息没有任何意义。**

因此，让我们来看看这些数据的背景:

![](img/ee89e2c064c05aa7a3943349dffc2c97.png)

所有学校都比 100 分增加了 5 分。即使学校 A 的增长率最高，它的表现仍然是最差的。另一方面，增加了 5%的 C 学校现在的成绩是 100 分。

从这个例子中，我们可以理解，在没有上下文的情况下报告一个数字(或特别是一个百分比)可能会导致**错误的**结论或解释。

可惜这种举报在 [**广告**](https://www.statisticshowto.com/misleading-statistics-examples/) **中很常见。尽管如此，这些迟早都会暴露出来，你不想损害你的声誉或你的品牌。所以，请避免。**

# 6.误导的图像

当想法伴随着图形时，很难不被任何东西说服。当第一个饼图出现时，我们进入自动驾驶状态，我们的大脑断开连接，只剩下我们点头的能力。

图像对于理解数据很重要，一个好的图表可以帮助我们综合各种数字信息。然而，[玩视觉化可能会很棘手](https://peltiertech.com/pie-chart-for-pi-day/)并可能导致错误的结论。这不仅可能因失误而发生，更糟糕的是，可能因恶意而发生。

然后，你绝不能相信(或设计)一个有错误的轴和比例或者没有轴和比例的图。为什么？请看下面的例子:

![](img/2cb3dfe742d2038213fffb64507833b1.png)

[来源](https://www.datasciencejunction.com/2019/01/how-to-be-cautious-about-misleading.html)

你可以在这里 找到更详细的列表 [**。**](https://www.statisticshowto.com/probability-and-statistics/descriptive-statistics/misleading-graphs/)

# 7.回归谬误

回归谬误未能解释自然波动和根本不存在的属性原因。许多事件都有波动，在极端事件**之后，会回归到均值**，但人们可能会忘记这一点，成为回归谬误的受害者。

下面是一个例子:

安装了测速摄像头后，道路上的事故频率下降了。因此，测速摄像头提高了道路安全性"

![](img/3051c8047ab304f99f176f63c12f3a0d.png)

测速相机能减少事故数量吗？([来源](https://unsplash.com/photos/PulU3AxfJtQ))

解释是:

“速度摄像头通常安装在道路发生异常高数量的事故之后，并且该值通常在事故发生后立即下降(回归平均值)。许多测速相机的支持者将事故的下降归因于测速相机，而没有观察整体趋势。

这个列表只是我们在解释数据时被统计愚弄的所有方式中的一小部分。您可以在以下书籍中找到更多示例:

[](https://www.amazon.com/How-Lie-Statistics-Darrell-Huff/dp/0393310728/ref=sr_1_1?crid=3260J220Z32EH&dchild=1&keywords=how+to+lie+with+statistics&qid=1613504459&sprefix=how+to+lie%2Caps%2C283&sr=8-1) [## 如何用统计数据撒谎

### 亚马逊网站:如何用统计数据撒谎(9780393310726):哈夫、达雷尔、盖修、欧文:书籍

www.amazon.com](https://www.amazon.com/How-Lie-Statistics-Darrell-Huff/dp/0393310728/ref=sr_1_1?crid=3260J220Z32EH&dchild=1&keywords=how+to+lie+with+statistics&qid=1613504459&sprefix=how+to+lie%2Caps%2C283&sr=8-1) [](https://www.amazon.com/Naked-Statistics-Stripping-Dread-Data/dp/039334777X/ref=pd_sbs_2?pd_rd_w=nja0d&pf_rd_p=3ec6a47e-bf65-49f8-80f7-0d7c7c7ce2ca&pf_rd_r=DC0RN6RFPVJZN9TR5FZ8&pd_rd_r=3f227d43-ce78-4779-b44a-6b604620e23e&pd_rd_wg=PT9aP&pd_rd_i=039334777X&psc=1) [## 裸统计

### 亚马逊网站:裸体统计(0889290310644):查尔斯·惠兰，乔纳森·戴维斯:书籍

www.amazon.com](https://www.amazon.com/Naked-Statistics-Stripping-Dread-Data/dp/039334777X/ref=pd_sbs_2?pd_rd_w=nja0d&pf_rd_p=3ec6a47e-bf65-49f8-80f7-0d7c7c7ce2ca&pf_rd_r=DC0RN6RFPVJZN9TR5FZ8&pd_rd_r=3f227d43-ce78-4779-b44a-6b604620e23e&pd_rd_wg=PT9aP&pd_rd_i=039334777X&psc=1) [](https://www.amazon.com/Art-Statistics-How-Learn-Data/dp/1541618513/ref=pd_sbs_2?pd_rd_w=kfH7a&pf_rd_p=3ec6a47e-bf65-49f8-80f7-0d7c7c7ce2ca&pf_rd_r=3BEH89ZV0ZMV5QDQJ87N&pd_rd_r=153e17e6-f005-4747-be8a-9a33c9baec88&pd_rd_wg=Uvj7x&pd_rd_i=1541618513&psc=1) [## 统计学的艺术:如何从数据中学习

### 统计学的艺术:如何从 Amazon.com 的数据中学习。*符合条件的免费*运输…

www.amazon.com](https://www.amazon.com/Art-Statistics-How-Learn-Data/dp/1541618513/ref=pd_sbs_2?pd_rd_w=kfH7a&pf_rd_p=3ec6a47e-bf65-49f8-80f7-0d7c7c7ce2ca&pf_rd_r=3BEH89ZV0ZMV5QDQJ87N&pd_rd_r=153e17e6-f005-4747-be8a-9a33c9baec88&pd_rd_wg=Uvj7x&pd_rd_i=1541618513&psc=1) 

最后，记住这一点:

> 用统计数据撒谎很容易，但没有统计数据就很难说出真相——
> 
> 安德烈·邓杰斯