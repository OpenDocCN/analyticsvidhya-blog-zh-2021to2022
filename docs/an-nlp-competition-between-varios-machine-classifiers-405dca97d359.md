# 各种机器学习分类器之间的 NLP 竞赛！

> 原文：<https://medium.com/analytics-vidhya/an-nlp-competition-between-varios-machine-classifiers-405dca97d359?source=collection_archive---------11----------------------->

逻辑斯蒂、正则化线性、SVM、安、KNN、随机森林、LGBM 和朴素贝叶斯分类器，哪一个在分类报纸文章方面做得最好？

![](img/3971d2e86a04b8fd955c5d1fe774d2ae.png)

Jonathan Chng 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

scikit-learn 和 light GBM 中的所有这些机器学习分类器都可以有效地处理 scipy.sparse 矩阵，该矩阵是[tfidf 矢量器](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer)的输出。TfidfVectorizer 是一个 NLP 特征提取器…