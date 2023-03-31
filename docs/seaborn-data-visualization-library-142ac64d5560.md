# Seaborn:数据可视化库

> 原文：<https://medium.com/analytics-vidhya/seaborn-data-visualization-library-142ac64d5560?source=collection_archive---------3----------------------->

数据可视化通过将数据整理成更容易理解的形式，突出趋势和异常值，有助于讲述故事。

![](img/e1e98d4cd0706856b0468547ca08e243.png)

在这篇博客中，我们将使用 *matplotlib* 的一些函数来了解 *seaborn* 。

**安装 Matplotlib 和 Seaborn**

```
pip install matplotlib
pip install seaborn
```

**导入 Matplotlib & Seaborn**

```
import matplotlib.pyplot as plt
import seaborn as sns
```

**读取数据集**

这里，我们将使用取自 Kaggle 的“汽车”数据集。要下载数据集，请点击[此处](https://www.kaggle.com/toramky/automobile-dataset)。

要加载数据集，您需要使用以下命令安装 pandas 库:

```
import pandas as pd
```

![](img/86bbde9db5fa1bf450ae5ad1cdd8bdca.png)

# **线条图**

绘制正弦和余弦函数:

![](img/cfcf54af9588d1e240c321dea8629261.png)![](img/6f6684eaa92ff38d2572d493ea08e7f1.png)

**改变图形尺寸:**

![](img/3d2447978909257169d6702963f60cfb.png)

**恢复默认 matplotlib 设置**

![](img/3087bb775a69c1ef32894c2d1726058a.png)

**显示标记(圆点)**

![](img/baf59c84057e9ff283720e59e100dd4e.png)

使用 matplotlib 设置标题、xlabel 和 ylabel

![](img/496fe717c3161da9a3e6ebac83b15ace.png)

**将 x 轴旋转 90 度**

![](img/f91b05ad0d9722ad799d5a9024ddf166.png)

**使用'*色调'*** 对地块进行分组

![](img/7f2c8f10f474b3805ec99d92b6baaf30.png)

**设置绘图的线宽、线型和颜色**

让我们在轴距和发动机尺寸之间画一个线图

![](img/8cc747318df018fc9e0ae174f7ad494c.png)

**使用调色板:**

![](img/e2c158b2022d9aadffb67e221a9bad18.png)![](img/cf573c7a48affc8bccbfbb6ed51c3220.png)

**创建调色板**

![](img/2e8c08cc6442fc304f2a9f8f98688a3f.png)![](img/23b919787b0655dc5eb048d861ffb76d.png)

# 散点图

在散点图中，每个观察值显示为 x 和 y 值处的点。它用于可视化二元分布。散点图表示变量之间的关系。

![](img/3708f33ab9ca87ff8434aa4707eed51c.png)![](img/4fa100ba1297a879846a405b26b1b60d.png)

# **联合剧情**

联合绘图显示了两个变量之间的关系以及边缘的 1D 剖面。

![](img/37c5ed1ecd85b0b98c5a40c9e2f04259.png)![](img/afcc0aef1d2de3295a08f4766ed1debe.png)![](img/6f4caeb4f18aab0ad26267eb3cda90d0.png)

# 条形图

条形图显示数字变量和分类变量之间的关系。它默认显示平均值。

![](img/4c5a1b6e6e6e02f7e2c192a41283c5b8.png)

**排序条形图**

使用参数' *order* ，我们可以对不同的条形进行排序。

![](img/4a078e8b71005c6e0a7481ef704004fb.png)

**水平条形图:**

要绘制水平条形图，我们只需要交换 x 和 y 值。

![](img/a5662019b18596fd2ae7a89c612e31f1.png)

# **盘点剧情**

计数图有点像某个分类区域的直方图或条形图。它显示了基于某个类别的某个项目出现的次数。

![](img/3586a5b87728f4512c810a52e3ed3506.png)

**排序顺序**

使用'*命令*变元

![](img/f9adf8f44b8f7d1b31a3b69043f61eff.png)

**添加色调**

![](img/8ed05fc05163fbf5f2ed63d443a935b8.png)

# **方框图**

箱形图显示最小值、第一个四分位数、中值、第三个四分位数和最大值。为四分位数范围(Q3 - Q1)画一个方框。一条线穿过中线。

![](img/a3ef6e1cfdeac61c0fc3397d6ef2d98f.png)![](img/9a8880441850148ecb47ec4a18f5b9c8.png)![](img/9e4f32910bd5cf7537ef855c29654661.png)

***添加色相、调色板和顺序***

![](img/1b21e5233183a1ab35c38e48c220ac0a.png)

# 小提琴情节

violin plot 用于绘制数字数据。它类似于箱线图，只是它们也显示数据的概率密度。

![](img/0fffff309dae1b66ba3f07f0762e48f7.png)![](img/f3a769294894bcaeca9032b48c187c99.png)![](img/2066fc451ca9f0b61b4ec82ae9e301ea.png)

# **猫情节**

猫图用于绘制分类变量。它使用不同的图显示数字变量和分类变量之间的关系。

下面给出的代码将根据“col”参数(燃料类型)中给出的分类变量划分数据点。

![](img/b3eab4640ba2116f9d67d77455a4e2ab.png)

下面给出的代码将根据燃料类型划分数据点，并绘制计数图。

![](img/81d6c3b637715c6f4e031607f3dda213.png)

*使用“col_wrap”来限制一行中的绘图数量。*

![](img/2b027691344a7c5c5b006efd262a4e42.png)

# **距离图**

![](img/4d57e6685bced123a452d154c0c9f7bb.png)

# 直方图

在直方图中，y 轴表示每列数据中出现的次数或百分比。

![](img/dedd29c41ce8a17dabc487428cd3f72e.png)![](img/ac3bf78d74391ca107f5ea59bae7083d.png)

**绘制数据集中所有数值变量的直方图:**

![](img/cdd217d926fcf37dc6d5f5f6d0c391c6.png)

# kde 图

![](img/ead6b602d6002a39409332d0036019c2.png)![](img/11b7602becb20663dbaa1a43b8ce165f.png)

# **配对图**

配对图允许我们看到单个变量的分布和两个变量之间的关系。

![](img/b7777704c969893ff198ed41ec85d4ad.png)![](img/85dae59bcc5d89db41af95e3d947ad1d.png)

# **回归图**

回归图在两个参数之间创建一条回归线，并帮助可视化它们的线性关系。

![](img/9547a23018db3eb5bf2e377d23d1218b.png)

# **热图**

![](img/b00ff71beeaf66c39178b09c46e444c8.png)![](img/65fb56fd84c7818fa2400aa25b1e3a09.png)

要了解更多关于 matplotlib 和 seaborn 的信息，请参考官方文档- [seaborn](https://seaborn.pydata.org/) 和 [matplotlib](https://matplotlib.org/) 。

如果你喜欢这个博客，那么看看我以前的一些博客:

1.  [熊猫](https://khushijain2810.medium.com/pandas-python-data-analysis-library-1d061c982fc8)
2.  [Numpy](https://khushijain2810.medium.com/numpy-day-3-at-internity-foundation-efcef826e549)

快乐学习！！