# 简单英语中的 XML

> 原文：<https://medium.com/analytics-vidhya/xml-in-plain-english-639fbea36e36?source=collection_archive---------29----------------------->

*XML 仍然被广泛使用，所以让我们学习基础知识！*

![](img/daee95b1511d66296a5e47b23e81ae77.png)

照片由 [Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/html?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 什么是 XML？

XML 代表 E **x** 可扩展 **M** arkup **L** 语言。它最常用于存储数据或用 HTML 构建页面，在某些方面非常相似。

XML 本身不能做任何事情。这只是一种存储数据的方式。程序可以使用 XML 文件来存储或检索数据，或者一些程序使用它来定义布局。

## 我为什么要使用 XML

XML 作为一种数据格式被广泛使用，并且很容易学习编写。例如，XML 作为一种数据格式可以处理比 CSV 更高级的数据。如果数据更有深度，这可能会很有用。

在安卓开发中，XML 也被用来设计应用程序的页面。它与 web 开发中使用的 HTML 非常相似，如果需要，可以通过某些网站轻松解析为另一种数据格式。

# 基本语法

让我们从一个非常基本的 XML 文档开始。我们将创建一个非常简单的个人数据文件。

如果您已经熟悉了 XML 或 HTML，那么您已经知道了这种格式。我们用所谓的*标签*创建数据。像`<person></person>`。标签必须有一个开始标签和一个结束标签，中间有数据。

在 HTML 中，我们可以关闭像`<br />`这样的开始标记，但是在常规的 XML 中这是不允许的。

但是，如果我们需要更复杂的数据呢？让我们看看。

# 嵌套

您可以在标签中插入标签，以创建更复杂的数据结构。让我们看看其中一个。

这个人现在有一个`jobs`标签，中间有两个`job`标签。

另请注意顶部的`<?xml?>`标签。这告诉任何我们正在使用 XML 的程序或网页。你可以看到同样的 HTML，你会写`DOCTYPE html`。这始终是相同的，您可以复制并粘贴它。

# 属性

让我们看看下面的 XML 文件。

我们使用*属性*来代替性别标签。这些在 XML 和 HTML 中非常常见。在 HTML 中，我们会为`style`添加标签。

属性必须用单引号或双引号引起来。

没有关于何时使用属性以及何时使用嵌套元素的规则。在 HTML 中，它们非常有用，但是在 XML 中，最好避免使用它们。

我希望这有所帮助。

祝你今天开心！玩的开心！