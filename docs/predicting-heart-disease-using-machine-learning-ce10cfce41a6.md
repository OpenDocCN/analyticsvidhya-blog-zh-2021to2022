# 使用机器学习预测心脏病

> 原文：<https://medium.com/analytics-vidhya/predicting-heart-disease-using-machine-learning-ce10cfce41a6?source=collection_archive---------3----------------------->

本文讨论了各种基于 Python 的 ML 和数据科学库，以构建一个能够根据医疗属性预测某人是否患有心脏病的机器学习模型。

## 我们将采取以下方法:

## 1.问题定义

## 2.检索数据

## 3.了解功能

## 4.数据准备及其工具

## 5.探索性数据分析

## 6.系统模型化

## 7.模型评估

# 1.**问题定义**

给定病人的临床参数，我们能预测病人是否有心脏病吗？

我们的目标是达到 85%以上的模型精度。如果模型得分高于 85%，我们将选择该模型。

# 2.**检索数据**

原始数据来自 UCI 机器学习知识库中的 Cleveland 数据，以及 Kaggle 上的一个版本。

[](https://www.kaggle.com/ronitf/heart-disease-uci?select=heart.csv) [## 心脏病 UCI

### https://archive.ics.uci.edu/ml/datasets/Heart+Disease

www.kaggle.com](https://www.kaggle.com/ronitf/heart-disease-uci?select=heart.csv) 

# 3.**了解特性**

1.**年龄:**显示个人的年龄。

2.**性别:**使用以下格式显示个人的性别:

1 =男性

0 =女性

3. **cp(胸痛类型):**使用以下格式显示个人经历的胸痛类型:

0 =典型心绞痛

1 =非典型心绞痛

2=非心绞痛性疼痛

3 =渐近

4. **trestbps(静息血压):**显示个人的静息血压值，单位为 mmHg(单位)

5. **chol(血清胆固醇):**显示血清胆固醇，单位为毫克/分升(单位)

6. **fbs(空腹血糖):**将个人的空腹血糖值与 120mg/dl 进行比较。

如果空腹血糖> 120mg/dl，则:1(真),否则:0(假)

7. **restecg(静息心电图):**显示静息心电图结果 0 =正常

1 =波异常

2 =左心室机能亢进

8. **thalach(达到的最大心率):**显示个人达到的最大心率。

9. **exang(运动诱发的心绞痛):**

1 =是

0 =否

10. **oldpeak(运动相对于静息诱发的 ST 段压低):**显示整数或浮点数的值。

11.**斜率(峰值运动 ST 段):**

0 =上升

1 =平坦

2 =下坡

12. **ca(透视着色的主要血管数(0-3):**显示整数或浮点数。

13.**地中海贫血:**显示地中海贫血(一种遗传性血液疾病，导致您的身体血红蛋白低于正常水平) :

0 =正常

1 =固定缺陷

2 =可逆缺陷

14.**目标(心脏病诊断):**显示个人是否患有心脏病:

0 =缺席

1 =存在

# 4.**数据准备及其工具**

**熊猫& Numpy** 进行数据分析和处理

用于数据可视化的 Matplotlib 和 Seaborn

**sci kit-了解**建模和评估

![](img/8eb9e1f4cedcb69b3bda8b708adce1f5.png)

# 5.探索性数据分析

![](img/dd9e75f8e898edb7287e866dc91d4969.png)![](img/c3e4c327e733de393bc03027839cbfbc.png)![](img/1d3356fd8d826e4135573e9ea1fcd3ff.png)![](img/7a32d93a53f3489c4c6e2faffa437b3f.png)![](img/b449902ada567c11cd75f247af2cdd85.png)![](img/73945aa9b24a37b39b604ce25f64e6a4.png)![](img/a2bf4d88ff18a8af15b7e655b9d3738b.png)![](img/7782c67f1b8514fd66b5a8fdb5fb18f3.png)![](img/1c56c537ff2e3ccc33992f81ec49e033.png)![](img/efc92d7d831905ec07ffe83a4337b3c5.png)![](img/c58aec1e169e903fadae3866248f743b.png)![](img/5ad9f784b6ac0c421087ca111aa0cf9e.png)![](img/27df4b7f18add1a6c792939a423082f9.png)![](img/535dd7675a21e5ab19a588f7bce79300.png)![](img/d75e262abe13c0a43d322f88cb1bb201.png)![](img/07dabd8159b9a7e69654e835cc6cb9cf.png)![](img/01a103be8f69cf0cb9024c95e7e65391.png)![](img/65a87170614a4a84c56ada7f1d714cda.png)![](img/7da133584f7960732d60cb0d3c41e11d.png)![](img/5620bfba0c59af685a3a20698f07c1cb.png)![](img/04417abea6b45520948c4df308300257.png)![](img/e12fbb0bde1c0993a8fb0a017454aa7e.png)![](img/8684c45b6db5a7658121dff1ce7667ce.png)

# 6.系统模型化

我们必须对模型进行实验，尝试 3 种不同的模型，获得结果，并在以后进行比较。

![](img/6b54da3bbd46a430b5a42c9228476bea.png)

现在我们已经将数据分为训练集和测试集，是时候建立一个机器学习模型了。

我们将在训练集上训练它(找到模式)，并在测试集上测试它(使用模式)。

**我们将尝试 3 种不同的机器学习模型:**

**1。逻辑回归**

**2。k-最近邻分类器**

**3。随机森林分类器**

![](img/87048133ea0970678028a10b234b0315.png)![](img/fc56a747b4b50094523c11c037c89168.png)![](img/40a4675d2bc2d814264db3603b0f19c9.png)![](img/ede241cd6d7c4e3f179669a8c69ca66b.png)![](img/1dbf6e9726176989e9ae8c1b914b5db2.png)![](img/9e6535f6ea7b0c1cf109fd2d4c2ded32.png)![](img/150ccefd5eb3649c01ab78b738890fea.png)![](img/c7b51fdfb605a99623413a0b3b8dbc51.png)![](img/f473a08cc3bf3e4b9059ba6bc88334f3.png)![](img/4712e5a046aada8d2e74ad9f9137493d.png)![](img/4a4db7e0791a31240744af201c7d12a9.png)

现在我们有了一个基线模型…而且我们知道模型的第一次预测并不总是最终的。

我们下一步应该做些什么？

让我们看看下面的内容:

*   超参数调谐
*   特征重要性
*   混淆矩阵
*   交叉验证
*   精确
*   回忆
*   f1-分数
*   分类报告
*   受试者工作特征曲线
*   曲线下面积

![](img/30d79531cfe15f5e23de582997243b51.png)![](img/52ef20b8ad9f9ef0a5862dcfa083ff34.png)![](img/61fc2ebdc1864f2e11f2dcb993009a1f.png)![](img/a3f45336aa299877b9ba9bd04beb7d47.png)![](img/2d992560776d3dbfd7d397994c83880d.png)![](img/5276e833a8bc46d75e0c05b5a8f667ab.png)![](img/96f0dc1d125c18715a10698a508ea8ed.png)![](img/ebf767577e0079f8b47e1b386e7e2aa1.png)![](img/a3cad20c593a1449269f08bc06dd2a90.png)

**我们获得了大约 89%的准确率。因此，我们将选择模型。**

# 评估我们经过调整的机器学习模型分类器，超越准确性

*   ROC 曲线和 AUC 评分
*   混淆矩阵
*   分类报告
*   精确
*   召回
*   f1-分数

还有交叉验证

为了进行比较和评估我们的训练模型，首先，我们需要进行预测。

![](img/df66fef2b08dd5ca526e4d5b277bfe4e.png)

1.  **ROC 曲线和 AUC 评分**

![](img/694997ee8e09549c0a3644bbd24ba38a.png)

2.**混淆矩阵**

![](img/1639324d7ba20ed0a765eebcd25e9192.png)

3.**分类报告**

![](img/6d6cb288186f8f8c72da7086012bba06.png)

4.**交叉验证**

*   准确(性)

![](img/a9b146c90f554f7b6ce860f817257e0e.png)

*   精确

![](img/22b0df182c28a7d0e1f638987fb8756b.png)

*   f1-分数

![](img/fb2d4579a5c452d441e104d2cee17f28.png)

交叉验证分类指标的比较

![](img/33b69e41e1f2c84931fba5c7259e4484.png)

## 特征重要性

特性重要性是另一种提问方式，“哪些特性对模型的结果贡献最大，它们是如何贡献的？”

![](img/872cfb06fd9f45e5d6ceab14aa3bd834.png)![](img/6ec261f24c2feaff9c4be22fc29d77d6.png)![](img/e7243be5c4a3f8a879f397d82ffe9954.png)

# 结论

心脏病是当今社会的主要问题之一。很难根据风险因素人工确定患心脏病的几率。然而，使用机器学习，我们将很快预测这个人是否患有心脏病。由于心脏病分类的快速和准确，医生将为患者提供适当的治疗并挽救他们的生命。

快乐学习:)

Github 链接:-[https://github . com/pulkitkhandelwal 29/Heart-Disease-class ification](https://github.com/pulkitkhandelwal29/Heart-Disease-Classification)