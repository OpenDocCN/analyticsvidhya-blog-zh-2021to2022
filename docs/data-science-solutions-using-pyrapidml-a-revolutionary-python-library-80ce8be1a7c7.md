# 使用革命性 Python 库 PyRapidML 的数据科学解决方案

> 原文：<https://medium.com/analytics-vidhya/data-science-solutions-using-pyrapidml-a-revolutionary-python-library-80ce8be1a7c7?source=collection_archive---------4----------------------->

PyRapidML 是一个开源 Python 库，它不仅有助于自动化机器学习工作流，还有助于构建端到端的 ML 算法。PyRapidML 最容易使用，只需 4-5 行代码，就可以实现机器学习或任何其他数据科学解决方案。

> **PyRapidML 简介**

PyRapidML 是由 **Zain Ali** 于 2021 年 6 月开发的 python 库，由于其清晰的文档和易用性，几个月内就成为顶级机器学习库中的特色。

PyRapidML 是一个**开源 Python 库**，它不仅有助于自动化机器学习工作流，还有助于构建端到端的 ML 算法。PyRapidML 本质上是几个机器学习库和框架的一个 **Python 包装器**，比如 Pycaret、scikit-learn、XGBoost、LightGBM、CatBoost、spaCy、Optuna、Hyperopt、Ray 等等。

PyRapidML 是一个**低代码库**，这意味着编写基本的和更少的代码行，人们可以在他们的机器学习模型中实现高精度。没有必要写很多行代码，因为 PyRapidML 会比较所有可能的机器学习算法，只用一行代码就能解决你的问题。一旦 PyRapidML 给你最好的算法。您可以进一步优化模型(只需一行代码)来进一步优化模型。

PyRapidML 提供了一系列函数来处理数据科学问题，如分类、回归、NLP、聚类、异常检测等。

> 【PyRapidML 提供了什么？

*   PyRapidML 具有一系列功能，用于:
*   数据准备
*   探索性数据分析
*   数据挖掘
*   比较多种机器学习模型并给出最佳模型
*   模特培训
*   超参数调谐
*   自然语言处理
*   深度学习(正在建设中)

> 你需要这个库吗？

*   回答以下问题？
*   您是否厌倦了为您的数据科学问题编写大量代码？
*   您是否很难找出哪种算法执行得最好？
*   你是不是很难比较多个算法，看哪一个的准确率最好？
*   您在超参数调整中面临问题吗？
*   您想要简单的模型部署吗？
*   你有自动 ml 的梦想吗？
*   你是否面临探索性数据分析的问题？
*   您是否想要一个能够自动执行数据科学生命周期所有步骤的库？
*   您是否想要一个可以进行探索性数据分析、特征工程、特征选择、比较多种机器学习算法、超参数调整、模型部署的库？

> **观众**

*   数据科学家
*   数据科学学生
*   公民数据科学家
*   数据分析师
*   希望构建端到端数据科学解决方案的数据专业人士

> **版本**

PyRapidML 1.0.13 现已推出。

> **文档和 Github 链接**

PyRapidML 的文档可在以下位置找到:

*   文件:【https://pyrapidml.readthedocs.io/en/latest/ 
*   Github 链接:[https://github.com/Zainali5/PyRapidML](https://github.com/Zainali5/PyRapidML)
*   Pypi 链接:[https://pypi.org/project/PyRapidML/1.0.13/](https://pypi.org/project/PyRapidML/1.0.13/)

> **如何安装 PyRapidML**

pip 安装 PyRapidML

将 PyRapidML 更新到新版本

pip 安装 PyRapidML —升级

> **一些重要的 PyRapidML 函数**

让我们快速看一下一些重要的 PyRapidML 函数:

*   *comparisng _ models 函数* —使用默认超参数训练模型库中的所有模型，并使用交叉验证评估性能指标。用于分类的指标:准确性、AUC、召回率、精确度、F1、Kappa、MCC。用于回归的指标:MAE，MSE，RMSE，R2，RMSLE，MAPE。
*   *creating_model* 函数-使用默认超参数训练模型，并使用交叉验证评估性能指标。
*   *tuning_model* 函数-调整作为估计器传递的模型的超参数。它使用随机网格搜索和预定义的完全可定制的调谐网格。
*   *ensemble_model* 函数——您传递一个经过训练的模型对象，该函数返回一个表，其中包含常用评估指标的 k 倍交叉验证分数。
*   *预测 _ 模型* —用于推理/预测。
*   *plot_model* —用于评估经过训练的机器学习模型的性能。
*   效用函数——在使用 PyRapidML 管理机器学习实验时，许多效用函数都很有用。

查看下面的文档以获得 PyRapidML 函数的完整列表

 [## PyRapid 主页！- PyRapidML 1.0.13 文档

### PyRapidML 是一个开源的 Python 库，它不仅有助于自动化机器学习工作流，而且有助于…

pyrapidml.readthedocs.io](https://pyrapidml.readthedocs.io/en/latest/index.html) 

> 【PyRapidML 入门

PyRapidML 仍然是一个新的库，教程正在构建中。

有些教程已经完成，可以在以下网址找到:[https://github . com/zainali 5/PyRapidML/tree/v 1 . 0 . 13/Tutorials](https://github.com/Zainali5/PyRapidML/tree/v1.0.13/Tutorials)

教程包括的主题有:分类、回归、NLP、聚类、异常检测和关联规则挖掘。

> **结论**

PyRapidML 为使用 Python 的数据科学家和希望使用工具来提高生产力的人扮演了同样的角色，这些人不使用成熟的 AutoML 平台，如 H2O.ai 或 Data Robot。

如果你在概念上很弱，并且想要为你的问题得到一个数据科学的解决方案，跳上 PyRapidML，看看奇迹。

如果您想编写更少的代码行并获得最大的输出，PyRapidML 是一个不错的选择。

如果您想要比较多个机器学习模型，调整 ML 模型，自动执行数据科学生命周期的所有步骤，轻松部署模型，自动数据挖掘等。那么使用 PyRapidML 是显而易见的。