# 如何用 3 行代码将表格从 PDF 导入 CSV、TSV、from PDF 表格

> 原文：<https://medium.com/analytics-vidhya/how-to-import-tables-from-pdf-to-csv-tsv-json-pdftables-using-tabula-in-3-lines-of-code-f09220167a5f?source=collection_archive---------5----------------------->

![](img/cd76affecec9a2a3b80c0198ad136b34.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 拍摄的照片

在本教程中，我将教你如何转换和提取表格，从 pdf 格式到 CSV，TSV，JSON 格式，只有三行代码。

**第一步。**设置表格(单行代码)

**第二步。**导入白板

**第三步。**转换 pdf

# 介绍

tabula-py 是一个将 PDF 表格转换成 pandas 数据框架工具。tabula-py 是 [tabula-java](https://github.com/tabulapdf/tabula-java) 的一个包装器，在你的机器上需要 java。tabula-py 还可以将 PDF 格式的表格转换成 CSV、TSV、JSON 文件。

tabula-py 的 PDF 提取精度与 tabula-java 或 [tabula app](https://tabula.technology/) 相同；tabula 的 GUI 工具，所以如果你想了解 tabula-py 的性能，我强烈推荐你试试 tabula app。

> **白板适用于:**
> 
> 使用 Python 脚本实现自动化
> 
> 转换熊猫数据帧后的高级分析
> 
> 使用 Jupyter 笔记本或谷歌实验室进行临时分析

# **第一步**

tabula-py 需要 java 环境，所以让我们检查一下您机器上的 java 环境。

打开您的终端或 CMD，输入

```
java -version
```

确认 java 环境后，使用 pip 安装 tabula-py。再次在终端或 cmd 中运行

```
pip install -q tabula-py
```

# **第二步**

打开你最喜欢的 IDE 并编写下面的程序

```
import tabula#check your environment via tabula-py,which shows Python, Java #version, Java version, and your OS environment.
tabula.environment_info()#
pdf_path = "/path/to/you/pdf/file"# read pdf as CSV
tabula.convert_into(pdf_path, "test.csv", pages="all", output_format="csv", stream=True)
```

保存文件并运行它会将 pdf 转换为您需要的格式。

**范例笔记本**

[](https://colab.research.google.com/github/chezou/tabula-py/blob/master/examples/tabula_example.ipynb#scrollTo=9WPnL5QwluSo) [## 谷歌联合实验室

### 示例白板笔记本

colab.research.google.com](https://colab.research.google.com/github/chezou/tabula-py/blob/master/examples/tabula_example.ipynb#scrollTo=9WPnL5QwluSo) 

**白板**

[](https://github.com/chezou/tabula-py) [## 车走/白板

### tabula-py 是 tabula-java 的一个简单的 Python 包装器，可以读取 PDF 格式的表格。您可以从 PDF 中读取表格并…

github.com](https://github.com/chezou/tabula-py) 

# 自定义请求

你想问一个关于这篇文章的问题，或者为你自己或你的公司需要一个自动定制的提取器吗？随时联系。

# 结论:

权力越大，责任越大。现在你有了一个惊人的数据集。你会用它做什么？

你觉得这篇文章有用吗？给它鼓掌👏，分享给社区，有一些想法，还是我漏掉了什么？请在评论中与我分享📝。

# **连接**

作者是一名应用深度学习工程师，热衷于构建有意义的影响导向型产品。他还是 AWS 教育云大使，以及由 Google 开发者领导的前开发者学生俱乐部成员。他真的很喜欢与人交流。如果你喜欢他的工作，就在 LinkedIn 上跟他打个招呼。

[](https://mrasimzahid.github.io/) [## @MrAsimZahid |研究科学家

### 双 Kaggle 专家|前谷歌开发者 Studnet 俱乐部负责人兼 AWS 教育大使

mrasimzahid.github.io](https://mrasimzahid.github.io/) 

# 阅读更多信息:

[](/analytics-vidhya/how-to-scrape-tweets-and-create-dataset-using-twint-without-twitter-api-e5890c25d1c9) [## 如何在没有 Twitter API 的情况下使用 Twint 抓取推文并创建数据集

### Twint 是一个用 Python 编写的高级 Twitter 抓取工具，允许从 Twitter 个人资料中抓取推文…

medium.com](/analytics-vidhya/how-to-scrape-tweets-and-create-dataset-using-twint-without-twitter-api-e5890c25d1c9)