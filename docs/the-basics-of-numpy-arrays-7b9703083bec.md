# Numpy 数组的基础

> 原文：<https://medium.com/analytics-vidhya/the-basics-of-numpy-arrays-7b9703083bec?source=collection_archive---------26----------------------->

![](img/8d03c2b2e9c1591b47d959e7cab717fd.png)

Python 中的数据操作几乎等同于 Numpy 数组操作，甚至像 Pandas 这样的新工具也是围绕 Numpy 数组构建的。本节将给出几个使用 Numpy 和操纵来访问数据和子数组，以及拆分、整形和连接数组的例子。

让我们从定义三个随机数组开始:一维、二维和三维数组。我们将使用 Numpy 的随机数生成器，我们将用一个设定值*播种*，以确保每次运行这段代码时都生成相同的随机数组。

要浏览 Numpy 简介，请点击以下链接:

[](https://spall6640.medium.com/introduction-to-numpy-1eb3b3fe6843) [## Numpy 简介

### 数据集可以来自各种来源和各种格式，包括文档集合、集合…

spall6640.medium.com](https://spall6640.medium.com/introduction-to-numpy-1eb3b3fe6843) 

```
import numpy as npnp.random.seed(0) # seed for reproducibilityx1 = np.random.randint(10, size=(3,4,5))
```

每个数组都有属性 ndim(维数)、shape(每个维的大小)、size(数组的总大小)、dtype(数组的数据类型)、itemsize(每个数组元素的大小)和 nbytes(数组的总大小)。

```
print("x1 ndim: ", x1.ndim)  #printing number of dimensions of array
print("x1 shape: ", x1.shape)  #printing the size of each dimension
print("x1 size: ", x1.size)  #printing total size of array
print("dtype: ", x1.dtype)  # printing the data type of array
print("itemsize: ", x1.itemsize) #printing size of each array element
print("nbytes: ", x1.nbytes)  #printing the total size of array
```

数组索引:访问单个元素

如果您熟悉 Python 列表中的索引，Numpy 数组的一维数组索引也是如此。

```
x2=np.array([5, 0, 3, 3, 7, 9])
x2[0] # print first element
x2[-1] #print last element
```

对于多维数组，可以使用逗号分隔的索引元组来访问项:

```
x3=np.array([[3, 5, 2, 4],
            [7, 6, 8, 8],
            [1, 6, 7, 7]])x3[0,0] #print first element
x3[2, -1] #print last element of 2nd row
```

修改数组的值:

```
x3[0, 0] = 12
```

数组切片:访问子数组

一维子阵列

Numpy 切片语法遵循标准 Python 列表的语法；要访问数组 x 的一部分，请使用以下命令:

x[开始:停止:步进]

```
x = np.arange(10)
x[:5] # first 5 elements
x[5:] #elements after index 5
x[::2] #every alternate element
x[::-1] # reversed array
```

多维子阵列

多维切片的工作方式相同，多个切片用逗号分隔。

```
x3=np.array([[3, 5, 2, 4],
            [7, 6, 8, 8],
            [1, 6, 7, 7]])x3[:2, :3] #two rows, three columns
x3[:3, ::2] #all rows, every alternate column
x3[::-1, ::-1] # subarray dimensions reversed together
x3[:, 0] #first column of x3
x3[0, :] # first row of x2
```

阵列的整形:

最灵活的整形方式是使用 shape()方法。

```
grid = np.arange(1, 10).reshape((3, 3))
```

数组串联

Numpy 中两个数组的连接主要是通过例程 np.concatenate、np.vstack 和 np.hstack 完成的。

```
x=np.array([1, 2, 3])
y=np.array([3, 2, 1])
np.concatenate([x,y])
```

垂直堆叠数组

```
x=np.array([1, 2, 3])
grid=np.array([[9, 8, 7],
              [6, 5, 4]])# vertically stack the arrays
np.vstack([x, grid])
```

水平堆叠数组

```
grid=np.array([[9, 8, 7],
              [6, 5, 4]])
y=np.array([[99],
           [99]])np.hstack([grid, y])
```

数组拆分:

与串联相反的是拆分，这是通过函数 np.split 实现的

```
x=[1, 2, 3, 99, 99, 3, 2, 1]
x1, x2, x3 = np.split(x, [3, 5])
print(x1, x2, x3)
```

要访问完整的代码，请访问以下链接:

[](https://github.com/Pallavi885/Numpy/tree/main/2.%20Basics%20of%20Numpy%20Arrays) [## Pallavi885/Numpy

### 这个存储库由 Numpy 基础组成。在 GitHub 上创建帐户，为 Pallavi885/Numpy 开发做出贡献。

github.com](https://github.com/Pallavi885/Numpy/tree/main/2.%20Basics%20of%20Numpy%20Arrays)