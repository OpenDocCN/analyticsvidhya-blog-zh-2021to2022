# UNIX/Linux 的顶级命令…每个开发人员都必须知道——第二部分

> 原文：<https://medium.com/analytics-vidhya/top-commands-of-unix-linux-every-developer-must-know-part2-9c6883e129c5?source=collection_archive---------11----------------------->

![](img/00ffcb4d9f222351f23b9ffed65b5c6a.png)

潘卡杰·帕特尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

16.**创建一个文件。**

```
**vi <Filename>****cat <Filename>**
```

这是一个 Unix 文件。我使用“**VI”**文本编辑器创建它。

现在我们将使用 **" :wq "** 命令进行保存。

17.**复制一个文件。**

```
**cp <filename1 location>**
```

18.**向一个方向复制文件 1 至文件 2 或文件范围。**

```
**cp <filename1 filename2>**
```

19.**更改文件名的新所有者。**

```
**chown <[new-owner] [filename]>**
```

20.**显示系统日期。**

```
**date** 
```

21.**显示系统时间。**

```
**time**
```

22.**逐字节比较两个文件，帮助你找出两个文件是否相同。**

```
**cmp <filename1with_extension   filename2with_extension>**
```

23.**显示论点。**

```
**echo <Argument = "This is Ankit" >**
```

24.**确定给 shell 的命令是作为外部命令执行还是作为内置命令执行。**

```
**type <command name you have to check>**
```

25.顾名思义，帮助命令可以帮助你了解任何内置命令。

```
**help < -dms > < pattern ... >**
```

*   **-d 选项:**当您只想对任何 shell 内置命令*有一个大概的了解时，即*只是给出简短的描述。
*   **-m 选项:**以伪联机帮助页格式显示用法。
*   **-s 选项:**只显示每个匹配主题的简短用法概要。
*   **模式:**指定帮助主题的模式。

26.**通过在 path 环境变量中搜索，定位与给定命令相关的可执行文件。它有如下三种返回状态:**

*   **0** :如果所有指定的命令都找到并可执行。
*   **1** :如果一个或多个指定的命令不存在或不可执行。
*   **2** :如果指定了无效选项。

```
**which [filename1] [filename2] ...**
```

27.**创建一个没有任何内容的文件。使用 touch 命令创建的文件是空的。当用户在创建文件时没有要存储的数据时，可以使用该命令。**

```
**touch <filename>**
```

28." f **ind "扫描指定的目录，并查找与指定选项匹配的文件。**

```
**find <pathname {options} {action}>**
```

29.“grep”是一个优秀的文件搜索工具，用于查找特定的字符串。

```
**grep <[options] string [filename]>**
```

30.**报告文件系统上的空闲块。**

```
**df <[options] [filename]>**
```

请尝试在终端上运行所有命令。

感谢您的阅读，如果您喜欢，请鼓掌并关注我们，获取更多简单有趣的文章。另一个博客的链接。

[](/analytics-vidhya/installation-of-opencv-in-simple-and-easy-way-15556edca7a4) [## 安装 OPENCV 的 5 个简单步骤

### 在这个有趣的教程中，我们将学习在 Ubuntu 系统中设置 OpenCV-Python。以下步骤针对 Ubuntu 16.04 进行了测试…

medium.com](/analytics-vidhya/installation-of-opencv-in-simple-and-easy-way-15556edca7a4) [](/analytics-vidhya/the-top-and-best-content-of-machine-learning-deep-learning-artificial-intelligence-free-9e36186d4b2) [## 机器学习、深度学习、人工智能(免费)的顶级最好的内容

### 这篇中型文章特别为那些想在机器学习/深度学习领域开始职业生涯的初学者而写…

medium.com](/analytics-vidhya/the-top-and-best-content-of-machine-learning-deep-learning-artificial-intelligence-free-9e36186d4b2) [](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350) [## 从 JSON 文件中提取有用数据用于机器学习

### 如何从一个 JSON 文件中提取数据用于 Python 中的机器学习模型

medium.com](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350)