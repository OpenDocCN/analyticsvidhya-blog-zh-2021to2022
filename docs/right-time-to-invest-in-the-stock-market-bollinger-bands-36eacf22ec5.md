# 投资股市的正确时机:布林线

> 原文：<https://medium.com/analytics-vidhya/right-time-to-invest-in-the-stock-market-bollinger-bands-36eacf22ec5?source=collection_archive---------11----------------------->

![](img/4278e9bf95639e6acfcad2acb11d9c28.png)

我现在应该投资股票市场吗？这是一个正确的时间吗？对于我们这些投资股市的人来说，这是一个永恒的问题。当市场上涨时，这个问题会出现很多次。但是，谁能回答这个问题呢？如果你问你的理财顾问，他会大声说“投资没有正确和错误的时间，现在就开始投资吧”。伴随着这个响亮的回答，他会用听不见的声音说:“你的投资有市场风险！!"。如果你问专家这个问题，(我的意思是如果你谷歌一下😊，没有专家会指导我们投资几块钱。他们总是说“‘现在’是投资市场的最佳时机”。哦…真的吗？“现在”怎么可能永远是投资的最佳时机呢？如果这是真的，那么均衡理论认为“现在”也不是投资股市的正确时机。正如我们所知，宇宙保持平衡，如果某事物永远为真，那么为了保持平衡，该事物必须永远为假。好吧，让我们把哲学分开:p .但是，谁能回答这个问题？投资市场有对错之分吗？我们怎样才能找到时间呢？，怎样才能找到合适的投资股市的机会？我们怎样才能找到一个合适的时机出售我们的投资并记录利润呢？答案就在布林线中，如果你知道如何用 python 来编码布林线，你自己就可以用 python 来回答这些问题。请继续关注我，在本文中，我们将使用 Python 详细介绍布林线的每个方面。

美国金融分析师约翰·布林格在上世纪 80 年代提出了交易区间的想法。布林线提供了高和低的相对定义，可以用来创建严格的交易方法，模式识别，等等。布林线由三条线组成——上、中和下波段。中间的波段是移动平均线，其参数由分析师选择。高波段和低波段位于移动平均线波段的两侧。分析师决定他们需要波动率指标设定的标准差的数量。标准差的数量又决定了中间波段与上下波段之间的距离。当股价达到上限时，它被认为是超买，可能是一个很好的退出点，当股票接近下限时，它被认为是一个很好的进入点。

如果我们使用布林线分析过去的股票市场数据，那么我们会看到市场在较低和较高波段之间波动。因此，如果目前市场触及上限，那么这意味着市场被高估，崩盘迫在眉睫。这是卖出股票和记录利润的好时机。如果市场触及较低的波段，那么这意味着市场超卖，牛市即将来临。这是购买股票和增加投资的最佳时机。如果市场在上下波段的中间，那么从市场图的方向，我们可以看到它的走向。如果方向是向上的，那么这是一个正确的投资时机，如果是向下的，那么我们应该等待一段时间把钱投入股票市场。

让我们使用布林线分析当前的股票市场(BSE SENSEX ),看看它的位置。现在是投资还是不投资的好时机？下图显示了 2000 年 1 月至 2020 年 12 月的 BSE SENSEX 数据。x 轴代表时间维度；y 轴代表市场收盘价。蓝色的图表显示了从 2000 年到 2020 年的收盘价历史。橙色图形代表布林线上轨，绿色图形代表布林线下轨。我们可以清楚的看到，股市在布林线的上下区间震荡。在最近的过去；2020 年 4 月是投资该市场的最佳时机，因为当时该市场触及了布林带下轨。同样，我们可以看到，在过去市场触及较低波段时，有很多机会。而且，我们还可以看到，当市场触及上限时，它就变成了熊市。目前市场已经触及上轨，它正在人为地推高上轨。所以，布林线表示下跌趋势即将来临。因此，现在根本不是投资股市的好时机。

上下波段之间的差距代表市场波动。一般来说，当市场变得更加波动，差距扩大，在波动较小的时期差距变得更小。在目前的情况下，上下波段之间存在前所未有的巨大差距，这可能是未来灾难的迹象。

![](img/4278e9bf95639e6acfcad2acb11d9c28.png)

## 现在，让我们来看看如何详细计算布林线债券:

## **布林线由 3 个简单的计算组成:**

*   第一个或中布林线是一条 n 期均线。例如，要计算 n=20 的窗口大小的移动平均值，您需要将连续 20 天的市场收盘价相加，然后将该值除以 20。

***MiddleBand=n 周期移动平均值***

