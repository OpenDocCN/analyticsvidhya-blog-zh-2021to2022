# Tensorflow 2.0 基础

> 原文：<https://medium.com/analytics-vidhya/tensorflow-basics-f5d4a4d9022a?source=collection_archive---------27----------------------->

![](img/ac2b6caf65a9dc171e363d6560b2175e.png)

图片由来自 Pixabay 的 Markus Winkler 提供

在这本笔记本中，我们看到了 Tensorflow 2.0 的基础知识。本次笔记本备考期间最新的 tensorflow 版本是 2.4.0。要在您的系统上安装 tensorflow 或 tensorflow-gpu，请点击此链接

[](https://www.tensorflow.org/install/pip) [## 用 pip 安装 TensorFlow

### python 3-version pip 3-version sudo apt 更新 sudo apt 安装 python 3-dev python 3-pip python 3-venv 安装使用…

www.tensorflow.org](https://www.tensorflow.org/install/pip) 

```
import tensorflow as tf
tf.__version__'2.4.0'
```

# 常数

我们使用 tf.constant()函数定义一个简单的常数

```
# Defning a constant

tensor=tf.constant([[2,4], [5,6]])tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>
```

我们也可以从张量常数中检索值。想法是使用 numpy()函数提取值

```
# Getting value of constant

tensor.numpy()array([[2, 4],
       [5, 6]], dtype=int32)
```

我们也可以将 numpy 数组转换回 Tensorflow 张量

```
import numpy as np
np_tensor=np.array([[2,2,],[3,3]])np_tensorarray([[2, 2],
       [3, 3]])tensor_np=tf.constant(np_tensor)tensor_np<tf.Tensor: shape=(2, 2), dtype=int64, numpy=
array([[2, 2],
       [3, 3]])>
```

# 可变的

我们用 tf 定义一个简单的常数。变量()函数

```
# Defining a variable

tf_variable=tf.Variable([[1,2,3],[4,5,6]])tf_variable<tf.Variable 'Variable:0' shape=(2, 3) dtype=int32, numpy=
array([[1, 2, 3],
       [4, 5, 6]], dtype=int32)>
```

我们还可以使用 numpy()函数检索变量值，方法与从张量中检索值类似

```
tf_variable.numpy()array([[1, 2, 3],
       [4, 5, 6]], dtype=int32)
```

我们也可以改变变量中的特定值。上面的 tf_variable 由 3 x 2 数组组成，假设我们想将值 3 改为 100

```
tf_variable[0,2].assign(100)<tf.Variable 'UnreadVariable' shape=(2, 3) dtype=int32, numpy=
array([[  1,   2, 100],
       [  4,   5,   6]], dtype=int32)>tf_variable<tf.Variable 'Variable:0' shape=(2, 3) dtype=int32, numpy=
array([[  1,   2, 100],
       [  4,   5,   6]], dtype=int32)>
```

# 张量间的运算

我们可以对张量进行数学运算。让我们看一个在标量和张量之间执行加法的场景

```
tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>tensor+2<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[4, 6],
       [7, 8]], dtype=int32)>
```

让我们看一个在标量和张量之间执行乘法的场景

```
tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>tensor*2<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[ 4,  8],
       [10, 12]], dtype=int32)>
```

# 在张量上使用 numpy 函数

我们也可以对张量进行 numpy 运算。让我们看一个使用 square()和 sqrt()函数的场景。

```
# Getting square of all numbers in tensorflow object
tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>np.square(tensor)array([[ 4, 16],
       [25, 36]], dtype=int32)# Getting square root of all numbers 
tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>np.sqrt(tensor)array([[1.41421356, 2\.        ],
       [2.23606798, 2.44948974]])
```

# 两个张量之间的点积

就像在向量和矩阵中一样，我们可以在两个张量之间进行点积。

```
tensor<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[2, 4],
       [5, 6]], dtype=int32)>tensor_1=tensor+1
tensor_1<tf.Tensor: shape=(2, 2), dtype=int32, numpy=
array([[3, 5],
       [6, 7]], dtype=int32)>np.dot(tensor,tensor_1)array([[30, 38],
       [51, 67]], dtype=int32)
```

# Tensorflow 2.0 中的字符串

```
tf_string=tf.constant("Tensorflow")
tf_string<tf.Tensor: shape=(), dtype=string, numpy=b'Tensorflow'>
```

## 简单的字符串操作

```
# length of string
tf.strings.length(tf_string)<tf.Tensor: shape=(), dtype=int32, numpy=10># unicode decode string
tf.strings.unicode_decode(tf_string, "UTF-8")<tf.Tensor: shape=(10,), dtype=int32, numpy=array([ 84, 101, 110, 115, 111, 114, 102, 108, 111, 119], dtype=int32)>
```

## 存储字符串数组

```
tf_array=tf.constant(["machine learning", "deep learning","NLP"])tf_array<tf.Tensor: shape=(3,), dtype=string, numpy=array([b'machine learning', b'deep learning', b'NLP'], dtype=object)># Iterating through TF string array
for string in tf_array:
    print(string)tf.Tensor(b'machine learning', shape=(), dtype=string)
tf.Tensor(b'deep learning', shape=(), dtype=string)
tf.Tensor(b'NLP', shape=(), dtype=string)
```