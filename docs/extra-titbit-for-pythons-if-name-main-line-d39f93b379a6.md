# Python 的 if __name__ == '__main__ '行的额外标题

> 原文：<https://medium.com/analytics-vidhya/extra-titbit-for-pythons-if-name-main-line-d39f93b379a6?source=collection_archive---------15----------------------->

![](img/05016b41266c639fa8b2424024a1334e.png)

塞巴斯蒂安·赫尔曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# Python 中发生的事情

你一定遇到过这句名言，并且震惊地想它到底是什么意思？在谷歌搜索了一番后，你很有可能出现在[的 stackoverflow](https://stackoverflow.com/questions/419163/what-does-if-name-main-do) 。

因此，简而言之，当您直接运行程序时，_ _ main _ _ attribute(“_ _ main _ _”)的值被赋给 __name__ attribute，该属性计算当前模块的名称，否则模块的名称被赋给 __name__ attribute。对于这一行，我们只是试图找出该模块是直接运行的，还是被导入到另一个模块中。

```
if __name__ == '__main__':
    print("Direct run")
else:
    print("Run by another module")
```

## 什么时候用？

*   当从外部(另一个模块)调用时，防止一些代码运行，否则我们会在测试时切换注释/取消注释代码。即用于单元测试目的。
*   当直接调用时，能够将测试值赋给变量。(实际上，这是第一种子弹的特殊类型优势)

# 如果我们说的“外部”是指“另一个程序”，而不是“另一个模块”呢？

假设您正在开发一个调用 python 代码的. Net 程序。与上面的代码不同，这次我们从另一个程序调用一个 python 文件，而不是另一个 python 模块。所以我们不能测试 __name__ 属性的值。我们需要别的东西。

假设我们从 c#向 python 传递 5 个参数。

这是 python 文件的内容。我们用 os.getppid ()获得父进程 ID，用 psutil 获得其名称。其余的是小菜一碟。