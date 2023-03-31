# 如何用 PIP 安装模块(并在失败时修复它)

> 原文：<https://medium.com/analytics-vidhya/how-to-install-modules-with-pip-and-fix-it-when-it-fails-a8f2b6a0c9b9?source=collection_archive---------2----------------------->

![](img/647dda85066a8e09b8804966079628db.png)

这是讨论在尽可能短的时间内全面掌握 Python 编程语言所需的一切的系列文章的一部分。不管你是初学者还是专家，我都希望你能学到一些新东西。

PIP 是一个强大的工具，每个人都应该知道如何使用。但是每个人都曾经遇到过 Python 的 PIP 给他们带来麻烦的情况，他们不知道该如何继续。**我已经学会了许多对我来说非常成功的排除 PIP 故障的方法。**

# 如何使用画中画

将模块安装到 python 非常容易。只需打开您的终端:

*   Windows —命令提示符(CMD)
*   MacOS —终端
*   Linux —终端(视情况而定……)

现在，在命令行中，键入:

```
pip install (THE NAME OF THE MODULE)
```

有时，在 MacOS 和 Linux 中，您可能需要键入:

```
sudo pip install (THE NAME OF THE MODULE)
```

(这在 Windows 下无论怎么努力都不行。)

如果这些都不起作用，那就意味着你有问题。**所以让我们试着解决这个问题。**

# PIP 故障排除

# 基本的东西

当你感到沮丧时，有时很容易忘记仔细检查东西。**首先确保你已经安装了 Python。**假设，首先要做的是检查模块是否存在。这样做的主要方法是去 [PyPi](https://pypi.org/) 并搜索你的包。如果什么都没有出现，一定要做一个快速的谷歌搜索，看看你是否在搜索正确的名字。StackOverflow 是你的朋友。

![](img/eb6fb09f410b964abc73e71253c5ac9b.png)

**如果您找到了您需要的模块，只需从页面上复制命令并粘贴到您的终端上，您就可以离开了。**

如果这仍然不起作用，在继续其余步骤之前，尝试最后一件事。尝试键入:

```
pip3 install (THE NAME OF THE MODULE)
```

如果您安装了多个版本的 python，其中一个是 Python3，这可能行得通。

您可能会发现自己所处的一个特殊情况是，python 模块曾经存在，但由于某种原因，它不再是 PIP 的一部分。在这种情况下，我只有一个解决办法。这是为了尝试为 python 模块找到一个. whl 或**车轮包**。我将在文章的后面介绍安装这些类型的文件[。](https://kgdavidson.co.uk/python-bare-minima--how-to-install-modules-with-pip-and-fix-it-when-it-fails/#installing-wheel-packages)

# 试图修复皮普

您可以尝试使用以下工具升级 pip:

```
python -m pip install --upgrade pip
```

或者，您可以尝试通过以下方式从头开始安装 pip。

**从**[**get-pip . py**](https://bootstrap.pypa.io/get-pip.py)**中复制代码或者从链接中保存文件。然后简单地用 python 运行这个文件。这将为您安装 pip 并使其工作。如果需要，请确保尝试使用 pip3。**

# 决定性解决方案

如果其他方法都失败了，这是让 pip 在 python 安装上工作的可靠方法。**我想先说一个事实，这不应该被一致地使用，我个人建议在卸载所有当前安装后简单地重新安装 python。**解决方案是尝试以下命令之一:

```
python -m pip install (THE NAME OF THE MODULE) 
python3 -m pip install (THE NAME OF THE MODULE)
```

这通过 python shell 运行 pip，并且几乎保证可以工作。如果这仍然不起作用，你应该在从 [Python 网站](https://www.python.org/downloads/)重新安装 Python3 之前卸载 Python 的所有痕迹。

# 安装车轮组件

您可以使用以下命令之一来安装转轮套件:

```
pip install (THE PATH TO THE WHL FILE) 
pip3 install (THE PATH TO THE WHL FILE) 
python -m pip install (THE PATH TO THE WHL FILE) 
python3 -m pip install (THE PATH TO THE WHL FILE)
```