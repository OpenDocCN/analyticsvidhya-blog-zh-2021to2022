# 数据可视化简介

> 原文：<https://medium.com/analytics-vidhya/introduction-to-data-visualization-a63adcd5b042?source=collection_archive---------25----------------------->

**数据可视化**是信息和**数据**的图形化表示。通过使用图表、图形和地图等可视化元素，**数据可视化**工具提供了一种便捷的方式来查看和理解**数据**中的趋势、异常值和模式。

![](img/4d633fec3790d2390fde4c44b6f66374.png)

# Matplotlib

Matplotlib 是一个非常棒的 Python 可视化库，用于数组的 2D 绘图。Matplotlib 是一个基于 NumPy 数组的多平台数据可视化库，旨在与更广泛的 SciPy 堆栈一起工作。它是由约翰·亨特在 2002 年提出的。

可视化的最大好处之一是，它允许我们以易于理解的视觉方式直观地访问大量数据。Matplotlib 由几个图组成，如线形图、条形图、散点图、直方图等。

# Matplotlib 安装

```
python -m pip install -U matplotlib
```

# 导入 Matplotlib

```
from matplotlib import pyplot as plt
*or*
import matplotlib.pyplot as plt
```

# Matplotlib 中的基本图

Matplotlib 附带了各种各样的情节。图表有助于理解趋势、模式和建立关联。它们是对定量信息进行推理的典型工具。这里包括了一些样地。

# 入门指南

```
import matplotlib.pyplot as plt
import numpy as np
xpoints=np.array([0,5])
ypoints=np.array([0,250])
plt.plot(xpoints,ypoints)
plt.show()
```

![](img/314d74c61bbd778b3cfdb002eaf9b354.png)

```
#draw a line from position (1,3) to (1,8)
xpoints=np.array([1,8])
ypoints=np.array([3,10])
plt.plot(xpoints,ypoints)
plt.show()
```

![](img/31968418cb92a8e97f5c2cd08d18480d.png)

```
#plotting without line
#draw two points one at(1,3) and one at(8,10)
xpoints=np.array([1,8])
ypoints=np.array([3,10])
plt.plot(xpoints,ypoints,'o')
plt.show()
```

![](img/151249ff9cd2fda79eea65c27868a6ab.png)

```
#multiple points 
xpoints=np.array([2,4,5,8])
ypoints=np.array([3,7,1,5])
plt.plot(xpoints,ypoints)
plt.show()
```

![](img/79e3dab5b227070a7d676a17e4cc5851.png)

```
#if we do not specify x-axis then defaukt value is 0,1,2,3,etc.
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints)
plt.show()
```

![](img/ef66bf6b0bc3bd6f2a676a0de85d97d7.png)

```
#marker at each point
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints,marker='o')
plt.show()
```

![](img/cee8befb0d2aedd178d173532afd3bc0.png)

```
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints,marker='*')
plt.show()
```

![](img/cee8befb0d2aedd178d173532afd3bc0.png)

```
#format string
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints, 'o:r')   #dotted line :
plt.show()
```

![](img/c95a611948738dc0ba0e4a758d3c14b7.png)

```
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints,'o-b')   #solid line
plt.show()
```

![](img/5dc28ee7afde819a49995ddeffce1283.png)

```
ypoints=np.array([3,8,1,10,5])
plt.plot(ypoints,'o--b')   #dashed line
plt.show()
```

![](img/4f43ab04f1ea38ca76c7f18bb1471594.png)