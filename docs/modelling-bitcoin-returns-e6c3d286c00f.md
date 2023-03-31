# 模拟比特币回报

> 原文：<https://medium.com/analytics-vidhya/modelling-bitcoin-returns-e6c3d286c00f?source=collection_archive---------15----------------------->

我们的密码交易者读者可能会发现这篇文章很有帮助。在这里，我们将使用一些简单的旧统计数据，并找出哪个概率分布函数最符合比特币的回报。这种功能将允许更好的风险控制，并且——我们希望——更准确地预测加密货币的价格。在本文中，我们将使用 R 作为我们选择的工具

## **简介**

2008 年，当比特币首次被提出时，一些人认为它是快速致富的机会，而另一些人则认为它是卑鄙的金融金字塔，旨在让其创造者致富。我们将在这里采取一种中间方法，即加密货币在金融投资组合中扮演类似于实物黄金的角色:防范所有法定货币系统性贬值的风险。当然，黄金和比特币并不相互替代，而是具有互补的功能:虽然黄金有着悠久的传统，即使在电子媒体(如互联网)完全中断的情况下也能保持其价值，但比特币在主要威胁是被专制或非法政府没收的情况下表现更好，因为没有存放它的存款机构或保险箱，也没有可能被没收。[1] [2]

然而，由于它是一项相对较新的资产，其行为仍然是研究的对象。例如，在股票市场中，一般来说，正态分布用于在风险计算中模拟回报(损失的概率和大小)。鉴于比特币是一种极不稳定的资产，在没有事先分析的情况下，我们不能假设其回报具有这种分布。然后，我们将通过一些概率分布家族的方法，然后通过 Kolmogorov-Smirnov tes 测试他们中的哪一个对比特币有最好的坚持(不要担心，我们将为不熟悉它的读者解释测试)。[1] [2] [3]

## 一个——没有那么多——深入研究统计数据

在这里，我们将解释一些连续概率分布的概念，列举几个并介绍 Kolmogorov-Smirnov 检验，已经熟悉这些主题的读者可以轻松地跳过这一部分。

概率分布被理解为确定实验或观察到的现象中每个不同的可能事件发生的概率的函数。当分布以可计数的方式呈现时，称为离散分布。这种分布的一个例子是二项式分布。另一方面，连续分布是发生在连续空间中的分布，不能以表格形式显示。作为连续分布的一个例子，我们可以给出成年人的身高分布。【4】

关于连续分布有一点很重要，那就是某个特定值的概率是由零给出的。以身高为例，我们知道一个人正好 164.00 厘米(右边有无限个小数位)的概率可以理解为 0。然而，一个人的身高在 163.999 厘米和 164.001 厘米之间的概率自然是非零的，并且是可以测量的。[4]

概率分布有很多种，在本文中我们将重点介绍以下几种:

**正态分布:**

A.由于其图表的格式，它是最著名的曲线之一，也是最常用的，因为它使计算最容易，也是自然界中最常见的，它由平均值和标准偏差定义。[4] [5]

正态分布的应用很多，例如高度分布、自然现象、统计过程控制(六适马方法)。其中，在现实中，正态性的应用是如此广泛，以至于在大多数情况下假设正态性而不测试这一假设是否有效是很常见的——Nassim Taleb 在他的《反脆弱性》中对这种滥用提出了广泛的警告——一般来说，在金融市场中必须谨慎对待正态性的假设，因为金融市场的行为不像自然现象。[4] [5]

假设正态性不成立的主要风险是，在正态分布下，极端事件很少或不可能发生(例如:发现一个 5 米高的人的概率为零)，然而在金融市场中，极端事件确实会发生，而且有一定的频率(对于比特币来说，这一点更加明显)。[4][5]

**学生的 t 分布:**

Student 的 t 分布，或简称为 T，与正态分布密切相关:它自然地出现在小样本的正态分布和未知标准差的总体的均值估计中。[6]

样本均值和标准差之外的 t 分布由一个称为自由度的量定义，它基本上是样本大小减一。[6]

