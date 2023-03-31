# 布局数据的 k 近邻(KNN)算法

> 原文：<https://medium.com/analytics-vidhya/k-nearest-neighbors-knn-algorithm-for-placements-data-5c0c1f81de4?source=collection_archive---------5----------------------->

![](img/f919f2465c0f19d807051bb2a2af98dd.png)

k 最近邻(KNN)

# 介绍

k-最近邻(KNN)算法是一种监督最大似然算法，可用于分类以及回归预测问题。但在工业上主要用于分类预测问题。以下两个特性可以很好地定义 KNN

*   **懒惰学习算法**—KNN 是一种懒惰学习算法，因为它没有专门的训练阶段，并且在分类时使用所有数据进行训练。
*   **非参数学习算法**—KNN 也是一种非参数学习算法，因为它没有对底层数据做任何假设。

在这些文章中，我们将创建一个关于校园安置数据的 KNN 分类器模型…

让我们导入所有的库并读取数据。

![](img/de62cbd2de83ff4df625e259bc9f244e.png)![](img/cfffe4545ea3f434442249bc8b78d93a.png)![](img/10fffd31a8965d57d3a9075adafca217.png)

现在让我们检查所有列中的所有唯一值…

![](img/9c8d5be3b43d80f77d9aeefc77c7b31a.png)

它显示所有唯一的值……

![](img/78ac28aaf42602f862963291dc792a94.png)![](img/4def76a9445a03bc31c77bada8918024.png)

我们可以看到所有列都有相同数量的缺失值…..

![](img/ed725573aef5965abe2597a533d5f64f.png)![](img/8385422df1dc5b123838ec6c00a08622.png)

以上图表显示了候选人的性别、SSC 委员会、HSC 委员会和 HSC 流的数量。

![](img/f3eb5b2634a86234e6c3821857ecce66.png)![](img/25029b7828aa006e5f722897f88accdc.png)

上面的图表显示了有工作经验的候选人的数量，他们选择的专业，以及是否被录用的状态。

![](img/ea38e654a917ab07a148e38e168c13e0.png)![](img/4ab1542fb5fb35ee63497121f25d2015.png)![](img/ddca4dcc06f993b21486baee36c61a33.png)![](img/448cbb703a9abd8ca74f79f033cc98ba.png)![](img/e89c2376adb922548e3941b1916d32bc.png)

从上面的相关矩阵可以看出，职位安排和薪酬与三个因素有很强的相关性

1)degree_ p 即学位%，2)HSC _ p 即 HSC %，3)SSC _ p 即 SSC %分数。

![](img/e8b38fbc20bee507baa0031d9583454b.png)![](img/cfe56fb4ad4270f321546761f95b8214.png)

上图显示了学位百分比与性别地位的关系，无论是否被录用。

![](img/90919dc368ec167ff414ffbf87322ee1.png)![](img/8a57e0e847d1cd7bb1f6b0574ee6de5e.png)

上图显示了学位百分比、HSC 百分比、SSC 百分比、MBA 百分比得分的距离图。

![](img/48a3ed159c73bf4a789673f344bfd102.png)![](img/6ad0a7fbf3bd0abeca0931a941849255.png)

上面显示了安置后的工资直方图。

![](img/00fc2c4777bc0ccacac1dfcb57a9c672.png)![](img/8500e4335fda1fd60be86947300779a7.png)

上面的图表显示了不同参数之间的关系，例如 HSC 分数的百分比与学位百分比，与职位或性别、他们选择的专业之间的关系。

![](img/1b928c0f9e2ca83a66a2d0f0fca388fa.png)![](img/5cfda33e6fab072f122a1ee5524aa445.png)

**KNN 分类器**

列车数据分割:

将训练数据拆分成 X_ train，X_ test，y_ train，y_ test。

![](img/9163f5a4602fe2b6486a250c8790af3a.png)![](img/328c4cf17bb95bab37b15a090fe3e47e.png)

**型号**

现在我们可以建立我们的 KNN 模型了。

![](img/8d98d1755d25695a9b04db44e98da22b.png)![](img/7bb05e43a0870fd3572779b9b3fd2e30.png)

预测未知:

![](img/d4984ebea7854826c863cb2a9e39e6a4.png)![](img/8693b54443df229ea2f534ee06d7c686.png)

比较实际值和预测值:

![](img/631bc1eca3abaae45d4b5b5c0785cdea.png)

显示包含实际值和预测值的数据框

最后计算准确度分数、分类报告和混淆矩阵……

![](img/64cf67d97061a76fddc30ba7bb60efc6.png)![](img/ba63246bf27e8379b548673b06ac615c.png)

**优点:**

1.  KNN-是一种**非参数算法。**
2.  **K-NN 是一种基于*实例的学习算法。***

*****缺点:*****

***除了一些优点之外，这种易于实现的算法也有一些缺点。一些定义如下:***

1.  ***K-NN 是一种 ***懒学习*** 算法。***
2.  **对于 ***高维问题*** 表现不佳。所以维度的诅咒也在这里。**
3.  **这个算法是 ***比较慢的*** 去求值，需要存储整个训练数据。因此，它也可能是 ***计算量大的*** 。**