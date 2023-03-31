# 开发 101:可教的时刻(用 Python)

> 原文：<https://medium.com/analytics-vidhya/dev-101-teachable-moments-with-python-7f620ea3c916?source=collection_archive---------14----------------------->

在 *Dev 101* 系列中，我为广大读者讲述了一些计算机编程的基本概念。我想这是我自己在寻找的解释，当我刚开始做程序员的时候…

![](img/d875ac590c860fe61514b279a87d1181.png)

图为 [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你的代码充满了可教的时刻。利用它们为你造福。

# 陷入困境

学习编程语言的基础——尤其是你的第一语言——通常是一种复杂的经历。一方面，你可能会觉得自己被赋予了相当大的力量，现在就在你敲击键盘的指尖。需要从新闻档案中获取文本语料库吗？没问题，你现在应该可以组装一个方便的网络爬虫了。想要重组您的照片库吗？简单，只需迭代图像，读取文件修改日期，并复制到 YYYY-MM 格式的文件夹中。

另一方面，你会很快发现基础只是众所周知的冰山一角。而发现这一点最常见的方式就是遇到麻烦。让我们以我们的网络爬虫为例，它可能看起来像这样:

如果您只运行一次，这段代码可能会满足您的需要，但是您可能需要改进它。例如，我们应该将我们的 URL 请求包装在一个`try ... except`中，以捕捉 HTTP 和 URL 错误。此外，这个版本“只”访问 1000 页，但如果你需要更多或下载量巨大呢？有什么方法可以加快这个速度？

另一方面，我们的照片库脚本可能是这样运行的:

你可以试着运行它，但是我不建议这样做，因为这里(至少)有一个主要的 bug。想一想:例如，当脚本复制两个不同的文件时会发生什么，这两个文件共享相同的名称并且是在同一个月拍摄的？

```
cp London/selfie.jpeg /home/username/Photolibrary/202012/selfie.jpeg
cp Bruges/selfie.jpeg /home/username/Photolibrary/202012/selfie.jpeg
```

没错；现在您正在覆盖文件。你会怎么解决这个问题？

因此，尽管你最初对能够"[用 Python](https://automatetheboringstuff.com/) 自动化枯燥的东西"充满热情，但这很容易(顺便说一下，也很正常！)开始写代码的时候遇到麻烦。

# 深入挖掘

解决这个问题的方法很简单，同时也是一生的工作:不断学习。如果你坚持学习、练习、阅读，你会学到更先进的概念和技术，这将帮助你解决你遇到的任何麻烦。例如，学习 Python 中的[并发性](https://realpython.com/python-concurrency/)，比如使用`concurrent.futures`模块的多线程，将使您能够制作更快的爬虫。当您意识到不将文件系统路径作为字符串，而是作为带有`[pathlib](https://towardsdatascience.com/10-examples-to-master-python-pathlib-1249cc77de0b)`模块的对象来处理时，您将很容易通过在`*path*.stem`(例如`selfie)`和`*path*.suffix`(例如`.jpeg`)之间插入某种索引，用唯一的文件名来修复 photolibrary 脚本。

然而，问题往往是:“从哪里开始？”外面有那么多信息，那么多书要读，那么多课程要上，那么多编码练习要做。一种开始的方法是在你自己的编码实践中认识到可教的时刻。下面我提供一些 Python 的例子，但是学习原则适用于所有语言。

# 可教的时刻

虽然在教育中，可教的时刻根据定义是计划外的机会，但还是有可能创造一种模式识别的思维模式，因为这些可教的时刻可以归纳为几个类别。

## 干燥的

第一种可教的时刻是当你注意到你在重复自己时。例如，编写一个检查遗留代码库的工具，我发现自己经常用字典做这件事:

随着脚本变得越来越复杂，我厌倦了检查一个条目是否已经存在于我的字典中，如果没有，初始化它的默认值。它也使代码膨胀和模糊。一定有更好的方法。当然，还有:`setdefault()`方法。

我相信你以前读过“不要重复自己”的建议，但我希望这个例子能告诉你如何认识到这一点，并把它作为一个可教的时刻。

## 只能有一个

Python 著名的[禅宗](https://www.python.org/dev/peps/pep-0020/)的格言之一说:

> 应该有一种——最好只有一种——显而易见的方法来做这件事。

然而，在实地，这并不总是显而易见的。的确，当你第一次开始使用 Python(和许多其他编程语言，但不是全部！)做同一件事似乎常常有许多不同的方法。考虑这个在字符串中查找子字符串的例子:

当你第一次开始编程时，你要么没有意识到可供选择的选项，要么只是选择你最熟悉的一个。然而，无论何时你发现自己在做后者，你都应该抓住机会认真考虑选项。与编写代码一样，有几个因素在起作用:可读性、效率、可重用性等等。

例如，研究上面的不同实现，您会发现:

*   `in`操作符是 Python 执行[成员测试操作](https://docs.python.org/3/reference/expressions.html#membership-test-operations)的方式。这是解决这个问题最通用的方法，也适用于其他数据类型(列表中的 int、元组中的 bool、字典键中的 string 等等)。因此，如果您想将此功能重构为一个通用的函数或类，它也是合适的。
*   `*string*.find()`是一个字符串的特定方法。当没有找到子串时，它返回子串在 string 或`-1`中的索引。这意味着这里发生的不仅仅是简单地检查成员资格，这也解释了为什么这个选项一定会比较慢:

当您在具有神奇功能`%timeit`的 Jupyter 笔记本上运行时，您会看到不同之处，这在性能关键的环境中可能相当明显:

```
%timeit check_in()
%timeit check_find()259 µs ± 31.9 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
273 µs ± 66.4 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

第三个选项，用正则表达式搜索，甚至更慢(即使你在函数之外做了`import`)。正如您在[文档](https://docs.python.org/3/howto/regex.html)中所看到的，“正则表达式模式被编译成一系列字节码，然后由用 C 编写的匹配引擎执行”，这意味着幕后发生的事情比`*string*.find()`还要多:

```
253 µs ± 34.6 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each) 
1.4 ms ± 65.9 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

通过这种方式，您看到了发现“等价”实现是如何成为另一个可以加深您对代码实际工作方式的理解的教育时机。

## 这不可能是对的

有时你会发现自己写的代码功能足够好，但仍然让你的[蜘蛛感觉](https://en.wiktionary.org/wiki/Spidey-sense)刺痛。对我来说，这发生在我刚开始在多种条件下工作的时候。例如，我发现自己串联了多个`or`操作符，如下所示:

```
if (char == "!" or char == "." or char == "?" or char == ";" or char == "," or char == ":"):
    # do something
```

在这样做了一段时间并阅读了关于好代码简洁的文章后，我知道一定有更好的方法。事实上，我们已经讨论过了:

```
if char in ["!", ".", "?", ";", ",", ":"]:
    # do something
```

多重条件的另一个例子是，当我的一个学生将 API 错误代码翻译成类似这样的退出消息时:

当我向他们展示您可以将 API 文档中的所有错误代码和消息放入一个字典中，然后像下面这样退出时，他们真是大吃一惊:

所以，每当你注意到你的代码变得冗长或笨拙时，相信你的直觉，利用那个时刻去学习更好的解决方案！

## 到底怎么……？

有时候，你与其说是在寻找更好的做事方式，不如说是在寻找任何解决编码难题的方式。有时，手头的任务会让人感到难以忍受，甚至看起来不可能。

这是另一个重要的教育时刻。每当你感到在一个问题上卡住了，这通常意味着你的知识和技能已经到了极限，应该寻找新的信息。这通常可以归结为转换视角或跳出框框思考。

例如，最近，我陷入了试图提出一个正则表达式来检测匹配括号的困境。可以做到这一点的东西:

在正则表达式上花了大约一个小时后，我准备放弃了。但是后来我深入 Googled 了一下(其实是“ [DuckDuckGoed](https://en.wikipedia.org/wiki/DuckDuckGo) ”)这个问题，发现这其实是无法用正则表达式解决的。相反，您需要求助于解析技术—一个非常简单的实现可能是:

这还不是最终产品(我将让您来考虑如何处理像`print(len("("))`这样的字符串中的括号！)，但这是一个巨大的进步。这表明陷入困境不应该导致沮丧和绝望(相信我，虽然，我知道这种感觉)，但实际上提供了一个学习的机会。

## 阅读各类手册

不久前，我需要批量处理一些 Word 文件，我找到了这个 oneliner 来用`docx`库提取文本:

```
from docx import Documenttext = ''.join([p.text for p in Document('myfile.docx').paragraphs])
```

这样聪明的代码都很好，直到你需要改变一些东西，这通常是你意识到你并不真正知道这一行发生了什么。所以为了找出答案，我将这一行分解成被创建的对象，检查它们的类型并对它们执行`dir()`。如果您不知道，`dir()`是一个函数，它返回一个对象的有效属性列表，从而提供对其结构和功能的一些洞察:

输出是:

```
<class 'docx.document.Document'>
['_Document__body', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', '_block_width', '_body', '_element', '_parent', '_part', 'add_heading', 'add_page_break', 'add_paragraph', 'add_picture', 'add_section', 'add_table', 'core_properties', 'element', 'inline_shapes', 'paragraphs', 'part', 'save', 'sections', 'settings', 'styles', 'tables']<class 'list'>
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']<class 'docx.text.paragraph.Paragraph'>
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_element', '_insert_paragraph_before', '_p', '_parent', 'add_run', 'alignment', 'clear', 'insert_paragraph_before', 'paragraph_format', 'part', 'runs', 'style', 'text']<class 'str'>
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

这告诉你所有你需要知道的:

*   `Document`对象有一个类成员变量`paragraphs`
*   `paragraphs`是包含`Paragraph`个对象的列表
*   `Paragraph`对象有一个类成员变量`text`，它是一个字符串

就我个人而言，我喜欢像这样检查变量，但是你也可以留意另一条臭名昭著的编程建议 [RTFM](https://en.wikipedia.org/wiki/RTFM) 并阅读`docx`手册来找到如何使用它。

阅读手册和文档本身就是一种技能，需要练习。这就是我们发现第三种可教时刻的模式。每当你发现自己在使用一个你不是 100%熟悉的函数或方法时，在文档中查找它。即使你认为你很了解它，也要试一试。你可能会惊讶地发现很多你没有意识到的事情。就拿`open()`来说吧，你可能认为它是众所周知的。你真的能声称知道所有记录在案的[参数](https://docs.python.org/3/library/functions.html?highlight=open#open)的作用吗？

> *打开* (file，mode='r '，buffering=-1，encoding=None，errors=None，newline=None，closefd=True，opener=None)

因此，通过研究文档进行更深入的研究——即使是在熟悉的领域——是另一种很好的改进方法。

# 结论

让我以我个人对如何成为一名软件工程师的看法来结束这篇文章。我相信你只需要一种天赋，那就是快速和独立的学习能力。

然而，我也相信这不是一个你有或没有的问题。这与“原始”天赋无关。所有的软件工程师，无论是 CTO 还是菜鸟，都要投入相当大的精力去学习新技术。最好的软件工程师抓住一切机会学习。

这就是识别可教时刻的意义所在。

*嗨！*👋我是汤姆。我是一名软件工程师、技术作家和 IT 倦怠教练。如果想取得联系，可以查看[*https://tomdeneire . github . io*](https://tomdeneire.github.io/)