# 使用蒙特卡罗方法更好地估计结果

> 原文：<https://medium.com/analytics-vidhya/using-the-monte-carlo-method-to-better-estimate-outcome-c4694b3d228f?source=collection_archive---------30----------------------->

![](img/73824b4e77d7dd99e377354b8dbb047a.png)

卡尔·伊巴莱在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你面临一个决策，不同结果的可能性带来了不可避免的巨大风险，那么你只能做这么多来计算这种不确定性。这种情况的一个例子是预测天气。当对如此多的变量进行实验太不切实际时，一种应对这种不确定性的方法被称为蒙特卡罗方法。

蒙特卡洛方法是由 20 世纪 40 年代从事曼哈顿计划(原子弹)的科学家正式命名的，当时他们的核物理研究模型过于复杂，维度太多，无法进行标准分析。想象一下，创造一种人类已知的最危险的武器……这里没有计算错误或失误的余地。他们设计了一种从概率上接近不确定性的方法，使用从样本群体中随机抽样来模拟结果。其思想是推断统计学和中心极限定理:*随机样本将展示总体*的特性。就像一系列抛硬币来确定正面或反面的概率，除了这里的一系列可能事件涉及可能杀死成千上万人的核裂变。多么令人愉快。他们决定以摩纳哥的蒙特卡洛赌场命名，因为命名科学家斯坦尼斯瓦夫·乌拉姆的一个亲戚有赌博问题。

这里需要注意的一点是，创建这样一个模型需要与当前项目相关的信息。Robert L. Harrison 发表的这篇论文定义了重要的关键点:

> -我们期望的产出是什么？
> 
> -这些输出将用于什么？
> 
> -输出必须有多准确/精确？
> 
> -我们到底可以/必须如何建模？
> 
> -我们究竟可以/必须如何定义输入？
> 
> -我们如何对底层流程建模？

对于任何可以取一系列可能值的变量，我们将在模型中模拟这种组合的所有排列，并从中随机取样。样本空间中排列的这些多次迭代应该呈现更接近真实概率的结果。这基本上就是大数定律。你运行模拟的次数越多，结果就越接近真实的发生概率。

一个非常容易理解的例子可以在轮盘赌的模拟中看到，轮盘赌是一种常见的赌场游戏，我和我的大学室友曾经天真地认为我们可以破解它(臭名昭著的赌徒谬误)。让我们从约翰·古塔格教授领导的麻省理工学院数据科学课程中为一个面向对象的轮盘赌游戏编写代码。*(伟大的代码已经写好了，为什么还要花额外的时间重新写代码？唯一的规则是你应该知道到底发生了什么。)*

下面的类定义了一个*公平*轮盘赌游戏，意思是预期收益应该是 0(%)。让我们运行游戏 1，000，000 次，并从空间中取样，以模拟蒙特卡洛方法，获得平均回报的最佳想法。

```
class roulette():
 def __init__(self):
  self.pockets = []
  for i in range(1, 37):
  self.pockets.append(i)
  self.ball = None
  self.pocketOdds = len(self.pockets) — 1
 def spin(self):
  self.ball = random.choice(self.pockets)
 def betPocket(self, pocket, amount):
  if str(pocket) == str(self.ball):
   return amount*self.pocketOdds
  else: 
   return -amount
 def __str__(self):
  return ‘Roulette’ 

def play_roulette(game, numSpins, pocket, bet):
 totPocket = 0
 for i in range(numSpins):
  game.spin()
  totPocket += game.betPocket(pocket, bet)
 print(numSpins, ‘spins of’, game)
 print(‘Expected return betting’, pocket, ‘-’,    str(100*totPocket/numSpins) + ‘%\n’)
 return (totPocket/numSpins)game = roulette()
for numSpins in (100, 1000, 10000, 100000, 1000000):
 for i in range(3):
 play_roulette(game, numSpins, 5, 1)
```

因此，在这里，我们的样本总数实际上是范围(1000000)，但我们只想知道 100 次旋转、1000 次旋转、10000 次旋转、100000 次旋转和 1000000 次旋转的 3 个回合的回报。

结果:

```
100 spins of Roulette
Expected return betting 5 = -64.0%

100 spins of Roulette
Expected return betting 5 = 44.0%

100 spins of Roulette
Expected return betting 5 = 8.0%

1000 spins of Roulette
Expected return betting 5 = 4.4%

1000 spins of Roulette
Expected return betting 5 = 11.6%

1000 spins of Roulette
Expected return betting 5 = -2.8%

10000 spins of Roulette
Expected return betting 5 = -6.4%

10000 spins of Roulette
Expected return betting 5 = 3.68%

10000 spins of Roulette
Expected return betting 5 = -5.68%

100000 spins of Roulette
Expected return betting 5 = 1.484%

100000 spins of Roulette
Expected return betting 5 = -0.712%

100000 spins of Roulette
Expected return betting 5 = -1.864%

1000000 spins of Roulette
Expected return betting 5 = 0.8684%

1000000 spins of Roulette
Expected return betting 5 = -0.2116%

1000000 spins of Roulette
Expected return betting 5 = 0.1448%
```

在 100 次旋转的 3 次迭代后，我们的平均回报率为-4.0%。

在 3 次 1000 次旋转的迭代后，我们的平均回报率为 4.399%。

在 10，000 次旋转的 3 次迭代后，我们的平均回报率为-2.800%。

在 100，000 次旋转的 3 次迭代后，我们的平均回报率为-0.364%。

我们在 1，000，000 次旋转 3 次迭代后的平均回报率为 0.408%。

如果我们不运行这么多游戏，对平均回报的感知会有所不同。如果我们运行 3 次仅 100 次旋转的迭代，可以假设回报率为-4%。如果我们运行到 10，000 次旋转，我们的平均回报率为-4，4.399 和-2.88，估计回报率为-0.827%。

我们在这里可以看到，通过大量的游戏组合进行越来越多的迭代后，我们的预期平均回报率下降到 0%，方差越来越小。然后我们可以推断，我们运行的实验越多，我们的结果的概率，在我们的例子中是% return，实际上是 0%。

这就是蒙特卡洛模拟的思想！我们的例子非常简单，但是在不知道某个结果的概率的情况下，任何组合都要迭代数千次，然后取平均值。我希望你能够掌握蒙特卡罗方法的概念，并能理解它如何有助于确定未知的风险。

如果你对这个方法有任何建议或问题，请联系我们！我一直在寻求学习和提高。

奥林