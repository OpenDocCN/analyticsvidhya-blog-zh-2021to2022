# 文章发布者预测| Softech 2021 AI 竞赛

> 原文：<https://medium.com/analytics-vidhya/article-publisher-prediction-softech-2021-ai-competition-6719d8be2542?source=collection_archive---------21----------------------->

来自 Softech 2021 AI 竞赛的几点收获和思考。

![](img/5b6c91b240a190ceb176c4d6cfdeebac.png)

[附身摄影](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

 [## Softec'21 人工智能竞赛

### 人工智能竞赛

人工智能 Competitionwww.kaggle.com](https://www.kaggle.com/c/softec21-artificial-intelligence-competition/code) [](https://www.kaggle.com/asimzahid/analysis-publisher-prediction-score-0-88357) [## 分析 _ 发布者 _ 预测得分 0.88357

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自 Softec'21 人工智能的数据…

www.kaggle.com](https://www.kaggle.com/asimzahid/analysis-publisher-prediction-score-0-88357) 

# 任务:

预测给定数据的发布者。

# 背景:

针对标签可用的 HTML 页面的 Blob(来源:发布者名称)。

# 数据分析:

只有两列:Source，Html

标签(源)类相当不平衡。一个极端是 37 篇文章，另一个极端是每个出版商只有 2 篇文章。
笔记本中可用颜色编码分析

Html 列有整个抓取的 html 页面的 blob，包括所有的代码和东西。

## 初级思维过程:

在这里，初级水平可能会做的是提取文章文本，并通过一些神经网络像 LSTM，RNN 等。试着得到一些结果。这完全没问题，也是获得结果的一种方式。但是在生产中没有优化，并且在计算能力、存储器等方面花费更多。

## 经历的思维过程:

我们有大量可用的 html。特色工程吧。提取文章文本，并从中提取元数据['标题'，'作者'，' url '，'主机名'，'描述'，'站点名'，'日期'，'类别'，'标签'，'指纹'，' id '，'许可证']。

Regex 不错，如果你有时间，最好使用一些数据提取库来加快进程。
经过 meta _ data 分析，有用的数据点是['author '，' url '，' hostname '，' sitename']。
按不同组合分组，用源标签观察。
观察大部分标签匹配，但不完全匹配(稍后会讲到这一点)。

按作者分组观察，作者只为一个出版商写作。一个很好的功能。

现在回到我们的主机名，网站名称功能。它们只有 148 个 Nan 值。而 sitename 更接近我们的标签。删除 Nan 值…

这里有两种方法，

1.  创建 key=sitename，value=Source 的键、值对字典。
2.  将“来源”列保存到列表中。

所有上述步骤都已在列车组上执行。

**线程合并>**

移动到测试设备。
提取元数据，获取“sitename”列并提交。
***F1 得分= 0.45***
注意我们还没有使用任何 ML 或 DL 方法。

在实现方面还是前面的两个方法。

1.  我们有一个可用的字典，只需使用我们之前创建的字典进行匹配和替换。
2.  使用 fuzzy-wuzzy 根据比率匹配字符串。并输入我们之前创建的源列表。

***F1 得分= 0.88***

观察这是一个实质性的改进。
尽管如此，我们还没有使用 ML。

# 未来工作:

我们之前也观察过作者特性。我们可以创建一个键=作者，值=源的字典。并基于此应用替换。我相信你可以轻松跨过 0.92 的门槛。

然后也许我们会走向 TF-IDF，随机森林，和其他 ML 算法。

# 离别的思绪:

1.  先尝试用方便的基于规则的方法解决问题，不要跳到 NNs。
2.  特征工程很重要。
3.  在生产中，ML 模型是昂贵的。它们需要时间、内存、计算能力和金钱。所以，如果可能的话，结合使用这些方法，会很有帮助。
    我们(团队)在 ***shopee 竞赛*** 中使用相同的方法放置了***131 号/2442*** 。
4.  学习语言、数据结构和最佳实践，以编写高效、优化的代码。

希望这能有很大帮助。祈祷时记得。亲切问候，阿西姆·扎希德

# 联系/雇用我:

[](https://linktr.ee/mrasimzahid) [## @MrAsimZahid

### 链接树。让你的链接做得更多。

linktr.ee](https://linktr.ee/mrasimzahid)