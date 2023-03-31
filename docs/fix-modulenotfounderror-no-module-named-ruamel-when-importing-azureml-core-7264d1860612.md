# 修复 ModuleNotFoundError:导入 azureml.core 时没有名为“ruamel”的模块

> 原文：<https://medium.com/analytics-vidhya/fix-modulenotfounderror-no-module-named-ruamel-when-importing-azureml-core-7264d1860612?source=collection_archive---------5----------------------->

![](img/f864cb6f46d27b5bd4fbdf5ef970d9a6.png)

天蓝色 ML

如果你在 Visual Studio 代码中使用过 Azure 机器学习 Python 库，你可能会从微软教程开始，该教程显示了如下代码:

```
from azureml.core import Workspace
```

看到这些看似无害的代码后，您(在某个时候)从 pypi 安装 azure ml . core…