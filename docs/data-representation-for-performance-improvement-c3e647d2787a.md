# 用于性能改进的数据表示

> 原文：<https://medium.com/analytics-vidhya/data-representation-for-performance-improvement-c3e647d2787a?source=collection_archive---------13----------------------->

# 调整您的特征以满足您的模型

![](img/aca80b461f424325bbfa7edb3c7e232a.png)

卢卡斯·布拉塞克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我的学习过程中，我意识到工程特征的概念更多的是一门艺术而不是科学，但是，有一些基本的概念是必须学习的。不同的作者试图给出在工程特性中重要的过程的简洁表示。本文将在这些先前工作的知识的基础上，通过一些实际的例子向前推进。

表示要素和提高模型性能有多种不同的方法。表示数据的理想方式不仅取决于数据，还取决于所用模型的类型。在本文中，我们将考察**宁滨或离散化的方法、多项式特性和相互作用。**

让我们深入研究一下。

首先，我们需要数据！我们将使用 sklearn 的 [dataset.make_regression 函数来生成我们的回归数据。](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_regression.html)

导入必要的库

```
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor
import sklearn.datasets as datasets
import matplotlib.pyplot as plt
import numpy as np
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import PolynomialFeatures
from sklearn.svm import SVR
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
```

由于 make_regression 函数使用线性函数生成数据，我们的线性模型可能会模拟得过于完美，这对实验不利。

```
data=datasets.make_regression(n_samples=100,n_features=1,noise=0.9,tail_strength=0.8,bias=0.9)
X,y=data
noise = np.random.normal(0,1,100).reshape(100,1) # Generate noise data
X=X+noise # Add noise to the data
```

我们对数据拟合线性模型和决策树回归器。决策树模型可以模拟相对复杂的关系，如下所示

预测强度的这种显示可以使用线性模型通过以模型更好理解的方式充分表示数据来建模。

这是使用**宁滨或离散化过程完成的。**这包括将特征分割成多个特征。这类似于在 matplolib 中为绘制直方图创建条块的过程。

我们创建一组任意确定的箱数(艺术！)并根据数据集所属的箱对数据集进行分组。我们使用数据集的最小值和最大值作为边界来创建 10 个箱。使用 NumPy 可以方便地做到这一点，如下所示:

```
bin_ = np.linspace(-5, 5, 11)
print("bin: {}".format(bin_))bin: [-5\. -4\. -3\. -2\. -1\.  0\.  1\.  2\.  3\.  4\.  5.]
```

在这种情况下，第一个容器包含范围从-5 到-4 的数据，第二个容器包含范围从-3 到-2 的数据，依此类推。

下一个过程包括将数据放入正确的箱中。这可以使用 NumPy 函数“数字化”来完成；

```
find_bin = np.digitize(X, bins=bin_)
```

这将数据的特征从连续值转换为离散值，这些离散值对它们所属的容器进行编码。

然后，我们使用 onehotencoder 将这些离散值转换为一键编码

```
# transform using the OneHotEncoder
encoder = OneHotEncoder(sparse=False,handle_unknown='ignore')
# encoder.fit finds the unique values that appear in which_bin
encoder.fit(find_bin)
# transform creates the one-hot encoding
X_binned = encoder.transform(which_bin)
```

上图显示了宁滨的强度，线性模型和决策树回归器预测每个输入的值相同，因此直接相互叠加，因为每个条柱内的特征是恒定的。

另一种改进特征表示的方法是使用多项式特征和交互(这是受其在统计建模中的使用的启发)。

**多项式和相互作用**

为了添加交互式要素，我们将已入库的数据与原始要素相结合，并尝试对新数据进行建模

```
X_combined = np.hstack([X, X_binned])
```

用线性回归对这种组合数据建模产生了这样的结果

线性模型试图基于组合的特征来学习不同箱处的不同偏移，然而斜率在箱之间似乎是相同的。为了改善这一点，让我们尝试一种不同的组合方法，我们添加一个产品交互

```
X_product = np.hstack([X_binned, X * X_binned])
```

该模型学习不同箱的不同斜率和偏移

添加交互增加了模型学习斜率和偏移的能力，让我们检查多项式特征的使用

**多项式特征**

多项式函数由下式给出

```
f(x)=ax+bx^2+cx^3+dx^4.... 
```

在我们的模型中使用多项式特征仅仅意味着使用所提供的原始特征的倍数，以便线性模型变成曲线并正确地对数据建模。

如下所示

```
poly = PolynomialFeatures(degree=5, include_bias=False)
poly.fit(X)
X_poly = poly.transform(X)
```

对新数据应用线性模型会产生以下结果

让我们将这个概念应用于 scikit learn 中提供的标准数据集；波士顿住房数据集

```
boston = load_boston()
X_train, X_test, y_train, y_test = train_test_split(
boston.data, boston.target, random_state=0)
# rescale data
scaler = MinMaxScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
poly = PolynomialFeatures(degree=2).fit(X_train_scaled)
X_train_poly = poly.transform(X_train_scaled)
```

使用岭回归对此数据集建模

```
ridge = Ridge().fit(X_train_scaled, y_train)
print("Score without interactions: {:.3f}".format(
ridge.score(X_test_scaled, y_test)))
ridge = Ridge().fit(X_train_poly, y_train)
print("Score with interactions: {:.3f}".format(
ridge.score(X_test_poly, y_test)))**Score without interactions: 0.621
Score with interactions: 0.753**
```

总之，要素的表示对提高线性模型的性能大有帮助，但是，由于要素工程是一门艺术，因此应谨慎使用，有多种方法可以实现这一点，但并不总是能提供更好的性能。

我很喜欢写这篇文章，我希望你也喜欢读它。