*   第二条线是布林线上轨。它是通过将 n 期标准差加到中间波段计算出来的。n 周期标准偏差乘以标准偏差测量值“y ”,然后将其添加到中间波段。

***上带* = *中带* +( *y* × *n* - *周期标准偏差* )**

*   第三条线是布林带下轨。它的计算方法是从中间带减去 n 期标准差。n 周期标准偏差乘以标准偏差测量值“y ”,然后从中间带中减去它。

***下带* = *中带*-(*y*×*n*-*周期标准偏差* )**

很简单！！不是吗？😊如果你已经理解了这些公式，那么你可以直接进入下面详细解释的 python 程序。然而，如果你像我一样在工程统计讲座中睡觉，那么请允许我在直接跳到代码之前解释一些基本的统计概念，如移动平均、标准差和 n 期标准差。(没错，写 python 程序之前我得先学会这些东西。)

## 移动平均值:

首先，我们看看什么是平均水平。集合的平均值()的计算方法是将集合中的所有元素相加，然后将总和除以集合中的元素总数。举个例子，

假设我们有一个集合 s = {5，2，3，7，1，8，9，4，6}，集合中总共有 9 个元素。

平均值= (5 + 2 + 3 + 7 + 1 + 8 + 9 + 4 + 6) / 9

平均值= 45 / 9 = 5

现在，让我们看看什么是均线。

窗口大小 n 的简单移动平均值是前 n 个数据点的未加权平均值。

MA(n) = (A1 + A2 + … + An-1) / n

是这样的；我们首先计算从索引 0 开始的 n 个数据点(元素)的平均值，然后针对第 n 个元素存储该值。然后，我们将索引增加 1，计算从该索引开始的接下来 n 个数据点的平均值，并针对第(n+1)个元素存储它，等等。

对于数据集 s = { 5，2，3，7，1，8，9，4，6 }，以下是窗口大小 n = 3 的移动平均值的逐步计算

**步骤 1:** 对于窗口大小 3，我们无法计算前 2 个元素的移动平均值，因为这些元素不存在之前的 n = 3 个数据点。因此，对第三个元素及以后的元素计算移动平均值。

第三个元素的移动平均值= (5 + 2 + 3) / 3

第三个元素的移动平均值= 3.33

![](img/7874e7a68a71979ff06cebf436a7ee78.png)

**步骤 2:** 现在，为了计算第四个元素的移动平均值，我们将第二、第三和第四个元素相加，然后将总和除以 n=3。

第四个元素的移动平均值= (2 + 3 + 7) / 3

第四个元素的移动平均值= 4

![](img/6efcc9daec3fc830b34fd7d421eeab2a.png)

**步骤(最后):**同样，我们继续计算所有元素的移动平均值。

![](img/fb8bbc43c67f2e241e10f744867b4fbf.png)

## 计算移动平均值的 Python 代码:

```
import pandas as pd 

Array = pd.DataFrame([5,2,3,7,1,8,9,4,6])
Array['Moving_avg_3'] = Array[0].rolling(window=3).mean()print(Array['Moving_avg_3'])
```

## 标准偏差:

在统计学中，标准差是对一组值的变化量或离差的度量。低标准偏差表示这些值倾向于接近集合的平均值(也称为期望值)，而高标准偏差表示这些值分布在更大的范围内。

## 标准差的公式为:

![](img/392a554ba5fc7384c8d52ca5199c6e97.png)

让我们计算数据集 s ={ 5，2，3，7，1，8，9，4，6 }的标准偏差(SD)

**第一步:**求数据集 s 的均值

平均值= (5 + 2 + 3 + 7 + 1 + 8 + 9 + 4 + 6) / 9

平均值= 45 / 9 = 5

**第二步:**求每个值与均值的差(x —均值)。对于我们的数据集，请参见下面的计算。

![](img/d66f52973feca800d00d1d4119eba983.png)

**第三步:**对第二步中得到的每个差异或每个答案进行平方(x-均值)计算。

![](img/f19745a0396dc122c27705afa863afbf.png)

**第四步:**将第三步得到的所有答案相加，然后除以元素个数——1，得到平均值。

**平方平均值** = ( 0 + 9 + 4 + 4 + 16 + 9 + 16 + 1 + 1 ) / 8

**平方平均值** = 7.5

**第五步:**取第四步求的平均值的平方根。这就是标准差。

标准偏差(SD) =平方根(7.5)

**标准偏差= 2.73**

## 计算标准差的 Python 代码:

```
import pandas as pd 

Array = pd.DataFrame([5,2,3,7,1,8,9,4,6])
SD = Array[0].std()print(SD)
```

