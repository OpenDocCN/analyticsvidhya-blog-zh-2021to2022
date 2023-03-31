# 使用文本相似度的 Vlookup

> 原文：<https://medium.com/analytics-vidhya/vlookup-using-text-similarity-bd06adb9e14b?source=collection_archive---------4----------------------->

## [深入分析。](/@benabrin/excel-approximate-match-fuzzy-match-up-7cf5abda2764?source=friends_link&sk=d111cd27d29168f4ec9844d979556641)

## 强大的 vlookup 工具基于 fuzzylookup 返回字符串相似性的匹配。

[点击此处下载使用文本相似性工具的 vlookup】](https://workspace.google.com/marketplace/app/xlookup_for_sheets/319859325063)

VLOOKUP 函数用于搜索值表，并从表中返回相应的项。VLOOKUP 函数在表的第一列中搜索与查找值匹配的值。

VLOOKUP 返回的值是同一行中的值，但在第二个参数指定的列中。

请看下面解释文本相似度 vlookup 的视频

例如，如果您有一个销售数据表，并且您想知道相应销售人员的姓名，您可以使用 VLOOKUP 搜索销售人员 ID 并返回相应销售人员的姓名。在本例中，VLOOKUP 函数搜索表的第一列中的值(在本例中为销售人员 ID ),并返回由第二个参数(在本例中为销售人员的姓名)指定的列的同一行中的值。

例如，如果您有一个包含相同姓氏和名字的姓名列表，但是缺少中间名，则可以使用模糊查找来查找中间名。当您与不太关心数据输入准确性的客户一起工作时，这是一个清理数据的好方法。模糊查找也适用于地址列表。例如，如果您有一个手动输入的地址列表，其中一些是缩写的，您可能希望找到每个地址的完整地址。模糊查找也可以用来查找电话号码和电子邮件。您还可以使用模糊查找来查找列表中的特定单词或短语，如果您的列表使用您不理解的语言，或者如果您想要查找输入不一致的特定单词或短语，这将非常有用。模糊查找对任何翻译者来说都是非常有用的工具

下面的视频链接说明了如何使用模糊查找匹配添加在*。*