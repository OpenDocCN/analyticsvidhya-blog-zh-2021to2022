# 视觉表现

> 原文：<https://medium.com/analytics-vidhya/visual-representations-2acd9cda0b03?source=collection_archive---------11----------------------->

这是我上一篇文章的后续，关于学习商业统计以便找到数据科学工作，如果你没有看过我的第一篇文章，请在[这里](https://rcrp.medium.com/starting-with-statistics-first-5f3d531338a2)查看。

![](img/e9055bd2cf888a3dbffaf386c232cf97.png)

因此，本文将涵盖大卫·r·安德森、丹尼斯·j·斯威尼和托马斯·a·威廉姆斯所著的《商业和经济学统计》一书第二章的第一章和第二章的一半的内容，如果你想看的话，我准备看第十一版。

在我们开始讲述我最大的收获之前，我想告诉你一些我在前几章一直在思考的事情:

> 如果你想走这条路，你需要更多地依靠文档学习，而不是从免费的互联网内容中学习。

为什么？嗯，这篇文章将会涉及一些非常早期的概念，但更多的是关于可视化表示，在 python 上做这些可能会非常棘手。最棘手的部分是关于如何输入数据，以及你的输入将如何给出期望的输出，我在网上查看了一些内容，试图帮助我，其中一些只是让你去复制屏幕另一边的人正在做的事情，这与现实相差甚远(即使是让你思考的练习，也基本上是复制和粘贴)。

鉴于这种情况，我不得不选择我选择的工具，以及它们如何协同工作，因为我的计算机非常旧(我运行的是 4GB RAM，所以为我将来祈祷)，我将使用 Jupiter 笔记本作为我的 IDE，用于运行每个单独的单元，Pandas 用于数据操作，只是因为我现在更喜欢它，但我正在学习如何在必要时切换到 Numpy 和 Plotly 以实现数据可视化，因为很明显，Matplotlib 在 Jupiter 上工作得不太好，我在运行它时有一个令人讨厌的 bug。

另一件我发现自己在第一章就已经感到惊讶的事情是，这本书所涵盖的概念的数量经常与数据科学联系在一起。推理、数据挖掘、时间序列、定量数据，jazz 真正解释的所有这些东西，都给了我一些基础。

现在你已经熟悉了，让我们来看看我到目前为止做了什么。

## 数字事实和统计

第一章的重点是以一种非常巧妙的方式，让你了解统计学到底是关于什么的，让你明白这门学科和你在报纸上读到的关于某个主题的统计学之间的区别。

那么什么是统计学呢？收集、分析、展示和解释数据，但这是科学吗？嗯，我绝对不是你应该问这个问题的人，但根据这本书，这被认为是一门艺术，因为数据可以有多种解释，这通常是在一些集合或一些方法上的讨论，以更客观的方式找到一些问题的答案。

所以，现在我要谈谈一些概念，澄清一些我没有看到的东西，但它们在解决问题的过程中非常重要。

首先，我们来说一个时间序列，嗯，时间序列是一段时间内收集的数据的数据集，横截面数据是在某个时间点收集的数据。如果你像我一样，已经研究了一些关于机器学习的东西，那么，你知道时间序列和预测是很难实现的，所以你看到的很有可能是一个横截面数据集，就像先知一样。

第二，人口，这是所有元素的集合，人口基本上是所有数据的 blob，它具有你正在处理的形状和特征。现在，一个样本是你的人口中的一小部分，一个好的样本是与你的斑点有相同形状和特征的样本，有一些偏差，毕竟是一个较小的斑点，但在应用于所有人口之前尝试一些东西是好的。

最后，每个人都至少玩过一次某种彩票，不管是不是为了好玩，你也会问自己，“那么，有了适量的数据，我会中对号码吗？”。我的朋友们，这是一个很好的统计推断的例子，你可以用数据来估计或测试一个假设。基本上这是数据科学家需要非常熟悉的东西。

## 我如何看到我的斑点？

第二章和第三章的重点是如何总结你的数据，可以有多种方式。第二章重点介绍描述性统计的可视化表示，这是一套可用于汇总数据的技术。

总结你的数据是很重要的，因为它可以给你人口的形状和特征，如果你想应用一些算法的话，这是非常重要的一步。正如我在第一部分所说的，我在这里陷入了困境，开始对我的速度非常自信，但后来变得非常慢。为了展示，这是我的第一张图表，第一章第 14 题，我花了三天时间想出来的，比我预期的要多得多:

```
# 14.import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
csm_data = {"Manufacturer":['General Motors', 'Ford', 'DaimlerChrysler', 'Toyota'], 
           "2004":[8.9, 7.8, 4.1, 7.8], 
           "2005":[9, 7.7, 4.2, 8.3], 
           "2006":[8.9, 7.8, 4.3, 9.1], 
           "2007":[8.8, 7.9, 4.6, 9.6]}
df_csm_data = pd.DataFrame(csm_data)
print(df_csm_data)
# A)
"""Setup data"""
tdf_csm_data = df_csm_data.set_index('Manufacturer').T.reset_index()
year = tdf_csm_data.iloc[:, 0]
car_production_values = [tdf_csm_data["General Motors"], tdf_csm_data["Ford"], tdf_csm_data["DaimlerChrysler"], tdf_csm_data["Toyota"]]
print('\n', tdf_csm_data)
"""Build histogram"""
fig1 = px.line(tdf_csm_data, 
            x=year, 
            y=car_production_values
)
fig2 = px.scatter(tdf_csm_data, 
               x=year, 
               y=car_production_values, 
               size=[5, 5, 5, 5], 
               opacity = 1
)fig3 = go.Figure(data=fig1.data + fig2.data)
fig3.update_layout(title='Global Auto Production', 
                  xaxis_title='Year', 
                  yaxis_title='Car production (millions)', 
                  legend_title='Automotive Brands')
fig3.show()
```

这是我在第二章第 21 题中制作的最后一张图表:

```
# 21.
computer = pd.read_excel('~/Documentos/projetos_ds/livro_1/excel_files/ch_02/Computer.xlsx')# A/B)interval_1 = (computer['Hours'] <= 3).sum()
interval_2 = ((computer['Hours'] >= 4) & (computer['Hours'] <= 7)).sum()
interval_3 = ((computer['Hours'] >= 8) & (computer['Hours'] <= 11)).sum()
interval_4 = ((computer['Hours'] >= 12) & (computer['Hours'] <= 13)).sum()
interval_5 = (computer['Hours'] > 14).sum()computer_freq = {'Intervals':['< 3', '4-7', '8-11', '12-14', '> 14'], 
                 'Frequency':[interval_1, interval_2, interval_3, interval_4, interval_5]}
df_computer_freq = pd.DataFrame(computer_freq)
df_computer_freq['Relative Frequency'] = (df_computer_freq['Frequency']/df_computer_freq['Frequency'].sum())
df_computer_freq['Cumulative Frequency'] = df_computer_freq['Frequency'].cumsum()print(df_computer_freq)# C)fig = px.histogram(x=df_computer_freq['Intervals'], 
              y=df_computer_freq['Frequency'], 
              title='Hour of computer home usage', 
              labels={'x':'Intervals', 'y':'Frequency'})
fig.show()# D)fig = px.scatter(x=[3, 7, 11, 14, 17], 
                 y=df_computer_freq['Cumulative Frequency']
                 )
fig2 = px.line(x=[3, 7, 11, 14, 17], 
              y=df_computer_freq['Cumulative Frequency']
              )
fig3 = go.Figure(data=fig.data + fig2.data)
fig3.update_layout(title='Ogive', 
                  xaxis_title='Classes', 
                  yaxis_title='Cumulative Frequency')
fig3.show()
```

比第一个干净多了，对吧？另一件事，第一个图是一个时间序列，最后一个是一个卵形线，所以，卵形线是一个线和散点图，显示了从一个数据集的累积频率的数量。这就是你们在第二部分的前半部分主要展示的内容，直方图、条形图、散点图、点阵图，所有这些显示元素频率的东西就是你的人口，帮助你可视化一种时尚、一种均值或诸如此类的东西。

此外，它们都是一维表示，这意味着这些图表只显示数据集的一个变量，没有相关性，这些图表中没有变量关系，这是学习如何用 python 制作图表的很好的材料，最重要的是，如何改进它。

## 下一步是什么？

基本上，下一篇文章将是关于第二章的最后一部分，更多的维度被添加到图表中，以及我们可以用它做的一些事情，以及第三章，处理数据的数字摘要，几乎就是你每天在新闻上看到的。

我很期待，如果读到这里，谢谢你，看看关于这一切是如何开始的最后一篇[帖子](https://rcrp.medium.com/starting-with-statistics-first-5f3d531338a2)，留下掌声，如果你想给这篇文章添加点什么，请随意评论。

我仍然没有把它放在 Github 上，但是我把它放在这里，我对这篇文章的下一次更新应该有代码，如果你读了这篇文章，你可能很早就读过了，谢谢你的支持！

保重！