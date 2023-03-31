# 随机森林(🎄🌴🌲)对于回归…

> 原文：<https://medium.com/analytics-vidhya/random-forest-for-regression-199ef5e8c34c?source=collection_archive---------15----------------------->

![](img/d427c707cf423e8be9fc4e467b8008fd.png)

随机森林

随机森林或随机决策森林，是一种*机器学习算法。*它也是使用最多的算法之一，因为它很简单。

随机森林是一种使用“装袋”方法训练几个决策树的集成方法。

在了解随机森林直觉之前，我们先了解一下

## **为什么，随机森林？**

随机森林是决策树的替代品。像森林一样，随机森林算法由多个单独的决策树组成。它**减少模型的方差而不增加偏差**。它也适用于高维数据，并且比决策树训练速度更快。

> 事实:
> 随机森林经常被用作商业中的“黑箱”模型，其背后的原因是它们可以通过大范围的数据生成合理的预测。

## 随机森林算法背后的理论:

随机森林应用 **bootstrap 聚合**或 **Bagging 算法的通用技术。**

> **重复(B 次)的 Bagging 算法**选择随机样本，替换训练集，并使树适合这些样本。

在决策树中，单个树的预测对其训练集中的噪声高度敏感，但是在自举过程的情况下，我们取许多树的平均值，这有助于减少噪声。

在随机森林中，唯一的区别是他们使用一种改进的树学习算法，该算法在学习过程中选择每个候选特征的随机子集，这被称为**“特征打包”**

我是时候把手弄脏了！！！

现在，在这篇博客中，我不会解释数据清理、探索性数据分析、特性缩放或标准标量化。你可以在我之前关于使用 python 的[决策树的博客中读到。](/analytics-vidhya/decision-tree-using-python-for-regression-ecf0ccd8884e)

我将从训练数据集开始，

为了训练数据集，我们将使用 Scikit-Learn 库中的 Ensemble 模块。

> [**集成方法**](https://scikit-learn.org/stable/modules/ensemble.html#ensemble) 的目标是结合用给定学习算法构建的几个基本估计量的预测，以提高单个估计量的可推广性/鲁棒性。

在系综模块中，我们将使用 RandomForestRegressor。
导入库:

```
# Importing the library
from sklearn.ensemble import RandomForestRegressor
```

现在，在我们去拟合数据集之前，让我们了解几个重要的参数。
1。 **n_estimators** : int，default value = 100
它估计森林中的树木数量

2.**判据** : {'mse '，' mae'}，default = 'mse'
mse =均方误差
mae =平均绝对误差
这有助于衡量拆分的质量。

3. **max_depth** : int，default = None
树的最大深度。

4. **random_state** : int，default = None
控制构建树时使用的样本自举的随机性。

让我们创建一个 RandomForesetRegressor 方法的对象，

```
# Object of the method
regressor = RandomForestRegressor(n_estimators = 200, max_depth = 4, random_state = 0)
```

既然我们的方法已经有了一个对象，那么是时候去适应训练和测试数据集了，

```
# Fitting the dataset
regressor.fit(X_train, y_train)
```

> 注意:X_train 和 y_train 是在 split 方法中初始化的。

我们将使用预测方法来预测我们的测试结果，

```
# Prediction
pred = regressor.predict(X_test)
```

现在我们将找出准确性和 F1 分数，为此我们将从 sklearn 导入矩阵模块

```
# importing the library
from sklearn.metrics import accuracy_score, f1_score
print('Accuracy of our dataset is:', accuracy_score(y_test, pred))
print('f1 score of our dataset is:', f1_score(y_test, pred))
```

Colab 笔记本:

[](https://colab.research.google.com/drive/1PCRAQfOkkPuNftiz6r-tPiCxJUy2tkbM?usp=sharing) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/drive/1PCRAQfOkkPuNftiz6r-tPiCxJUy2tkbM?usp=sharing) 

> 编码快乐！！