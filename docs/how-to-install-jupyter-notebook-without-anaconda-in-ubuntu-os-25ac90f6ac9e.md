# 如何在 Ubuntu OS 中安装没有 Anaconda 的 Jupyter 笔记本

> 原文：<https://medium.com/analytics-vidhya/how-to-install-jupyter-notebook-without-anaconda-in-ubuntu-os-25ac90f6ac9e?source=collection_archive---------1----------------------->

如果你正在阅读这篇文章，那么很明显你应该知道 Anaconda(不是蛇！！)，如果不是参考这个 [*贴*](/analytics-vidhya/a-brief-overview-python-and-anaconda-for-starters-4a38676d5778)

![](img/fcc041ce9b943a85ed033849cb0b90b3.png)

现在让我们直接进入安装过程。这篇文章是 Quora 空间文章[在 ubuntu 上安装虚拟环境](https://qr.ae/pG9dTT)的延伸

当我们处理 Python 时，使用虚拟环境总是好的。因此，让我们从创建虚拟环境开始。

**步骤:1 创建虚拟环境**

进入你的命令行并输入“pip3 install virtualenv”

```
$ pip install virtualenv
```

现在我们已经安装了 virtualenv 包，是时候使用命令 virtualenv <name_of_your_environment>创建虚拟环境了</name_of_your_environment>

要创建一个虚拟环境，使用命令“virtualenv <name_of_the_environment>”</name_of_the_environment>

我的第一个环境

```
$ virtualenv my_first_environment
```

如果您想要使用特定的 python 版本创建虚拟环境，那么使用“— python”标志

例如:virtualenv my _ first _ environment-python =/usr/bin/python 3.5

```
$ virtualenv my_first_environment --python=/usr/bin/python3.5
```

运行上述命令后，您应该会找到一个名为“my_first_environment”的文件夹

**步骤 2 虚拟环境的激活**

要激活我们在步骤 1 中创建的虚拟环境，请转到包含虚拟环境的文件夹并运行命令

"信号源 <name_of_the_environment>/bin/activate "</name_of_the_environment>

例如:source my _ first _ environment/bin/activate

```
$ source my_first_environment/bin/activate
```

**步骤 3 Jupyter 笔记本的安装**

一旦创建并激活了虚拟环境，我们就离安装 Jupyter 笔记本只有两步之遥了

使用以下两个命令

*“pip 安装笔记本”*

*"pip 安装 jedi==0.17.2"*

(pip 安装 jedi 是为了修复 Jupyter 笔记本中的自动完成功能)

```
$ pip install notebook
$ pip install jedi==0.17.2
```

万岁！！您已经安装了没有 anaconda 的 jupyter 笔记本，现在您可以从您的终端键入“jupyter 笔记本”,它将启动笔记本

不要忘记留下两个掌声(更多的掌声是赞赏😅)如果这篇文章对你有帮助

***问候，***

***维格内什***

[艾匹林奴](https://aipylinux.quora.com/)