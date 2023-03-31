# 蟒蛇 101

> 原文：<https://medium.com/analytics-vidhya/anaconda-101-199c84ca897a?source=collection_archive---------15----------------------->

所以，这里的任何人如果开始在 Windows 上使用 Anaconda，你必须首先感谢它的开发者，因为我们 Windows 人不像“基于 Linux 的操作系统”的人那样幸运(请不要发送链接到*如何在 Windows 10 上双启动 Ubuntu*)。如果你知道，你就知道。

如果你不知道，原因如下:

*Linux 默认支持几乎所有的编程语言，比如 Java、Python、Julia、Ruby、C 和 C++等等。*

基于 Linux 操作系统的人知道把所有东西都放在一个地方是什么感觉。不像 Windows 人什么都要下载包/库。

这就是为什么你会很高兴与 Anaconda 一起工作。如您所知，Anaconda 是一个首选环境，它拥有适用于 Windows、macOS 和 Linux 的所有高性能数据科学库。当然，它主要围绕 Python 展开，但是您也可以找到为 R 安装的包。在这个地方，你不仅可以拥有不同的数据科学库/包，还可以拥有不同的 ide！这使得数据科学项目的工作变得更加容易。

重要的是要知道，除了必须下载 Anaconda 之外，您还可以选择安装 Miniconda，这是 conda 的一个免费的最小安装程序，也是 Anaconda 的一个小而简单的版本。

Anaconda 有超过 1500 个包，占用 3 GB 的空间，所以如果你没有这么多空间或者不需要这么多包，你可以选择 Miniconda。

你也可以在 conda 的帮助下安装更多的包或者创建虚拟环境。那么你问的康达是什么？" Conda 是一个开源的软件包管理系统和环境管理系统，可以在 Windows、macOS 和 Linux 上运行."。所以，它基本上可以帮助你安装软件包(就像 pip 一样)，创建并从一个环境跳到另一个环境。它还可以帮助您使用其他应用程序堆栈，如 Java、Node.js 等。

因此，如果您刚开始使用 Anaconda，这里有一些对您有用的命令:

*阅读文档的提示*

![](img/62df9ea273f908dec26baf195d521f02.png)

礼遇:【tenor.com】T4

## 1.关于入门:

验证是否安装了 Conda 并检查其版本

```
**conda info**
```

将所有软件包更新到 Anaconda 的最新版本。它安装稳定和兼容的版本，但不是最新的。

```
**conda update anaconda**
```

## 2.使用包和通道

安装软件包

```
**conda install PKGNAME**
```

按确切的版本号(3.1.4)安装软件包

```
 **conda install PKGNAME==3.1.4**
```

要安装列出的版本之一(或)

```
**conda install “PKGNAME[version=’3.1.2|3.1.4']”**
```

要安装以下几个约束(和)

```
**conda install “PKGNAME>2.5,<3.2”**
```

## 3.用于处理环境

创建名为 ENVNAME 的新环境

```
**conda create --name ENVNAME**
```

创建一个名为 ENVNAME 的新环境，其中安装了特定版本的 Python 和包

```
 **conda create --name ENVNAME python=3.6 “PKG1>7.6” PKG2**
```

激活命名的 Conda 环境

```
**conda activate ENVNAME**
```

停用当前环境

```
**conda deactivate**
```

列出命名环境中的所有软件包和版本

```
**conda list --name ENVNAME**
```

创建基于确切软件包版本的环境

```
**conda create — name NEWENV --file pkgs.txt**
```

将环境导出到可以在 Windows、macOS 和 Linux 上读取的 YAML 文件

```
 **conda env export --name ENVNAME > envname.yml**
```

## 4.多方面的

有关任何命令的完整文档，请在命令中添加—(双连字符)帮助

```
**conda create --help**
```

要获得关于软件包版本的详细信息

```
 **conda search PKGNAME --info**
```

从环境中删除软件包

```
 **conda uninstall PKGNAME --name ENVNAME**
```