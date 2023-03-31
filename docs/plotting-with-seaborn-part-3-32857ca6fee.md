# 用 seaborn 绘图-第 3 部分

> 原文：<https://medium.com/analytics-vidhya/plotting-with-seaborn-part-3-32857ca6fee?source=collection_archive---------6----------------------->

![](img/9316b5ffacd2dc97591f708b63149d43.png)

# 显示

该功能提供了几种可视化单变量或双变量数据分布的方法，包括通过语义映射和跨多个子图分面定义的数据子集。`kind`参数选择要使用的方法:

```
histplot**()** (with kind="hist"; the default)kdeplot**()** (with kind="kde")ecdfplot**()** (with kind="ecdf"; univariate-only)
```

正在加载数据集。

```
d = sns.load_dataset("tips")
```

默认图是直方图。

```
sns.displot(data=d, x = "total_bill")
```

![](img/d0620ef60b1f326eea18d3c43334ac7b.png)

我们可以使用 kind 参数来选择不同的表示。

```
sns.displot(data=d, x = “total_bill”,kind=”kde”)
```

![](img/c7eed52b907b52c3a65b8c355784314b.png)

在直方图模式下，也可以添加 KDE 曲线

```
sns.displot(data=d, x = "total_bill",kde=True)
```

![](img/c10d1ad307f28812dbecee9acb9d2133.png)

要绘制二元图，请指定`x`和`y`

```
sns.displot(data=d,x="size",y="total_bill")
```

![](img/b89e4712e0dc13320e579b3d70c99e62.png)

```
sns.displot(data=d,x="size",y="total_bill",kind="kde")
```

![](img/27de3be8e5250b2e30de39997fb7e654.png)

用边缘“地毯”显示个人观察结果:

```
sns.displot(data=d,x="size",y="total_bill",kind="kde",rug=True)
```

![](img/4ecc577665c50e8287ce1203d42e1b81.png)

使用`hue`映射，可以为数据子集单独绘制每种图

```
sns.displot(data=d,x="total_bill",kind="kde",hue="sex")
```

![](img/3ea2916c341cc8806fe710f6ba356998.png)

```
sns.displot(data=d,x="total_bill",kind="kde",hue="day")
```

![](img/b81d86d36fb1b153ee552e8664526a1a.png)

```
sns.displot(data=d,x="total_bill",kind="kde",hue="day",
multiple="stack")
```

![](img/2b03d8e2c10232c367e846b2ce9b0a73.png)

```
sns.displot(data=d,x="total_bill",hue="day",multiple="stack")
```

![](img/54bb25351c84eb44bd0b42ef56461382.png)

这个图形是使用`FacetGrid`构建的，这意味着你也可以显示不同支线剧情的子集

```
sns.displot(data=d, x="total_bill", hue="smoker", col="sex", kind="kde")
```

![](img/7ad0d8c813fb7c0fc5fa117dd49c4075.png)

# **ecdfplot**

ECDF 表示低于数据集中每个唯一值的观察值的比例或计数。与直方图或密度图相比，它的优点是每个观察结果都是直接可视化的，这意味着不需要调整宁滨或平滑参数。它还有助于在多个分布之间进行直接比较。一个缺点是，图的外观和分布的基本属性(如其中心趋势、方差和任何双峰的存在)之间的关系可能不直观。

沿 x 轴绘制一元分布图:

```
sns.ecdfplot(data=d, x="total_bill")
```

![](img/e73c812cf9e5bfb1e2f4f48e4ffc9755.png)

可以绘制来自具有色调映射的长格式数据集的多个直方图。

```
sns.ecdfplot(data=d, x="total_bill",hue="day")
```

![](img/586af9e061779d4a9aa2b57e2fd946d3.png)

```
sns.ecdfplot(data=d, x=”total_bill”,hue=”size”)
```

![](img/f5f08476c6b87cf223b7fff575ca9db9.png)

默认的分布统计数据被归一化以显示比例，但您也可以显示绝对计数:

```
sns.ecdfplot(data=d, x="total_bill",hue="size",stat="count")
```

![](img/e0286faa3f02a231fd4b19b85fdff422.png)

也可以绘制经验互补图:

```
sns.ecdfplot(data=d,x="total_bill",hue="size",
stat="count",complementary=True)
```

![](img/63c78c8d424275a9fe62ce609291e000.png)