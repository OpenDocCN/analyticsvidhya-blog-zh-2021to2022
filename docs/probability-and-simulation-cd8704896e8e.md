# 概率与模拟

> 原文：<https://medium.com/analytics-vidhya/probability-and-simulation-cd8704896e8e?source=collection_archive---------12----------------------->

## 使用 Python 计算一手扑克的概率

本文给出了一个简单的程序，演示了如何使用 Python 计算概率。该代码模拟一组 5 张牌，然后计算两手牌的概率:

*   **四张一类的牌**，一手牌包含四张一种等级的牌和一张另一种等级的牌，如 j♣j♠jj♥9♥(“四张一类，j”)。
*   **满堂彩**，一手牌包含三张一种级别的牌和两张另一种级别的牌，如 3♣3♠36♣6♥(一张“满堂彩”，三张全是六)。

## 1.生成副牌和手牌

最初，我们需要生成一副由 52 张牌组成的完整牌组，以及一个生成 5 手牌组的方法。我们可以通过创建一个将每个名字和所有排名结合起来的列表(笛卡尔积)来做到这一点。然后，我们使用 Python 函数从整副牌中随机生成一手牌，如下所示:

```
from random import sampleNAMES = ['Spade','Club', 'Heart', 'Diamond']
names = ['S','C','H','D']
ranks = ['K','Q','J', '1','2','3','4','5','6','7','8','9','10']Full_Deck = [k+c for k in names for c in ranks] 

def Generate_Hands(num_hands = 3):    
    """ generate a set of 5-card hands ['CJ','SJ','D3','DK','H9']"""
    for i in range (num_hands):
        hand = sample(Full_Deck, 5)     # generate random hand
        yield (hand)
```

“生成器”功能 ***Generate_Hands*** 从*整副牌*中随机抽取 5 张牌，并将其交还给调用者。

## 2.电脑扑克手

现在我们知道了如何生成一手牌，让我们编写一个简单的 for 循环来计算并分组相似的排名，如下所示:

```
# Using a defaultdict to assign 0 if card entry does not exists 
# when we try to access it via an index []
groups  = defaultdict(int)
for card in hand:    
    groups[card[1:]] += 1
```

因此，如果给我们以下三手牌，上述 for 循环将产生以下 3 组牌:

```
hand = [‘CJ’,’SJ’,’D3',‘DK’,‘H9’]
groups -> {'J':2, '3':1, 'K':1, '9':1} hand = [‘CJ’,’SJ’,’DJ’, ‘DJ’, ‘HK’]
groups -> {'J':4, 'K':1}     # four of a kindhand = [‘CJ’,’SJ’,’DJ’,‘DK’,‘HK’] 
groups -> {'J':3, 'K':2}     # full house
```

## 3.检查扑克手

让我们将上面的 for 循环插入一个函数，该函数检查分组并计算一手牌是同类型的四点还是满堂红。

```
from collections import defaultdict def check(hand):
    groups  = defaultdict(int)
    for card in hand:    
        groups[card[1:]] += 1

    if len(groups) == 2:
        if max(groups.values()) == 4 :  # four-of-a-kind
            return (1,0)
        else:   
            return (0,1)    # max=3 and min=2 -> full house
    return (0,0)   # all other cases
```

检查手的代码非常简单。基本上，我们检查组字典是否包含 2 个项目，如果不包含，则意味着这手牌不能是四个一类或满堂红。但是，如果组包含 2 个项目，则它只能是以下两种形式:

*   **{Rank: 4，Rank: 1}** *(四个一类)*或者
*   **{等级:3，等级:2}** *(满堂彩)*

## 4.主程序

有了这些，现在我们的主程序看起来非常简单。

```
def main():
    print (Full_Deck)
    four_kind = 0
    full_house =0
    iterations = 300_000 for hand in Generate_Hands(iterations):
        four, full = check (hand)
        four_kind += four
        full_house += full

    print ("Probability four of a kind:", four_kind/iterations)  
    print ("Probability full house:", full_house/iterations)# on my computer I get the following output
Probability four of a kind: 0.000238911
Probability full house: 0.00145329
```

## 5.比较计算

为了理解这些数字，我们需要看看用组合学计算的真实概率。在 52 张牌的游戏中，我们有 2，598，960 手可能的 5 张牌。在这些可能性中，有 624 手四张同类型的牌和 3744 手满座牌。因此，真实概率如下:

```
 Hand          Possible Hands          Probability
 Four of a Kind        624           624 /2,598,960 = 0.000240096
 Full House           3744           3744/2,598,960 = 0.00144058
```

那么为什么用模拟计算出来的，和用组合学计算出来的不一样。简单地说，在我们的 Python 代码中，我们执行了 300，000 次迭代。为了更接近真实的概率，我们需要进行更多的模拟。用 300 万次迭代试一下，检查一下差别！

*要查看完整的 Python 程序，请查看 Github repo:*

[https://github.com/abbas-taher/Probability-and-SimulationT21](https://github.com/abbas-taher/Probability-and-Simulation)