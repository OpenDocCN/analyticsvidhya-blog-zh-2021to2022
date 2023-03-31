# 癌症行为风险分类的逻辑回归

> 原文：<https://medium.com/analytics-vidhya/logistic-regression-for-classifying-cancer-behavioral-risk-c9cfdadc800e?source=collection_archive---------17----------------------->

# 介绍

线性模型是在实践中广泛使用的一类模型，在过去的几十年里得到了广泛的研究，其根源可以追溯到一百多年前。线性模型使用输入要素的线性函数进行预测。

线性模型也广泛用于分类。最常用的一种线性分类模型是逻辑回归。

逻辑回归是机器学习从统计学领域借用的另一种技术。这是二元分类问题(具有两个类值的问题)的常用方法。

在本文中，我们将尝试使用逻辑回归对宫颈癌行为风险进行分类，这是一个二元分类问题(具有两个类值的问题)

**免责声明**

作者不是卫生部门的专家，所以本文不应作为主要参考资料。

# 导入所需的库

首先，我们首先导入我们需要的库

> *从 matplotlib 导入熊猫作为 pd
> 导入 pyplot 作为 plt
> %matplotlib inline
> 导入 seaborn 作为 sns*
> 
> *从 sklearn.model_selection 导入 train_test_split
> 从 sklearn.model_selection 导入 GridSearchCV
> 从 sklearn.linear_model 导入 LogisticRegression*

# 关于数据集

我们下载数据集，获取数据信息

> URL = '[https://archive . ics . UCI . edu/ml/machine-learning-databases/00537/sobar-72 . CSV '](https://archive.ics.uci.edu/ml/machine-learning-databases/00537/sobar-72.csv')
> 
> data = pd.read_csv(url)
> 
> 数据.形状
> 
> data.info()
> 
> data . is null()values . any()
> 
> SNS . Count Plot(x = data . ca _ 子宫颈)
> plt.title('标签的计数图')
> plt.show()
> 
> label _ 0 = len(data[data . ca _ 宫颈= = 0])
> label _ 1 = len(data[data . ca _ 宫颈== 1])
> 
> 总计=标签 _1 +标签 _0
> 
> PC _ of _ 0 = label _ 0 * 100/total
> PC _ of _ 1 = label _ 1 * 100/total
> 
> 打印('无癌百分比:{:.0f} '。format(pc_of_0))
> 打印('无癌百分比:{:.0f} '。格式(第 1 页，共 1 页)

根据上面的信息，我们知道数据集有 72 个样本，包含 20 个特征。

一个特征(ca _ 子宫颈)是一个值为 1 的标签，表示样本有宫颈癌，值为 0 表示样本没有宫颈癌。从数据来看，大约 71%的样本没有患宫颈癌，而大约 29%的样本患有宫颈癌

同样基于上述信息，数据集没有空值，因此我们可以继续下一步

# 分割数据集

将数据分为训练集和测试集的目的是为了使以后获得的模型在分类数据时具有良好的泛化能力。分类模型在训练集中很好地执行数据分类，但在分类新的和不存在的数据时表现很差，这种情况并不罕见。

> 数据集=数据.值
> 
> X =数据集[:，:-1]
> y =数据集[:，-1]
> 
> X_train，X_test，y_train，y_test = train_test_split(X，y，random_state=42)
> 
> print('训练集的形状是'，(X_train.shape，y_train.shape))
> print('测试集的形状是'，(X_test.shape，y_test.shape))

# 使用逻辑回归分类

逻辑回归是统计学中的一种数据分析技术，旨在确定几个变量之间的关系，其中响应变量是分类的，包括名义变量和顺序变量，解释变量是分类的或连续的。二元逻辑回归是一种数学模型方法，用于分析几个因素和二元变量之间的关系。在逻辑回归中，如果响应变量包含两个类别，例如 Y = 1 表示获得的结果“成功”, Y = 0 表示获得的结果“失败”,则逻辑回归使用二元逻辑回归。

在本文中，我们将使用默认参数值和使用网格搜索指定的参数值进行逻辑回归。然而，具体到规划求解，我们将使用 liblinear。这是因为我们将使用的分类是二元分类，数据集属于小数据集类别

逻辑回归的主要参数是正则化，称为 C。C 的小值表示简单模型。因此，调整这些参数尤其重要。

你要做的另一个决定是你想使用 L1 正则化还是 L2 正则化。如果你假设你的特性中只有少数是真正重要的，你应该使用 L1。否则，您应该默认为 L2。如果模型的可解释性很重要，L1 也是有用的。由于 L1 将只使用少数几个特征，这就更容易解释哪些特征对模型是重要的，以及这些特征的作用是什么。

所以，我们将只在第二种方法中设置 2 个参数，即 C 和 penalty

我们可以用一个新的缺省的线性和参数解算器进行逻辑回归。

> log reg = LogisticRegression(solver = ' liblinear ')
> 
> logreg.fit(X_train，y_train)
> 
> 打印('分数训练集:{:.3f} '。格式(logreg.score(X_train，y _ train))
> 打印('分数测试集:{:.3f} '。格式(logreg.score(X_test，y_test)))

在默认值下，逻辑回归在训练集上提供 100%的准确度，在测试集上提供 83.3%的准确度。这说明我们过度拟合了。

接下来，我们设置 C 和惩罚参数。我们使用交叉验证值为 5 的网格搜索来确定 C 参数和最佳惩罚

> param_grid = {'C':[0.001，0.01，0.1，1，10，100]，' penalty':['l1 '，' l2']}
> 
> grid _ search = GridSearchCV(log reg，param_grid，cv=5)
> 
> grid_search.fit(X_train，y_train)
> 
> 打印('最佳参数:{} '。format(grid _ search . Best _ params _)
> print('最佳交叉验证分数:{:.3f} '。格式(grid _ search . best _ score _)
> 打印('测试集分数:{:.3f} '。格式(grid_search.score(X_test，y_test)))

验证集上最好的分数是 91%，比以前低，可能是因为我们使用了更少的数据来训练模型(X_train 现在变小了，因为我们对数据集进行了两次拆分)。然而，测试集上的分数——实际上告诉我们概括得有多好的分数——变成了 89%,比以前更好。

# 结论

逻辑回归可用于估计上述数据集中的癌症风险。

使用默认值(使用 liblinear 解算器)我们得到 83.3%的分数准确度。同时，如果我们使用交叉验证值为 5 的网格搜索找到的参数(使用 liblinear 求解器)，我们会得到 88.9%的准确率。

# 参考

1.  [https://archive . ics . UCI . edu/ml/datasets/宫颈癌+癌症+行为+风险](https://archive.ics.uci.edu/ml/datasets/Cervical+Cancer+Behavior+Risk)
2.  安德烈亚斯·穆勒和萨拉·圭多。Python 机器学习简介
3.  [https://machine learning mastery . com/logistic-regression-for-machine-learning/](https://machinelearningmastery.com/logistic-regression-for-machine-learning/)

完整的代码你可以访问[这里](https://github.com/dhiboen/Project/blob/main/Untitled3.ipynb)