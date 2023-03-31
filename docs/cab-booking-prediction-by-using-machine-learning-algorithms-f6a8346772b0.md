# 基于机器学习算法的出租车预约预测

> 原文：<https://medium.com/analytics-vidhya/cab-booking-prediction-by-using-machine-learning-algorithms-f6a8346772b0?source=collection_archive---------5----------------------->

出租车预订通常是一个过程，在这个过程中，整个城市的租赁都是通过电话/应用程序自动完成的。使用该应用程序更简单，因为人们可以通过选择目的地来预订从一个地点到另一个地点的出租车，并且管理预订也很方便。对于出租车预订应用公司来说，了解出租车的供应和需求可以通过最大限度地减少等待时间来提高服务效率和改善用户体验。

让我们努力实现项目目标，将历史使用模式与开放数据源(如天气数据)结合起来，预测城市中的出租车预订需求。

让我们看看这些步骤。

1.  读取数据。

![](img/9934054fcc85f318a341e020fd82732b.png)

可以看到，数据中有 4 个文件

2.导入列车数据

![](img/5ba40639e3e30a67314f45d16938ed40.png)

列车数据中列的概述

3.导入测试数据

![](img/5bef49bfc424e584f50d645a6a6dfb08.png)

测试数据列概述。

4.在训练和测试中检查标签数据

![](img/0eae5cae69e394cd923e8030becb1874.png)

使用 Total_booking 列重命名 train_label 数据

![](img/161442221cc0827d4044fb8ef7c3475a.png)

带有 Total_booking 列的测试标签数据

5)将该数据附加到训练和测试中

![](img/35df0e8743d5aeab73f8a32f91552850.png)

可以看出，Total_booking 列现在从 train_label 添加到 train 数据集

![](img/9730304fec55230befff9d2a5027dca6.png)

在测试数据中，我们可以看到 Total_booking 有 NaN 值

6)日期时间列同时给出了日期和时间的信息，让我们将其分为日期和时间列

![](img/aac075ab97df6f0f9e8201703fbd29f1.png)

火车用

![](img/3d060cac5b73286c3079e3c9f9bdf393.png)

用于测试

7)数据分析和可视化

![](img/5365e49ad6b9451ac32489caf8976af2.png)![](img/d6dbd7208621b6d579697f286a6a3c18.png)![](img/f529803a8e16466c15eebbb8fabd3272.png)![](img/b00102ad2f76dd54f619091de3c0f462.png)![](img/569a5c406a1ca2db6f7fe6ccaf6412d6.png)

训练集中没有缺失值

![](img/397a0c73ae663da0a4dff2c01d2acb7c.png)

特征之间的相关性

![](img/fc31238021f9514e255845f3bfa15405.png)![](img/ede1cd0cf42be797262532d7b1a77232.png)![](img/dfca89381bd641073bcac5f6ef362670.png)![](img/62d6ea760bdd20961ecce1182163c582.png)

多重共线性混淆矩阵

![](img/90c9db9d759f89969584da151fdc9619.png)![](img/0b044e8d46361bf89b655a77a30e95c1.png)

Total_booking 列的测试数据中有空值，我们需要填充 NaN 值

![](img/97f2461b3a6b87ea1362c2e0e2905b5d.png)

从 Total_booking 中的其他值中选择的值

让我们对一些列使用标签编码器，因为它们是将它们转换成数值的对象

![](img/cd3265386faf976895030cab13eede9f.png)

通过使用标注转换为数字要素

![](img/cac1a0b9933a089c3fd3332b4b7738c6.png)

可以看出，这些值现在已经被编码

![](img/e5575112c7ec9fc384f36786c9c38c0a.png)

适用于模型的所有整数数据

![](img/e8d08b2334b3224503b670fe525b9ea7.png)

搜索缺少的值

![](img/5cefd535933fe7acbb5876ae620acb87.png)

找出特征之间的相关性

![](img/550055f4af58e31b123cd306dcc8f0dc.png)

天气图

![](img/df42401115a11528afc50f4f2106194d.png)

季节图

查找异常值(如果有)

![](img/a386e6aebbb18b131ccdde431371f384.png)

没有这样的离群值

使用 train_test_split 将模型拆分为训练和测试

![](img/272792fec3e5571750d2ff2bfa39ca30.png)

x，y 定义

![](img/e613e97a55b2af788aa5c973c3a4a663.png)

拆分数据

![](img/9e8acd553b1bcb7cf83fbf728510750f.png)

要测试的各种分类器

![](img/484300873a5913b01c9316a54d06a708.png)

来自分类器的分数

![](img/771916c3e102d3e16aca20b88efa4eff.png)

各种分类的精度

执行 GridSearchCV

![](img/05886825016dad6613e2652967d43212.png)

对于线性回归

![](img/a0d6b1b7ad929313e5328ec71dad0b77.png)

定义参数

![](img/607a7494b64ad32d957eb89db9b27b02.png)

GridSearchCV 列出的最佳参数

![](img/6ecbdc2dda5e9a5beced24109c0900b1.png)

可以看出，精确度略有提高

![](img/e66ee0eb53b7ca7812066f04179be7c7.png)

XGBRegressor 上的 GridSearchCV

**结论**

可以看出，精确度并不是很高，它的范围在 12%到 99%之间，最大平均值为 19%。

**参考文献:**

1.  如何使用 grid search CV-[https://www . datatechnotes . com/2019/09/how-to-use-gridsearchcv-in-python . html](https://www.datatechnotes.com/2019/09/how-to-use-gridsearchcv-in-python.html)
2.  什么是离群值-[https://www . aquare . la/en/what-are-outliers-and-how-treat-them-thes-in-data-analytics/](https://www.aquare.la/en/what-are-outliers-and-how-to-treat-them-in-data-analytics/)
3.  关于各种模型-[https://towards data science . com/predictive-modeling-picking-the-best-model-69ad 407 E1 ee 7](https://towardsdatascience.com/predictive-modeling-picking-the-best-model-69ad407e1ee7)
4.  Github commit 获取这里解释的完整代码——[https://github . com/saish 15/Machine-Learning/blob/master/Cab _ booking . ipynb](https://github.com/saish15/Machine-Learning/blob/master/Cab_Booking.ipynb)