# Numpy 阵列上的计算；

> 原文：<https://medium.com/analytics-vidhya/computation-on-numpy-arrays-7e73c0931bc3?source=collection_archive---------11----------------------->

到目前为止，我们一直在讨论 Numpy 的一些基本细节；在这一节中，我们将深入探究 Numpy 在 Python 数据科学世界中如此重要的原因。

快速计算 Numpy 数组的关键是使用矢量化运算，通常通过 Numpy 的通用函数(ufuncs)来实现。矢量化方法旨在将循环推入 Numpy 底层的编译层，从而提高执行速度。Numpy 中的向量化操作是通过 ufuncs 实现的，其主要目的是对 Numpy 数组中的值快速执行重复操作。

为了阅读以前关于 Numpy 的文章，请访问下面的链接:

[](/analytics-vidhya/introduction-to-numpy-1eb3b3fe6843) [## Numpy 简介

### 数据集可以来自各种来源和各种格式，包括文档集合、集合…

medium.com](/analytics-vidhya/introduction-to-numpy-1eb3b3fe6843) [](/analytics-vidhya/the-basics-of-numpy-arrays-7b9703083bec) [## Numpy 数组的基础

### Python 中的数据操作几乎等同于 Numpy 数组操作，甚至像 Pandas 这样的新工具也被构建…

medium.com](/analytics-vidhya/the-basics-of-numpy-arrays-7b9703083bec) 

**探索 Numpy 的 Ufuncs:**

有两种类型的 uf unc:对单个输入进行操作的一元 uf unc 和对两个输入进行操作的二元 uf unc。

```
import numpy as np
# Array Arithmetic
x = np.arange(4)
print(x)
print(x+5) #addition
print(x-5) #subtraction
print(x*2) #multiplication
print(x/2) #division
print(x//2) #floor division
```

**计算绝对值:**

```
# Absolute value
x=np.array([-2,-1,0,1,2])
abs(x)
```

**三角函数:**

```
# Defining an array of angles
theta = np.linspace(0,np.pi,3)
print(theta)
print(np.sin(theta))
print(np.cos(theta))
print(np.tan(theta))
```

**反三角函数:**

```
x = [-1,0,1]
print(x)
print(np.arcsin(x))
print(np.arccos(x))
print(np.arctan(x))
```

**指数和对数:**

```
x = [1,2,3]
print(x)
print(np.exp(x))
print(np.exp2(x))
print(np.power(3,x))
```

**使用 Numpy 对数组中的值求和:**

```
# Summing the values in an array
L = np.random.random(100)
sum(L)
```

与 Python 内置函数相比，Numpy 的版本执行起来要快得多。

```
big_array = np.random.rand(1000000)
%timeit sum(big_array)
%timeit np.sum(big_array)
```

**计算数组的最小值和最大值:**

```
np.min(big_array)
np.max(big_array)
```

本文包含了使用 Ufuncs 对 Numpy 数组的计算。

在下面的 github 链接中找到完整的代码:

[](https://github.com/Pallavi885/Numpy/tree/main/Computation%20on%20Numpy%20Arrays) [## Pallavi885/Numpy

### 这个存储库由 Numpy 基础组成。在 GitHub 上创建帐户，为 Pallavi885/Numpy 开发做出贡献。

github.com](https://github.com/Pallavi885/Numpy/tree/main/Computation%20on%20Numpy%20Arrays)