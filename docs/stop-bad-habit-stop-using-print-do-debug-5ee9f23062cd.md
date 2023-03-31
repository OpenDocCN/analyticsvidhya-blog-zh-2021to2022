# 停止坏习惯:停止使用“打印”做调试

> 原文：<https://medium.com/analytics-vidhya/stop-bad-habit-stop-using-print-do-debug-5ee9f23062cd?source=collection_archive---------17----------------------->

![](img/03443e30839fa3662a3b61c110febf5f.png)

照片由[马南查布拉](https://unsplash.com/@manancfc23?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bad-habits?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

像许多数据科学家和数据分析师一样,`print`命令在数据调试中非常常见，但这是一个坏习惯，原因有很多，我们将在这里解释。

*   使用`print`的第一个限制是，当你想把你的脚本转移到生产中时，你也要转移所有的`prints`，有时它们是不需要的。所有这些`prints`只会在你的脚本上制造噪音，让它不那么…