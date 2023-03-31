# 机器学习中用较少的代码测试更多的超参数

> 原文：<https://medium.com/analytics-vidhya/testing-more-hyperparameters-with-less-code-in-machine-learning-a476074f786?source=collection_archive---------11----------------------->

![](img/df5c4eed09bd64cd8686c5702a577182.png)

假设您已经完成了数据预处理、特征工程等所有工作，并准备好构建和训练您的模型，让我们以 K 最近邻算法为例:

```
model = KNeighborsClassifier(n_neighbors=10, algorithm="ball_tree")
model.fit(x_train, y_train)
model.score(x_test, y_test)
```