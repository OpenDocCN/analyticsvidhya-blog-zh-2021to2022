# 使用 NumPy 和 Matplotlib 的简单相机模型

> 原文：<https://medium.com/analytics-vidhya/simple-camera-models-with-numpy-and-matplotlib-92281f15f9b2?source=collection_archive---------1----------------------->

![](img/586de8300f72d93af7fdd4fa63e12ed4.png)

在 [Unsplash](/s/photos/camera?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [ShareGrid](https://unsplash.com/@sharegrid?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片。

# 介绍

这篇文章中的代码可以在[这里](https://github.com/mnslarcher/camera-models)找到。我建议您看一看它，并尝试使用它来完全吸收将要介绍的概念。

```
import matplotlib.pyplot as plt
import numpy as np

from camera_models import *  # our package

DECIMALS = 2  # how many decimal places to use in print
```