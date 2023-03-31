# 比较两个 CSV 文件，找到相同或不同的数据

> 原文：<https://medium.com/analytics-vidhya/compare-two-csv-files-and-find-common-or-different-data-492f1261a7ce?source=collection_archive---------13----------------------->

![](img/064462bd2b2bc7f34f51501ce6e60acc.png)

例如，有两个文本文件。两个文件的每一行都是一个字符串。我们想逐行比较它们。为此，我们将每个文件的每一行作为一个字符串读取，以形成一组字符串，然后执行 set 操作。

paint.txt 和 dance.txt 分别记录报名绘画班和舞蹈班的孩子的 id 和名字。以下是 paint.txt 的部分内容:

```
20121102-Joan
20121107-Jack
20121113-Mike
```

# 查找公共数据

找到两个文件的共同记录就是得到它们的交集。

示例:查找报名参加绘画班和舞蹈班的所有孩子，并将他们的记录写入 result.xlsx。

esProc SPL 脚本:

# 寻找差异

找出两个文件的所有不同记录。

示例:查找报名参加绘画班或舞蹈班的孩子。

esProc SPL 脚本:

在数据分析工作中，比较两个文本文件以找到它们的共同或不同数据并不是一件罕见的事情。根据要比较的内容，有按行比较和按关键字段比较；根据比较文件的大小，有小文件比较和大文件比较。更多详情，请参见[对比文件样本](http://c.raqsoft.com/article/1600309188122)并了解更多关于 [esProc 和 SPL](http://www.raqsoft.com/p/script-over-csv-xls) 的信息