## n 周期标准偏差:

和移动平均线一样，n 期标准差是前 n 个数据点的移动标准差。

为了计算 n 周期标准偏差，我们首先计算从索引 0 开始的 n 个数据点(元素)的标准偏差，然后存储第 n 个元素的值。然后，我们将索引增加 1，计算从该索引开始的接下来 n 个数据点的标准偏差，并针对第(n+1)个元素存储它，等等。

对于数据集 s = {5，2，3，7，1，8，9，4，6 }，以下是窗口大小 n = 3 的 n 期标准偏差的逐步计算

**步骤 1:** 对于窗口大小 n=3，我们无法计算前 2 个元素的 n 期标准偏差，因为这些元素不存在之前的 n = 3 个数据点。因此，为第三个元素及以后的元素计算 n 周期标准偏差。

元素 5、2 和 3 的标准偏差= 1.52

![](img/8e3b74b5ae4bd72f77867c0dfc0aa5d1.png)

**步骤 2:** 现在，使用元素 2、3 和 7 计算第四个元素的 n 周期标准偏差。

元素 2、3 和 7 的标准偏差= 2.64

![](img/f5e0dac6e745783a95b31386675d5d6e.png)

**步骤(最后):**同样，我们继续计算所有元素的 n 周期标准偏差。

![](img/bee07fd45010c95fa3b5ab08fc024297.png)

## 计算 n 周期标准差的 Python 代码:

```
import pandas as pd Array = pd.DataFrame([5,2,3,7,1,8,9,4,6])
Array['Standard_deviation_3'] = Array[0].rolling(window=3).std()print(Array['Standard_deviation_3'])
```

## 计算布林线的 Python 代码:

现在，我们有了所有的信息和知识，可以继续编写 python 程序来计算布林线。以下是相同的完整 python 程序。代码被足够注释来解释这个问题的每个方面。将它复制粘贴到 python IDE 上，运行它来查看布林线。在运行代码之前，您需要股票市场数据。我已经在 GitHub 上传了 BSE SENSEX 数据和代码，点击这里[https://github.com/srkhedkar/Bollinger-bands](https://github.com/srkhedkar/Bollinger-bands)

如果您对实施有任何意见/担忧，请随时拨打 srkhedkar@gmail.com[的电话联系我](http://mailto:srkhedkar@gmail.com/)

```
import pandas as pd
import matplotlib.pyplot as plt # Block 1 for BSE SENSEX
################################################################################
# read the Indian BSE data filestockprices = pd.read_csv('D:\\python\\data\\SensexHistoricalData.csv') #setting date as index
stockprices['Date'] = pd.to_datetime(stockprices.Date)
stockprices.index = stockprices['Date'] # Following parameters should be decided by data analyst.
# Refer bollinger bands rules for more details at official site [https://www.bollingerbands.com/bollinger-band-rules](https://www.bollingerbands.com/bollinger-band-rules)yearsOfData = 20
windowSizeFactor = 6
windowSize = 20 + yearsOfData * windowSizeFactor
stdDeviationFactor = 0.025
stdDeviation = 2 + (yearsOfData * stdDeviationFactor) # Calculate moving average
stockprices['Moving_avg_n'] = stockprices['Close'].rolling(window=windowSize).mean() # Calculate moving standard deviation
stockprices['Standard_deviation_n'] = stockprices['Close'].rolling(window=windowSize).std() # Calculate the upper band
stockprices['Upper_band'] = stockprices['Moving_avg_n'] + (stockprices['Standard_deviation_n'] * stdDeviation) # Calculate the lower band
stockprices['Lower_band'] = stockprices['Moving_avg_n'] - (stockprices['Standard_deviation_n'] * stdDeviation) # Plot the graph
stockprices[['Close','Upper_band','Lower_band']].plot(figsize=(10,4))
plt.grid(True)
plt.title(' Bollinger Bands ')
plt.axis('tight')
plt.ylabel('Price')
plt.legend(["Market Close", "Upper Band", "Lower Band"])
plt.show()
```

## 参考资料:

【https://www.bollingerbands.com/ 

[https://en.wikipedia.org/wiki/Standard_deviation](https://en.wikipedia.org/wiki/Standard_deviation)

[https://www . khanacademy . org/math/statistics-probability/summaring-quantitative-data/variance-standard-deviation-population/a/calculating-standard-deviation-step-step](https://www.khanacademy.org/math/statistics-probability/summarizing-quantitative-data/variance-standard-deviation-population/a/calculating-standard-deviation-step-by-step)