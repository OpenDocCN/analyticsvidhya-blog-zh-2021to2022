# 避免管道泄漏

> 原文：<https://medium.com/analytics-vidhya/what-is-a-pipeline-anyways-e6a814d4888?source=collection_archive---------11----------------------->

管道到底是什么？

![](img/2ed3596fb5188da4593a40713de75961.png)

在数据科学领域，管道是以可重复的方式标准化和提取数据的过程，尽管它更多的是一个概念而不是一个公式。通常，管道是一个工作流。然而，具体来说，Sci-Kit Learn 的 Pipeline 模块是一种有效的方式，可以通过 Python 中的预处理自动执行[模型验证](https://towardsdatascience.com/validating-your-machine-learning-model-25b4c8643fb7)的良好实践。[如果你用 R 写代码，这可以用脱字符号包](https://machinelearningmastery.com/caret-r-package-for-applied-predictive-modeling/)来完成，尽管这里的代码示例可能对你没什么用。Pipeline 允许用户将 Sci-Kit Learn 的其他模块中的许多有价值的方法打包到一个工作流中，该工作流可以在许多数据集上实施和重复，并减少了可能导致不准确建模和训练的潜在错误，尤其是在处理大型数据集时。它们不仅可以简化您的工作，而且管道还可以用作防止数据泄漏的预防措施。

Sci-Kit Learn (也称为 sklearn)是 David Cournapeau 于 2007 年开始的一个项目，同年晚些时候加入该项目的 Matthieu Brucher 也参与了该项目。它于 2010 年 2 月 1 日首次正式发布，目前是一个社区开发的库，拥有国际用户和贡献者基础。Sklearn 被设计为在 SciPy 和 NumPy Python 库之上运行，但是对于 Pandas、Matplotlib 和 Plotly 也是高度功能化的。目前，它是预测分析的标准之一。

**什么是数据泄露，为什么说它不好？** “当在构建模型时使用预测时不可用的信息时，就会发生数据泄漏”，这“导致乐观的性能估计……因此，当模型用于实际上新的数据时，性能会更差，例如在生产期间”( [Sklearn 常见陷阱](https://scikit-learn.org/stable/common_pitfalls.html))。基本原则是永远不要将测试数据引入到应用于训练数据的特征提取和转换步骤中。作为一个推论，在特性选择和模型估计之前执行[训练/测试分割](https://machinelearningmastery.com/train-test-split-for-evaluating-machine-learning-algorithms/)或其他形式的模型验证可以确保不会发生数据泄漏，并且对您的测试数据没有影响(这听起来很乐观，但会导致不准确，在预测建模的情况下，我宁愿成为一个精确的怀疑者)。一些包含的链接进一步详细说明了模型验证，但其总体思想是将数据分为训练集和测试集-一个集是模型的基础，另一个集用于验证模型是否可以准确预测未知结果。

将测试数据引入训练数据转换会模糊算法准确预测未知数据的能力，因为它已经暴露在训练结构中，从而在自我强化循环中导致较差的[模型验证](https://towardsdatascience.com/validating-your-machine-learning-model-25b4c8643fb7)。当训练信息渗透到测试数据中，使结果偏向训练集时，就会出现这种循环。反过来，这使得预测未知信息有偏差，并使模型离准确预测越来越远。突然，你的模型失控了，你不得不重新开始。数据泄漏在小范围内会导致不准确的预测模型，在大范围内会使它们完全失效。在专业环境中，它可能会延迟部署并造成严重的安全漏洞。

因为这在概念上很难想象，下面是一个简单的野外数据泄漏的例子:

> “一个特征用于训练在预测时在生产中不可用的模型。例如，当入院后 24 小时内可能不会进行药物调节时，可以使用患者目前正在服用的口服药物数量来预测住院时间。”

[此医疗保健示例](https://healthcare.ai/data-leakage-in-healthcare-machine-learning/)通过使用在预测时不存在于原始集合中的信息来计算目标而发生泄漏，这是无效的，因为它应用尚不可访问的信息来确定未来结果。

**那么管道如何解决这个问题呢？** Pipeline 模块允许您打包特征选择、提取和估计，并有助于确保您只对训练数据进行操作——在上面的示例中，可以使用 Pipeline 来选择只在预测时间之前生成的数据。

管道也允许你只需要调用**。fit()** 和**。执行选择/标准化序列后预测()**一次，而不是每次单独重复该过程。除了简化数据挖掘过程之外，它还通过消除与每个独立实例的硬编码相关的潜在人为错误，提供了一个额外的防泄漏层。

**什么时候建造管道会有好处？**

**预处理
一旦你清理了你的数据，归一化可以强制它成为一个均值为零和单位方差的高斯(正态)分布，这是许多机器学习算法的必要条件(和假设)。如果不缩放数据，权重不同的特征可能会掩盖其他特征，并使算法无法准确预测测试子集的数据。除了缩放之外，预处理模块还有很多有用的工具，例如二值化和居中——访问 sklearn 的[文档页面。预处理](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing)了解更多特性。**

**为标准化构建管道:** 构建管道涉及到使用一列*(键，值)*对，其中*键*是估计器步骤的字符串名称，*值*是估计器对象。一个例子可能是这样的:

```
>>> from sklearn.pipeline import Pipeline>>> from sklearn.preprocessing import StandardScaler>>> from sklearn.linear_model import LinearRegression>>> estimators = [('scale', StandardScaler()), ('linreg', LinearRegression())] #contains a list of scaling and estimator objects>>> pipe = Pipeline(estimators) #constructs a pipeline using the objects>>> pipePipeline(steps=[(‘scale’ , StandardScaler()), (‘linreg’, LinearRegression())]) #the output is a pipeline object containing the above methods
```

实现这一点的一个简单方法(如果你真的不想给自己的对象命名)是调用 **make_pipeline()** :

```
>>> from sklearn.pipeline import make_pipeline>>> from sklearn.naive_bayes import MultinomialNB>>> from sklearn.preprocessing import Binarizer>>> make_pipeline(Binarizer(), MultinomialNB()) #initializes a Pipeline objectPipeline(steps=[(‘binarizer’, Binarizer()), (‘multinomialnb’, MultinomialNB())]) #the contents of the Pipeline object
```

**构建特征选择的流水线** :
对于选择满足一定统计标准的特征，[**sk learn . feature _ selection**](https://scikit-learn.org/stable/modules/feature_selection.html)工具可以作为数据预处理步骤的一部分，集成到流水线中。例如:

```
clf = Pipeline([(‘feature_selection’, SelectFromModel(LinearSVC(penalty=”l1"))),(‘classification’, RandomForestClassifier())])clf.fit(X, y)
```

这个 Sklearn 代码片段演示了如何初始化一个名为 **clf** 的管道对象，其中包含一个 **feature_selection** 工具和一个**算法分类**工具，它们将在所选特性上实现。然后，对数据 X 和 y 调用 **clf.fit()** 将使模型适合所选和修改的数据。

当谈到训练机器学习算法时，如果没有一套正确的工具来组织您的工作流程，这个过程可能会变得势不可挡、错综复杂和不精确。使用管道来构建信息处理和建模的方式有助于减少犯粗心错误的机会，并防止创建训练不足的模型。管理大型数据集已经是一项具有挑战性的任务，不存在许多可能遇到的障碍，因此了解在不牺牲您的理智的情况下避免它们的选项非常重要。

![](img/253bd2e39cec5e511a96b4151709f8ff.png)

*有关推理和预测分析的更多信息，请访问:*

[https://scikit-learn.org/](https://scikit-learn.org/)
[https://towardsdatascience.com](https://towardsdatascience.com/)T22[https://machinelearningmastery.com/](https://machinelearningmastery.com/)