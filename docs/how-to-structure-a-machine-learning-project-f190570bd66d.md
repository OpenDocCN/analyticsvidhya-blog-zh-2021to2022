# 如何构建机器学习项目

> 原文：<https://medium.com/analytics-vidhya/how-to-structure-a-machine-learning-project-f190570bd66d?source=collection_archive---------8----------------------->

我如何构建我的

![](img/a49f26ccf28ae56c41e289d871181f65.png)

[Dimitry Anikin](https://unsplash.com/@anikinearthwalker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/organized?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

有一个结构化的目录来放置文件是非常有用的，并且可以加速你的机器学习项目。虽然没有一种构建机器学习项目的方法(事实上，任何项目)，但我会分享我通常是如何组织我的项目的。

# 概观

您可以在下面查看完整的结构，之后我会检查每个目录的内容。

```
├── data/
│
├── notebooks/
│
├── saves/
│   ├── data/
│   ├── models/
│   └── samples/
│
├── tests/
│
└── utils/
```

# /数据

在这里，我通常存储项目中任何数据集的原始、未处理的原始副本。无论我是从 Kaggle 下载的，还是我自己用刮网器或手工制作的，不管是什么情况。

一旦我把文件放在这里，这个文件夹中的文件通常不会被修改。你可以把它想象成一个数据湖。这样，我总是有我的数据的原始副本，并且可以随意复制和重做。

# /笔记本

虽然笔记本电脑不适合生产，但它们非常适合进行实验、分析、演示等。最好将它们放在一个单独的文件夹中，以使它们更容易跟踪，尤其是在一起尝试几件事情时。

# /保存

您很可能想在您的机器学习项目中保存一些东西，以便不必每次都从头开始。处理的数据(不同级别)和您的模型在此列表中名列前茅。通常，我会在项目开始时创建单独的子文件夹(*/数据*、*/模型*)来存储它们。

有时我在研究一个生成模型，就像一个生成音乐的模型。为此，我使用 */samples* 子文件夹来保存我的模型生成的音乐样本。

# /测试

我听到有人抱怨了吗？

我敢打赌，你没有看到这一点，但测试是伟大的有，甚至在机器学习。事实是，我们确实进行了测试，但是它们通常是手动进行的，在任何地方，当我们没有得到预期的结果时。您可以通过将它们放在一个地方并有意识地组织起来，使它更好。

# /util

现在进入最后一个目录。我在这里保留了对我的项目很重要的 Python 脚本。有人叫它*/脚本*，有人 */src* ，我叫它 */utils* ，你可以随便叫它什么。

在这里结束的代码通常起初分散在多个笔记本上，有无数的重复。但是随着项目的进展，我想减少这些重复，更好地管理我的代码。

最后，我把它们从我的笔记本里拿出来，把它们变成功能，并根据它们的用途把它们归类到这个文件夹的文件里。处理数据、构建、训练和评估模型的代码在这里很常见。

# 笔记本特价

在结束之前，我想把这些简短的特价商品放在笔记本上，您可能会感兴趣。

**解决相对导入错误**

在当前的项目结构中，如果您试图将/ *utils* 目录中的模块导入到 */notebooks* 目录中的笔记本中，您将会得到一个*导入错误*。这是关于相对进口在笔记本中是不可能的。

```
# Generates an import error
from ..utils import models
```

我通常使用的修复方法是创建一个变量 *BASE_DIR* ，它存储项目目录并将其附加到 *sys.path* 。这样做的目的是让 Python 在我每次运行 *import* 语句时，在我的项目目录中查找我想要导入的内容。然后，我可以从项目目录开始访问我的项目中的任何脚本。

```
import os
import sys# For one-level deep notebook
BASE_DIR = os.path.abspath("../")
if BASE_DIR not in sys.path:
    sys.path = [BASE_DIR] + sys.path# This works
from utils import models
```

如果更有意义的话，您可以选择使用环境变量而不是 Python 变量。

**处理移动笔记本**

我经常做的另一件事是写相对于 *BASE_DIR* 的路径。所以，每次我移动任何笔记本(或者经常出现的副本！！！)，我需要改变的只是 *BASE_DIR* 变量，一切都按预期运行。

```
torch.save(clean_data, BASE_DIR **+** "/saves/data/clean_data.pt")
```

**使用更新的脚本**

将代码组织成脚本(这是一个很好的实践)增加了一些不便。您希望 Python 脚本的任何更新都能立即反映在笔记本上，但这并没有发生。在已经导入的模块上调用 import 也没有任何作用。

为此，来自 *importlib* 库的 *reload* 函数就派上了用场，你不必每次更新你的工作都重启你的笔记本。

```
from importlib import reload
from utils import models# Reloads a module that has been imported before.
reload(models)
```

**在 Colab 上工作**

最后，如果你像我一样，喜欢在 Colab 上有一份你的项目的副本，那么你可能会觉得这很有趣。将 google drive 安装(连接)到笔记本上是访问文件和使用 Google drive 上的空间所必需的。通常，我会将这样做的代码放在笔记本的第一个代码单元。

```
from google.colab import drive
drive.mount('/content/drive', force_remount=True)
```

我们还必须用我们的 Google drive 中的确切路径来更改我们对 *BASE_DIR* 的引用，因为在 Colab 文件系统中情况有所不同。由于我们以前的工作(使每个路径相对)，我们可以像在本地一样工作。

```
# It may look like this
BASE_DIR = "/content/drive/MyDrive/MyCurrentProject/"
```

因此，在本地工作时，我注释掉安装部分并更改 *BASE_DIR。T* 在 Colab 上，我取消对它的注释，并使用适当的 *BASE_DIR* 。厉害！

# 结论

这就是本文的全部内容。您可能会发现一些目录名很奇怪，当然，您可以随时将其更改为您认为有意义的名称。主要的一点是理解他们做什么，并使用一个能说明这一点的名字。

如果你觉得这篇文章很有帮助，请记得分享、鼓掌并关注我。

感谢阅读！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)