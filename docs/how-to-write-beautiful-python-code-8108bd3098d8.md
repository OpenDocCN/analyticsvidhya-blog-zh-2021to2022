# 如何写出漂亮的 python 代码？

> 原文：<https://medium.com/analytics-vidhya/how-to-write-beautiful-python-code-8108bd3098d8?source=collection_archive---------1----------------------->

## 注意名字，我会注意格式

![](img/cc337c0f26cef51b1a3c340180eda717.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

在这篇博客中，您将学习如何转换您的 python 代码，并使其美观、易读和结构化。这个想法是，代码应该像故事一样流动。现在，如果这看起来有点吓人，坚持住，因为除了编写文档和命名变量，其他一切都是自动化的。

# 命名约定

现在 Python 中的命名约定有点乱。这不是我的解读，是 pep8 这样写的:

“Python 库的命名约定有点混乱，所以我们永远也不会做到完全一致——然而，这里是目前推荐的命名标准。新的模块和包(包括第三方框架)应该按照这些标准编写，但是如果现有的库有不同的风格，内部一致性是首选的。”— [ [PEP 8](https://www.python.org/dev/peps/pep-0008/#naming-conventions)

让我给你一些简单的规则。

规则 1 —用小写字母书写变量名、方法名和函数名，用下划线隔开。

规则 2 —保持这些变量名简短，它们是你故事中的角色。避免使用“非常长的变量名称”，而是使用“短名称”

*提示*:将变量人性化，把变量当成你的朋友，并据此命名。

**规则 3** —所有常量均为大写

**规则 4** — CamelCase 类名

有了这些简单的规则，你就不会违反任何 pep8 原则。还有，不用担心全部记住。在自动格式化部分的后面，我将分享一个很棒的工具，它将帮助你找到错误的变量名并重命名它们。只要确保您保留了函数和变量名，因为它们是您的朋友。

# 自动格式化

找出你的分数(满分 10 分)

不要为你的第一个分数难过，我的分数是 a -4.5。保存 python 文件并执行以下操作。

第一步

第二步

在您的代码刚刚运行的目录中

简单吧？—这会给你满分 10 分，并且会很好地指出所有的问题。现在，在进行任何更改之前，只需运行以下命令。

第三步

第四步

第五步

再次运行 pylint，看看你的分数有没有提高。

(Pylint 非常重视空格！)

第六步

现在我们几乎已经完成了代码的美化，你可以在 autopep8 之后直观地看到不同之处。试着攻击皮林特报告中的每一点。我感觉好的代码都有 8.5 分以上。这不是一个硬性规定，但根据我作为数据科学工程师的行业经验，我们可以毫无问题地接受它。

让我再给你一些自动格式化的工具。

1.  [autopep 8](https://pypi.org/project/autopep8/)——如上所述
2.  [Yapf](https://github.com/google/yapf)
3.  [黑色](https://github.com/psf/black)

P.S. Pylint 也作为 VS 代码扩展出现，它突出显示变量名和其他与 pep8 不同的地方。确保你也激活了它。

# 评论< Docstring < Standard Documentation

Now, if you have run the pylint and if you are writing non-documented code you will come across one very peculiar error in pylint — docstring missing. Docstrings are like a small writeup about your code, class, functions, and methods, its task (ideally, it should be in one line), and about its arguments i.e., input and return i.e. output.

The various docstrings are given in the snippet below.

Now we can take the documentation to the next level (as I have done above) by following set formats of docstrings and later on, use a module like the sphinx. I highly recommend reading about various standard formats of docstrings. I like to follow the NumPy format given in this link. [【链接】](https://numpydoc.readthedocs.io/en/latest/format.html)

如果您是 VS 代码用户，一个非常好的扩展可以帮助您自动创建标准文档格式的占位符。这个扩展是“Python Docstring Generator”——这里的[链接](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring)。

# 版本控制的重要性

![](img/aa5321995c7d046d5f653942dbfec027.png)

[Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

我们在本地开发时经常忽略的一点是版本控制。如果你开始使用它，你可以避免很多令人头痛的事情。这与如何编写漂亮的 python 代码略有不同，但在维护每个文件的长期历史、分支和合并以测试不同的特性、高可用性和灾难恢复方面增加了巨大的价值。

# 把所有的东西放在一起

以 docstring 开始您的代码:范围、目标和如何使用它。在本节中添加有关附加文档的更多详细信息。我知道这个要求很过分，但是相信我，它增加了很多价值。这就像是对你的代码的介绍，你是作者。从一开始就添加一个自述文件。

我们从所有的导入开始，pylint 将处理所有的导入序列并声明所有的常量。现在声明所有的函数和方法，并在每个函数中添加一个标准的 docstring。维护一个功能流，每个功能应该有且只有一个任务。运行任何自动格式化工具，获得您的 pylint 分数，将分数提高到 8 分以上，就大功告成了。一个漂亮的 python 代码。

# Python 的禅。

不管讨论了多少技巧和诀窍，它总是从“导入这个”开始。如果你以前没有尝试过，只要在 python 解释器中输入并执行“import this”，你就会看到 Tim Peters 写的 19 条指导原则。这些在[ [PEP 20](https://www.python.org/dev/peps/pep-0020/) ]中也有提及

这些原则在下面给出，我强烈推荐阅读它们，我在这篇文章中没有涉及到。

1.  漂亮总比难看好。
2.  显性比隐性好。
3.  简单比复杂好。
4.  复杂总比复杂好。
5.  扁平的比嵌套的好。
6.  疏比密好。
7.  可读性很重要。
8.  特例不足以特殊到打破规则。
9.  虽然实用性战胜了纯粹性。
10.  错误永远不会无声无息地过去。
11.  除非明确沉默。
12.  面对暧昧，拒绝猜测的诱惑。
13.  应该有一种——最好只有一种——显而易见的方法来做这件事。
14.  尽管这种方式一开始可能并不明显，除非你是荷兰人。
15.  现在总比没有好。
16.  尽管从来没有比现在更好。
17.  如果实现很难解释，这是一个坏主意。
18.  如果实现很容易解释，这可能是一个好主意。
19.  名称空间是一个非常棒的想法——让我们多做一些吧！

我希望您的代码现在更像样，易于阅读和理解。在以后的生活中，当你看着你的代码时，你会感谢你自己，你把它记录得如此好，它看起来如此优雅。