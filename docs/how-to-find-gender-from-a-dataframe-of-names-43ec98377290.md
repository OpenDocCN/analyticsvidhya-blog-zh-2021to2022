# 如何从一个数据帧中找到性别？

> 原文：<https://medium.com/analytics-vidhya/how-to-find-gender-from-a-dataframe-of-names-43ec98377290?source=collection_archive---------9----------------------->

> 从数据帧中的姓名推断性别的 5 个简单步骤

**要求:**

*   [熊猫](https://pandas.pydata.org/docs/)
*   [性别化 API](https://pypi.org/project/Genderize/)

**步骤:**

**1。在您的环境中安装 gender ize**

```
pip intall genderize
```

**2。在你的 Python 脚本或 Jupyter 笔记本中导入 gender ize**

```
from genderize import Genderize
```

**3。创建一个函数，通过传递姓名并返回姓名的性别来应用 Genderize API 调用** …