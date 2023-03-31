# UNIX/Linux 的顶级命令…每个开发者都必须知道。

> 原文：<https://medium.com/analytics-vidhya/top-commands-of-unix-linux-every-developer-must-know-34cb154d3aa6?source=collection_archive---------12----------------------->

![](img/6d5b8815b46d18e7a392cc8ae61b62d9.png)

阿里安·达尔维什在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们作为开发人员工作时，我们必须知道 Unix/Linux 的命令。在预算紧张的公司中，有许多计算机，如果他们在每台计算机上安装 Microsoft window，对公司来说成本太高。

Linux 是**免费的**。无论你在多少台电脑上安装它，Linux 的成本都是零。

![](img/f45debc2c0aaeddcda2029f9649037ab.png)

凯文·霍尔瓦特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

大多数 Linux 发行版每六到九个月就有一个更新版本，没有其他操作系统像它一样发布更新。

Linux 是独立于平台的。 Linux 与其他系统配合得很好。它意识到了 Windows 和 Mac OS X 的存在，并将安装在它们旁边，与它们共享文件，并且通常对它们很友好。这与 Windows 的观点非常不同，Windows 认为多重启动意味着在 Windows 7 和 Vista 之间进行选择。

**威胁检测和解决方案**速度非常快，因为 Linux 主要是由社区驱动的，无论何时任何 Linux 用户发布任何类型的威胁，都会有几个开发人员从世界各地开始着手处理。

是的，我知道你可以在一台 Windows 7 机器上拥有多个账户，但这并不能使它成为真正的**多用户。**在 Windows 7 中可以一次登录多个用户吗？不是默认的。要在 Windows 7 上进行并发用户会话，您必须下载第三方工具。在 Linux 中，默认情况下可以这样做。

任何用户都可以准确地知道任何发行版的某个特性冻结发生的时间。所有的 Linux 发行版都在**完全公开模式下工作**。

大部分都是。做服务器和服务器的公司，没有 Linux 什么都不是。

![](img/2026e7f246d61803ad4c402b1484d229.png)

照片由 [XPS](https://unsplash.com/@xps?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Linux 不需要太多资源，这意味着你可以在不关机的情况下运行你的电脑一年，它也不会变慢。

我会说，不用读任何书，使用 Linux 作为你的主要驱动程序，并尝试在终端中完成每一项任务，因为信任比使用 UI 更容易。

是的，不管您将使用什么操作系统，您绝对应该至少掌握终端的工作知识。有些任务在终端上完成会简单得多。示例:需要递归扫描每个目录并找到哪些文件包含某个字符串？在 Linux 中，这就像打开终端并输入:

```
**grep -r "your string here"**
```

这些是我们应该知道的基本命令…

1.  查看谁登录了 Unix 系统。

```
**who or whoami**
```

2.添加用户。

```
**Useradd <username>**
```

3.更改密码。

```
**passwd**
```

4.以 root 权限运行程序。

```
**sudo**
```

5.清除命令屏幕。

```
**clear**
```

6.列出目录/文件夹中的所有文件。

```
**ls**
```

7.创建目录/文件夹。

```
**mkdir <DIRECTORY_Name>**
```

8.删除空目录/文件夹。

```
**rmdir <DIRECTORY_Name>**
```

9.删除文件。

```
**rm <Filename>**
```

10.改变方向或穿越特定方向。

```
**cd <Dirname>**
```

11.打印工作目录。

```
**pwd**
```

12.将文件 1 重命名为文件 2。

```
**mv <File1 File2>**
```

13.文件已移至目录。

```
**mv <Filename DirectoryName>**
```

14.按页面方式显示文件内容。

```
**pg <Filename>**
```

15.查看文件。

```
**cat <Filename>**
```

更多的命令在这个博客的第二部分。请尝试在终端上运行所有命令。

感谢您的阅读，如果您喜欢 Clap，请关注我们，获取更多简单有趣的文章。另一个博客的链接。

[](/analytics-vidhya/what-and-why-opencv-3b807ade73a0) [## OpenCV 是什么，为什么这么受欢迎？

### OpenCV 是用于实时计算机视觉的编程函数库。它还支持深度…

medium.com](/analytics-vidhya/what-and-why-opencv-3b807ade73a0) [](/analytics-vidhya/the-top-and-best-content-of-machine-learning-deep-learning-artificial-intelligence-free-9e36186d4b2) [## 机器学习、深度学习、人工智能(免费)的顶级最好的内容

### 这篇中型文章特别为那些想在机器学习/深度学习领域开始职业生涯的初学者而写…

medium.com](/analytics-vidhya/the-top-and-best-content-of-machine-learning-deep-learning-artificial-intelligence-free-9e36186d4b2) [](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350) [## 从 JSON 文件中提取有用数据用于机器学习

### 如何从一个 JSON 文件中提取数据用于 Python 中的机器学习模型

medium.com](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350)