# 提高你的数字技能

> 原文：<https://medium.com/analytics-vidhya/brush-up-your-numpy-skills-936be9593fe?source=collection_archive---------9----------------------->

数据科学关注的是成比例的结构化数据表。scikit-learn 包需要二维 NumPy 数组作为输入表。

在本文中，我们将修改 NumPy 库的非常基本的概念，它可以成为更大项目中的帮手。

# **NumPy 阵列的形状和尺寸**

1.  首先，导入 NumPy:

```
import numpy as np
```

2.构造一个 10 位数的 NumPy 数组，相当于 Python 的 range(15)技术:

```
np.arange(10)

Output: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

3.只有一对括号，该数组类似于 Python 列表。这表明它只有一维。通过存储数组来确定形状。

```
array_one = np.arange(10) 
array_one.shape Output: (10,)
```

4.Shape 是数组中的数据属性。在 array_one 中，形状是一个长度为 1 的元组(10)。元组的长度决定维度的数量，在这种情况下是 1:

```
array_one.ndim   #Find total number of dimensions of array_oneOutput: 1
```

5.数组中有十个条目。调用整形机制来整形数组:

```
array_one.reshape((5,2)) Output: array([[0, 1],  
               [2, 3],  
               [4, 5], 
               [6, 7],  
               [8, 9]])
```

6.该数组被整形为一个 5 x 2 的数据项，看起来像一个列表的列表(一个类似列表的列表的三维 NumPy 数组)。修改未被存储。只需以下列格式保存整形后的数组:

```
array_one = array_one.reshape((5,2))
```

7.值得注意的是，array_one 变成了二维。这是意料之中的，因为它的形状包含两个数字，类似于 Python 列表中的列表:

```
array_one.ndim Output: 2
```

# 数字广播

8.通过广播，可以给每个数组元素加任意数(取 1)。请务必注意，对阵列的更新不会保存:

```
array_one + 1Output: array([[ 1, 2],  
               [ 3, 4], 
               [ 5, 6],
               [ 7, 8], 
               [ 9, 10]])
```

较短的阵列被拉伸或广播到较大的阵列上，这被称为**【广播】**。

9.创建一个名为 array_two 的新数组。检查当您将数组本身相乘时会发生什么(这是基于元素的数组乘法，而不是矩阵乘法):

```
array_two = np.arange(10)
array_two * array_twoOutput: array([ 0, 1, 4, 9, 16, 25, 36, 49, 64, 81])
```

10.每个分量都被平方了。这里，一个元素接一个元素地相乘。这里有一个稍微复杂一点的例子:

```
array_two = array_two ** 2 
#Note that this is equivalent to array_two * array_two 
array_two = array_two.reshape((5,2)) 
array_twoOutput: array([[ 0, 1],  
               [ 4, 9],
               [16, 25], 
               [36, 49], 
               [64, 81]])
```

11.让我们也修改 array_one:

```
array_one = array_one + 1 
array_oneOutput:array([[ 1, 2],  
              [ 3, 4],  
              [ 5, 6], 
              [ 7, 8],
              [ 9, 10]])
```

12.只需在两个数组之间添加一个加号，就可以逐个元素地添加 array_one 和 array_two:

```
array_one + array_two Output: array([[ 1, 3], 
               [ 7, 13],
               [21, 31],
               [43, 57],
               [73, 91]])
```

# dtypes 和 NumPy 数组的初始化

除了 np.arange，还有各种其他方法来初始化 NumPy 数组:

13.使用 np.zeros 创建一个零数组。命令 np.zeros((2，5))产生一个 2×5 的零数组:

```
np.zeros((2,5))Output: array([[0., 0., 0., 0., 0.],
               [0., 0., 0., 0., 0.]])
```

14.使用 np.ones，创建一个 1 的数组。要验证它们是否为 NumPy 整数类型，请添加一个值为 np.int 的 dtype 参数。值得注意的是，scikit-learn 要求数组有 np.float 参数。NumPy 数组中的每个元素都有一个由 dtype 指定的类型。数组中的每一条路都是一样的。np.int 整数类型用于数组中的每个元素，如下所示:

```
np.ones((2,5), dtype = np.int)Output: array([[1, 1, 1, 1, 1],
               [1, 1, 1, 1, 1]])
```

15.要为具有特定大小和数据类型但没有特定填充值的数组分配内存，请使用 np.empty:

```
np.empty((2,5), dtype = np.float)Output: array([[0., 0., 0., 0., 0.],
               [0., 0., 0., 0., 0.]])
```

16.要为具有不同先前值的 NumPy 数组分配内存，请使用 np.zeros、np.ones 和 np.empty。

# 数字索引

17.使用索引，检索二维数组的值:

```
array_one[0,0] #Finds value in first row and first columnOutput: 1
```

18.请看第一行:

```
array_one[0,:] Output: array([1, 2])
```

19.现在，让我们看看第一列:

```
array_one[:,0] Output: array([1, 3, 5, 7, 9])
```

20.具体数值可以在两个轴上看到。也看看第二到第四排:

```
array_one[2:5, :] Output: array([[ 5, 6], 
               [ 7, 8], 
               [ 9, 10]])
```

21.只看第一列的第二到第四行:

```
array_one[2:5,0] Output: array([5, 7, 9])
```

# 布尔数组

NumPy 还使用布尔逻辑来控制索引:

22.最初创建一个布尔数组:

```
array_one > 5Output: array([[False, False], 
               [False, False],
               [False, True], 
               [ True, True],
               [ True, True]], dtype=bool)
```

23.将圆括号放在布尔数组的两边将允许您根据它进行筛选:

```
array_one[array_one > 5] Output: array([ 6, 7, 8, 9, 10])
```

# 基于算术的运算

24.sum 方法将数组中的所有元素相加。返回 array_one:

```
array_oneOutput: array([[ 1, 2], 
               [ 3, 4], 
               [ 5, 6],
               [ 7, 8], 
               [ 9, 10]])array_one.sum()Output: 55
```

25.按行查找所有的总和:

```
array_one.sum(axis = 1) Output: array([ 3, 7, 11, 15, 19])
```

26.要按列计算所有总和，请使用以下公式:

```
array_one.sum(axis = 0) Output: array([25, 30])
```

27.以类似的方式，计算每列的平均值。值得注意的是，averages 数组的 dtype 是 np.float:

```
array_one.mean(axis = 0)Output: array([ 5., 6.])
```

# 空值或 NaN 值

28.Scikit-learn 不接受 np.nan 值。以 array_three 为例:

```
array_three = np.array([np.nan, 0, 1, 2, np.nan])
```

29.np.isnan 方法创建一个特定的布尔数组，可用于查找 nan 值:

```
np.isnan(array_three)Output: array([ True, False, False, False, True], dtype=bool)
```

30.通过用符号~对布尔数组求反并用括号将表达式括起来来消除 NaN 值:

```
array_three[~np.isnan(array_three)] Output: array([ 0., 1., 2.])
```

31.作为一个选项，将 NaN 值设置为零:

```
array_three[np.isnan(array_three)] = 0 
array_threeOutput: array([ 0., 0., 1., 2., 0.])
```

# 结论

数据最基本的形式是由 NumPy 擅长处理的 2D 数字表组成的。如果您忘记了 NumPy 语法，请考虑这一点，Scikit-learn 只接受没有丢失 np.nan 值的绝对值的 2D NumPy 数组。

在我看来，将 np.nan 更改为一个值而不是传递数据似乎效果最好。最好跟踪布尔表达式并保持数据形式的一致性，因为这样可以减少编码错误，从而提高编码的通用性。