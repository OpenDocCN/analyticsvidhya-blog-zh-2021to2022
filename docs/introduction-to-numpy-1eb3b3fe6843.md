# Numpy 简介

> 原文：<https://medium.com/analytics-vidhya/introduction-to-numpy-1eb3b3fe6843?source=collection_archive---------12----------------------->

![](img/d2308022a27df46d9e926f0b9c8bea0f.png)

数据集可以来自各种来源和各种格式，包括文档集、图像集、声音剪辑集或几乎任何其他内容。

例如，图像可以被认为是简单的二维数字阵列。声音片段可以被认为是强度对时间的一维数组。无论数据是什么，使其可分析的第一步是将它们转换成数字数组。

在某些方面，Numpy 数组类似于 Python 的内置列表类型，但是随着数组变大，Numpy 数组提供了更高效的存储和数据操作。

创建 Numpy 数组的方法:

**1。导入 Numpy 库**

```
import numpy as np
```

**2。从 Python 列表创建 Numpy 数组**

```
np.array([1, 2, 3, 4, 5])
```

**3。创建多维数组**

```
np.array([range(i,i+3) for i in [2,4,6]])
```

**4。从头开始创建数组**

创建零数组

```
np.zeros(10, dtype=int)
```

创建 1 的数组

```
np.ones((3,5), dtype=float)
```

**5。创建一个填充了 3.14 的 3*5 数组**

```
np.full((3,5), 3.14)
```

**6。创建一个用线性序列填充的数组，从 0 开始，到 20 结束**

```
np.arange(0,20,2)
```

**7。创建一个由五个值组成的数组，这些值在 0 和 1 之间均匀分布**

```
np.linspace(0,1,5)
```

**8。在 0 和 1 之间创建一个均匀分布的随机值的 3*3 数组**

```
np.random.random((3, 3))
```

**9。创建一个 3*3 正态分布的随机值，平均值为 0，标准差为 1**

```
np.random.normal(0, 1, (3, 3))
```

**10。创建一个 3*3 的单位矩阵**

```
np.eye(3)
```

点击下面的链接获取完整的代码供您参考:

[https://github.com/Pallavi885/Numpy](https://github.com/Pallavi885/Numpy)