t 分布的曲线类似于正态分布，呈钟形，但具有较长的尾部，因此它是解释金融现象的有力候选对象，众所周知，金融现象就是由 t 分布呈现的。然而，高样本量的学生 t 分布收敛于正态分布。[5] [6]

**拉普拉斯分布**

以皮埃尔·西蒙·拉普拉斯命名的拉普拉斯概率分布与伽马分布有一些相似之处，正如它由两个参数 mu(均值或位置)和 b(标度)定义一样，它也是贝叶斯统计中使用的先验分布(将来我可能会写一些关于这个主题的思考)和厚尾(允许极端事件比正态事件更多)分布，这就是我们对这项研究感兴趣的原因。【9】

拉普拉斯分布具有用于建模布朗运动(即流体运动，这在未来的资产价格建模中可能是有用的)的附加属性。[9]

**察利斯分布**

在这里的所有分布中，Tsallis 可能是最复杂的一个，因为它涉及到熵的概念，用几句话解释起来可能会很混乱(有时它被理解为“混沌的度量”)。为了我们的目的，我们将保持简单，并说它也是一个厚尾分布，这是已经在以前的作品中使用的-不是作者-股票市场期权定价。[16]

**幂律**

幂律是一组允许长尾的分布，也就是说，用小概率(但不为零)模拟极端事件。最著名的幂律分布之一是帕累托分布，80-20 法则(例如，80%的收入掌握在 20%的人口手中)就是由此而来。[10]

**科尔莫戈夫-斯米尔诺夫试验**

Kolmogorov-Smirnov 检验以俄罗斯科学家安德雷·柯尔莫哥洛夫和尼古拉·斯米尔诺夫的名字命名，是一种连续概率分布之间相等的非参数检验(即，它不要求数据具有参数化或甚至参数化的概率分布，这就是为什么它非常通用)。它可以在一个样本和一个连续分布和两个样本之间完成。该检验有一个相等的零假设，返回一个 p 值和一个称为 D 的统计值，D 值越低(或 p 值越高)表示样本对应于相同分布的概率越大。[13]

Kolmogorov-Smirnov 检验可用作两个分布之间的拟合优度检验:给定一组具有已知参数分布的数据和另一组我们对其分布一无所知的数据，我们可以使用 Kolmogorov-Smirnov 检验来比较这两个数据，并查看未知数据是否可以用已知分布来近似。[13]

在我们的研究中，我们将比较从已知概率分布中获得的随机样本，并将它们与比特币的随机样本进行比较，我们将选择具有最低 d 的随机样本作为最合适的分布[13]

## 数据和代码

