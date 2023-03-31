# 探索 9/11 数据集

> 原文：<https://medium.com/analytics-vidhya/exploring-9-11-dataset-bf443b55f4cb?source=collection_archive---------27----------------------->

![](img/0ab5b1a2d3b7e07b76d9fd1c894398fd.png)

在这篇文章中，我们用 Python 查看了 9/11 数据集，看看我们能从中得出什么样的见解。

## 获取数据集

我们正在研究的数据集位于 Kaggle。你可以在[紧急事件-911 电话| Kaggle](https://www.kaggle.com/mchirico/montcoalert) 找到这个数据集

这是一个非常全面的数据集。它列出了包括位置、日期、呼叫类型、邮政编码、地址和描述在内的列。在这篇文章中，我们将探索数据集，不会应用任何机器学习算法。

我没有提到代码的输出，因为我已经上传了完整的 IPython 内核，在[分析调用类型| Kaggle](https://www.kaggle.com/prahauk/analysis-for-the-type-of-calls)

## 导入库

首先，我们将导入所需的库。

```
# Imports import pandas as pd import numpy as np import matplotlib.pyplot as plt from plotly import __version__ from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot import cufflinks as cf import plotly.graph_objects as go
```

这些只是任何 Python 笔记本中用于机器学习的标准库。

## 了解数据集

现在，我们已经导入了库，是时候理解数据集了。

这将显示数据集中涉及的列。我们有以下栏目:

```
['lat', 'lng', 'desc', 'zip', 'title', 'timeStamp', 'twp', 'addr', 'e']
```

现在，如果查看数据集，我们可以看到我们的目的需要一些列。我们将使用以下代码删除这些列:

```
df = df[['lat', 'lng', 'title', 'timeStamp', 'twp', 'e']] df.head(3)
```

## 清理数据集

首先，我们将看到空值，看看它们是否需要修复。

```
print(df.isnull().sum()) print(len(df))
```

当我们运行它时，我们看到 twp 列有 **293** 个空行。有 663522 行。如果我们删除 twp 为 null 的行，不会有太大的危害。毕竟，我无法获得这些行的数据。另一个选择是在这里引入**【未指定】**城镇。

接下来，我们看到数据集的**标题**列中的唯一值。

```
df['title'].unique()
```

运行上面的代码，返回如下内容:

```
array(['EMS: BACK PAINS/INJURY', 'EMS: DIABETIC EMERGENCY', 'Fire: GAS-ODOR/LEAK', 'EMS: CARDIAC EMERGENCY', 'EMS: DIZZINESS', 'EMS: HEAD INJURY', 'EMS: NAUSEA/VOMITING', 'EMS: RESPIRATORY EMERGENCY', 'EMS: SYNCOPAL EPISODE', 'Traffic: VEHICLE ACCIDENT -', 'EMS: VEHICLE ACCIDENT', 'Traffic: DISABLED VEHICLE -', 'Fire: APPLIANCE FIRE',
```

清晰的图案清晰可见。存在第一种类型，描述由冒号(:)分隔。我们用冒号分割数组项，以分离类型和描述。下面是实现这一点的代码:

```
df['type'] = df['title'].apply(lambda title: title.split(":")[0]) df['type explanation'] = df['title'].apply(lambda title: title.split(":")[1])
```

## 处理日期列

最后，我们需要处理日期列。当我们看到日期列的类型时，我们看到它的 **str** 类型。因此，我们将采取以下措施:

*   将日期列从 **str** 类型转换为 **date** 类型。
*   将数据集的索引设置为 datetime 列。
*   我们将日期时间分为四个部分:早晨、下午、晚上、夜晚。

下面是要实现的代码:

```
df['Day of Week'] = df['timeStamp'].apply(lambda time: time.dayofweek) df['Day of Week'] = df['Day of Week'].map({0: 'Monday', 1: 'Tuesday', 2: 'Wednesday', 3: 'Thursday', 4: 'Friday', 5: 'Saturday', 6: 'Sunday'}) df['Month No'] = df['timeStamp'].apply(lambda time: time.month) df['Month'] = df['Month No'].map({1: 'January', 2: 'Febuary', 3: 'March', 4: 'April', 5: 'May', 6: 'June',7: 'July', 8: 'August', 9: 'September', 10: 'October', 11: 'November',12: 'December'}) df['Hour'] = df['timeStamp'].apply(lambda time: time.hour) df.loc[(df.Hour >= 6) & (df.Hour < 12) , 'Time of Day'] = 'Morning' df.loc[(df.Hour >= 12) & (df.Hour < 15) , 'Time of Day'] = 'Afternoon' df.loc[(df.Hour >= 15) & (df.Hour < 18) , 'Time of Day'] = 'Evening' df.loc[(df.Hour >= 18) | (df.Hour < 6) , 'Time of Day'] = 'Night'
```

## 绘制图表

这里我没有粘贴图表。然而，我们将着眼于我们如何策划。首先，我们需要配置袖扣。

```
init_notebook_mode(connected=True) cf.go_offline()
```

我们绘制了以下图表:

*   用条形图绘制数值计数。
*   我们每天都看到打字的趋势。
*   每天的呼叫类型
*   每月类型和每小时类型
*   最后，我们绘制出地理日期和通话次数。

```
geo_data = df.groupby(['lat', 'lng']).size().reset_index().rename(columns={0: "no of calls"}) fig = go.Figure(data=go.Scattergeo( locationmode = 'USA-states', lon = geo_data['lng'], lat = geo_data['lat'], text = geo_data['no of calls'], mode = 'markers', marker = dict( size = 2, opacity = 0.8, reversescale = True, autocolorscale = False, line = dict( width=1, color='rgba(102, 102, 102)' ), colorscale = 'Blues', cmin = 0, color = geo_data['no of calls'], cmax = geo_data['no of calls'].max(), colorbar_title="Plot of the Calls" ))) fig.update_layout( title = 'Hover for County Name', geo = dict( scope='usa', projection_type='albers usa', showland = True, landcolor = "rgb(250, 250, 250)", subunitcolor = "rgb(217, 217, 217)", countrycolor = "rgb(217, 217, 217)", countrywidth = 0.5, subunitwidth = 0.5 ), ) fig.show()
```

## 结论

这是一篇简单的文章，我们在其中对一个奇妙的数据集进行了一些分析。这样做，我们可以看到什么时候打电话，哪种类型的电话是在哪天打的，每月等等。

你可以自由使用这段代码并分享文章。跟随我的 [*Github*](https://github.com/umairk83) *资源库找到完整代码。*

*最初发表于 2021 年 1 月 1 日 https://buildwithuk.com**的* [*。*](https://buildwithuk.com/2021/01/02/exploring-9-11-dataset/)