在这项研究中，我们将使用一个包含比特币每分钟价格的数据库，该数据库是从 Bitstamp 加密货币经纪人那里收集的。Bitstamp 是卢森堡货币管理局许可的货币经纪人，为其客户提供与欧洲支付系统(SEPA)集成的交易环境。该数据库可以在网站上找到:[https://www . ka ggle . com/mczielinski/bit coin-historical-data【14](https://www.kaggle.com/mczielinski/bitcoin-historical-data[14)】

该数据库提供 2012 年至 2020 年每分钟以美元报价的比特币交易数据。其中包含的信息是:交易的日期和时间(unix 时间戳格式)、开盘价、最高价、最低价、收盘价、以 BTC 和美元表示的交易量，以及按交易价值加权的平均价格。

比特币交易一天 24 小时都在进行，与货币市场(外汇)类似，与大多数资产的情况相反，交易发生在特定的窗口内——例如，泛欧交易所的股票交易窗口是上午 9 点到下午 5 点。因此，收盘价对这个市场没有重大意义:在一个有固定时间的市场中，收盘价象征着参与者之间的共识，在全球货币和加密货币市场中，每个地区都有其特定的收盘价格。因此，我们将使用最低价格和最高价格之间的中间值(市场上称为“MID”)作为参考价格，并计算这两个价格之间的百分比差异作为一天的回报。[15]

首先，让我们定义我们的数据框架:

bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31

并导入我们的依赖项:

`` `{r，include = FALSE }
load(" my _ work _ space。RData")
库(anytime)
#库(dplyr)
库(plyr)
库(dgof)
库(fitdistplus)
库(VGAM)
库(data . table)
库(data.table)
库(EnvStats)
库(ggplot2)
库(tsallisqexp)
库(poweRlaw)
库(fitur)【库

现在，让我们检查我们的数据框架:

![](img/183fe4fe489a7c5b71e1776b96475dbe.png)

图 1:我们的数据集，注意有很多包含 NaNs 的线，它们是没有交易活动发生的分笔成交点。

并运行摘要:

`` `{r}
摘要(bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31)
` '

![](img/c152d8eb8a9a693b2cea2f0a3baf1d1c.png)

图 2:数据集摘要，注意所有列的最大值和最小值之间的差异，这是因为比特币的价格大幅上涨，这就是为什么我们将对回报而不是价格建模

我们的兴趣在于评估每日回报，因此我们将我们的数据框架分为 24 小时时段:

`` `{ r }
bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31 = DropNA(bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31，Var = "Close "，message = FALSE)

bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31[' Data ']= any date(bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31[1，' Timestamp'])

bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31[is . na(bitstampUSD _ 1 . min _ data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31)]

` `{ r }
for(I in 1:nrow(bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31)){
bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31[I，' Data ']<-any date(bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01

` `{ r }
df<-Data . table(bitstampUSD _ 1 . min _ Data _ 2012 . 01 . 01 _ to _ 2020 . 12 . 31)
a = aggregate(df $ Low，by=list(df$Data)，min)
names(a)【1】<-c【Data】
names(a)【2】<-c【Low】
b = aggregate(df $ High，by

现在我们已经格式化了数据，我们将插入一个包含每日回报的列—一天中的百分比变化:

` `{ r }
df[' retorno ']= NaN
df['中']=NaN
df[1，'中']=(df[1，'高']-df[1，'低'])/2+df[1，'低']
for(I in 2:nrow(df)){
df[I，'中']=(df[i，'高']-df[i，'低'])/2+df[i，'低']。

```

现在，让我们建立一个回报直方图:

`` `{r，echo = FALSE }
# hist(DropNA(df[' Retorno '])，breaks = 10)
qplot(DropNA(df[' Retorno '])，
geom="histogram "，
binwidth = 0.005，
main = " Histograma do Retorno do Par 比特币 x USD "，
xlab = "Retorno "，
fill=I("blue ")，【t

![](img/a8e5d6ae68a62b2c21d4315aa9c9afec.png)

图 03:以美元计算的比特币回报直方图，我们可以观察到该图中存在厚尾和长尾，这直观地表明正态分布不太可能恰当地捕捉到这些极端事件的概率。

我们现在将继续为每个分布创建随机样本，并执行 Kolmogorov-Smirnov 测试，以评估哪个分布最符合比特币的价格。

正常:

` `{ r }
df _ test = r norm(length(DropNA(df[' retorno ']))，mean=mean(DropNA(df['retorno']))，SD = SD(DropNA(df[' retorno ']))
ks . test(DropNA(df[' retorno '])，df _ test)
`'

学生:

` `{ r }
df _ test = rt(length(DropNA(df[' retorno ']))，length(DropNA(df[' retorno '])-1)
ks . test(DropNA(df[' retorno '])，df _ test)

```

拉普拉斯:

` `{ r }
df _ test = rla place(length(DropNA(df[' retorno ']))，mean(DropNA(df['retorno']))，SD(DropNA(df[' retorno ']))
ks . test(DropNA(df[' retorno '])，df _ test)
`'

察利斯:

` `{ r }
df _ test = rtsal(length(DropNA(df[' retorno ']))，mean(DropNA(df['retorno'])，SD(DropNA(df[' retorno ']))
ks . test(DropNA(df[' retorno '])，df _ test)
`'

幂律:

` `{ r }
df _ test = rpl con(length(DropNA(df[' retorno ']))，-0.3，SD(DropNA(df[' retorno ']))
ks . test(DropNA(df[' retorno '])，df _ test)
`

D 统计是:

正常:0.14951

学生:0.45524

拉普拉斯:0.1285

察利斯:0.99178

幂律:0.80207

似乎我们有一个赢家:拉普拉斯，但紧随其后的是正常。

## 结论

然后，我们得出结论，在上述 Kolmogorv-Smirnov 统计标准下，拉普拉斯分布是最接近比特币价格分布的分布。但是，正态分布也提供了合理的拟合质量，因此可能存在使用正态分布仍然是合理方法的情况。

这一事实可以作为与加密货币金融期权定价相关的研究计划的输入，为此，一种可能的方法是用拉普拉斯分布代替布莱克-斯科尔斯模型中的正态分布，这样我们就能够根据历史数据估计期权的市场价值。
这些资产的期权。

## 参考资料:

[1]中本聪——比特币:一个点对点的电子现金系统:【https://bitcoin.org/bitcoin.pdf 

[2]弗拉德·莫罗，佩德罗·古伊列梅——比特币。uma anáLise de oportunidades e riscos do ponto de vista finance iro:[https://www . LinkedIn . com/in/Pedro-guil herme-frade-Moro-79046579/detail/treasury/education:317754845/？entity urn = urn % 3 Ali % 3 afsd _ profiletmeasurymedia % 3A(acoabcejbwblnhghwn 5 mqjfw 7 zfbnbfv 7 qkjiy % 2c 1506881417544)&section = education % 3A 317754845&treasury count = 1](https://www.linkedin.com/in/pedro-guilherme-frade-moro-79046579/detail/treasury/education:317754845/?entityUrn=urn%3Ali%3Afsd_profileTreasuryMedia%3A(ACoAABCejBwBLNHGHwn5mqjFw7zfbnbfv7qKjiY%2C1506881417544)&section=education%3A317754845&treasuryCount=1)

[3][https://en . Wikipedia . org/wiki/Kolmogorov % E2 % 80% 93 Smirnov _ test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test)

[4]沃波尔，罗纳德；工程师和科学家的概率和统计。

[5]纳西姆·塔勒布

[https://en.wikipedia.org/wiki/Student%27s_t-distribution](https://en.wikipedia.org/wiki/Student%27s_t-distribution)

[https://en.wikipedia.org/wiki/Gamma_distribution](https://en.wikipedia.org/wiki/Gamma_distribution)

【8】[http://www . MAS . ncl . AC . uk/~ nl F8/teaching/MAS 2317/notes/chapter 3 . pdf](http://www.mas.ncl.ac.uk/~nlf8/teaching/mas2317/notes/chapter3.pdf)

[9] [https://en.wikipedia.org/wiki/Laplace_distribution](https://en.wikipedia.org/wiki/Laplace_distribution)

[10] [https://en.wikipedia.org/wiki/Pareto_distribution](https://en.wikipedia.org/wiki/Pareto_distribution)

[11] [https://en.wikipedia.org/wiki/Weibull_distribution](https://en.wikipedia.org/wiki/Weibull_distribution)

[12] [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7217033/#:~:text=The%20estimated%20median%20of%20incubation,47%2C%2021%C2%B762](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7217033/#:~:text=The%20estimated%20median%20of%20incubation,47%2C%2021%C2%B762)

[13] [https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test)

[14] [https://www.bitstamp.net/terms-of-use/sa/](https://www.bitstamp.net/terms-of-use/sa/)

[15] [https://www.journaldenbusiness.pt/mercados/details/euronext-下载-交易时间-更短-失败共识#:~:text=那些%20hor%C3%A1rios%20das%20bolsa%20da,和%2017h%20na%20 小时%20 本地](https://www.jornaldenegocios.pt/mercados/detalhe/euronext-descarta-horario-de-negociacao-mais-curto-apos-falha-de-consenso#:~:text=Os%20hor%C3%A1rios%20das%20bolsas%20da,e%2017h%20na%20hora%20local) 。

[16] — Guazelli, V.B. — 非高斯分布的欧洲期权升值