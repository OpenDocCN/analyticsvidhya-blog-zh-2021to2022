# 兰姆达斯、元组和(熊猫)熊…哦，我的天！

> 原文：<https://medium.com/analytics-vidhya/lambdas-and-tuples-and-panda-bears-oh-my-9f6e6b5c10aa?source=collection_archive---------19----------------------->

“对不起，我现在不能说话。我在重复一个与你的价值主张正交的随机森林。”

技术人员说话很有趣。

至少这是当你从一个不太懂技术的背景进入科技世界时的感觉。在这个故事中，我将探索一些有趣的数据科学术语的起源和含义。

当我开始学习数据科学编码时，我被告知我将需要 Python。但是为了得到 Python，我需要 Anaconda。一旦我有了 Anaconda 和 Python，我就应该导入熊猫。我开始怀疑是否所有的数据科学家都痴迷于外来动物。

![](img/4b3a2c3fb4da14f1df53ec5251044db9.png)

戴维·克洛德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**巨蟒**

你可能知道,‘Python’这个名字并不是指大蟒蛇，而是指它的创造者对巨蟒的喜爱。Python 的创造者吉多·范·罗苏姆在编写他的新编程语言时，正在阅读电视连续剧“蒙蒂 Python 的飞行马戏团”的脚本。正如 Python 官方文档在其[常见问题](https://docs.python.org/3/faq/general.html#why-is-it-called-python)中解释的那样，“Van Rossum 认为他需要一个简短、独特、有点神秘的名字，所以他决定称这种语言为 Python。”

**蟒蛇**

Anaconda 公司的文件没有解释其名称的来源。但是由于 Anaconda 是 Python 编程语言的发行版，人们可以猜测它的创造者以另一种长蟒蛇命名他们的产品，以突出它与 Python 的关系。

虽然巨蟒生活在非洲、亚洲和澳大利亚，蟒蛇生活在南美洲，但是这两种蛇都可以长到 9 米(30 英尺)并且是蟒蛇，这意味着它们会缠绕并窒息猎物。两者都很重，但是蟒蛇比巨蟒重，可以重达 250 公斤(550 磅)。水蟒的直径也可以达到 1 米(3 英尺)。呀！我很高兴我只是在用软件包。

熊猫

一旦我设置了 Anaconda 和 Python，我终于准备好导入熊猫了。我住在 DC 的华盛顿州，那里的国家动物园从 20 世纪 70 年代开始从中国进口熊猫。根据史密森学会的说法，熊猫国际旅行的首选方式是联邦快递的货运飞机，飞机上配有动物园管理员和熊猫零食。

![](img/a4aee629bc878084be94adefcc02fb60.png)

由[希德·巴拉钱德朗](https://unsplash.com/@itookthose?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

哦，等等。他们指的是熊猫，就像在用于分析结构化数据集的 Python 库中一样。不是吃竹子的巨型熊([顺便说一下，它们真的是熊](https://www.fws.gov/international/pdf/archive/factsheet-giant-panda-facts.pdf)。该图书馆的作者韦斯·麦金尼(Wes McKinney)称，“该图书馆的名字来源于 panel data，这是统计学和计量经济学中多维数据集的常用术语。”啊，我明白了——潘+大。了解这种派生确实有助于我理解该库的主要目的，即提供一种在 Python 中存储和操作表格数据的简单方法。这也有助于解释为什么熊猫的标准别名是“pd”。

**Seaborn**

和 Python 的作者一样，Seaborn 的创作者从流行文化中汲取灵感。 [Michael Waskom 以电视剧《白宫风云》中的虚构人物 Samuel Norman Seaborn 命名他的统计可视化工具库](https://github.com/mwaskom/seaborn/issues/229)。因此，图书馆的标准别名是其同名的首字母，sns。

**Matplotlib**

Seaborn 是基于可视化库 Matplotlib 构建的，相比之下，Matplotlib 的名字非常有意义。根据 Matplotlib 用户指南，这个用于制作好看的**剧情**的 Python **lib** rary 基于 **MAT** LAB，一种更老的编程语言。根据 MathWorks 的作者 [Cleve Moler 的说法，MATLAB 的名字是 Matrix Laboratory 的首字母缩写。有趣的是，matrix 这个词来源于拉丁语 *mater* (母亲)，根据](https://www.mathworks.com/company/newsletters/articles/a-brief-history-of-matlab.html)[韦氏词典](https://www.merriam-webster.com/dictionary/matrix#h1)，它最初的拉丁语意思是“用于繁殖的雌性动物”。这个意思启发了詹姆斯·西尔维斯特在 1850 年用“矩阵”来描述一个矩形的数字阵列:

在以前的论文中，我将“矩阵”定义为一个矩形的项阵列，不同的决定因素系统可能从其中产生，就像从一个共同的母体子宫中产生一样(《詹姆斯·约瑟夫·西尔维斯特数学文集:1837–1853》，[第 37 卷](https://books.google.com/books?id=5GQPlxWrDiEC&pg=PA247)，第 247 页)

![](img/9d12f0275a141dce9afafab6c607ca96.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**元组**

作为一个初学编码的人，我开始遇到不熟悉的单词。比如什么是“元组”，为什么 Python 一直指责我传递它们？

Python 中的[元组](https://www.tutorialspoint.com/python/python_tuples.htm)是有序的、不可改变的对象集合。它不同于 Python 列表，因为列表是可以改变的。元组用括号写，而列表用方括号写。

该词源自英语单词中使用的后缀- [*tuple*](https://en.wiktionary.org/wiki/tuple) ，如五元组、六元组、七元组和通用术语 n 元组。

**浮动**

我还对数据类型“float”感到惊讶，这意味着任何包含小数点的实数。为什么不直接叫它小数呢？答案是“ [float](https://www.quora.com/In-Python-why-is-float-called-float-Why-not-just-call-it-realnum-or-something#:~:text=In%20python%2C%20and%20programming%20in,float'%20around%20the%20decimal%20place.) 来源于术语“[浮点运算](http://www.dspguide.com/ch28/4.htm)”，这是计算机表示实数的一种特定方式。一个数字的小数点可以“浮动”或放在任何地方，然后使用指数对数字进行缩放。

**布尔**

类似地，我可以为“Boolean”想到许多更合理的名称，Boolean 是一种可以有两个可能值的数据类型，比如 True 和 False。我们应该感谢为这个术语发明了布尔代数的乔治·布尔。事实证明，我们有很多东西要感谢他，因为他在 19 世纪发明的符号逻辑为现代计算机科学奠定了基础。

**λ**

虽然“熊猫”来源于“面板数据”，但 lambda 表达式与羔羊或数据无关，尽管这很可爱。Python 中的 lambda 表达式用于动态创建一个函数，该函数将应用于一个系列中的每一项。数学家们会认识到这个术语，因为它来自于λ微积分。 [Alonzo Church 选择了希腊字母 lambda 来表示一般函数的概念，这是因为他最初使用了一个插入符号，就像在^x 一样，然后为了便于印刷，他将它改为了ƛx。(](http://www.users.waitrose.com/~hindley/SomePapers_PDFs/2006CarHin,HistlamRp.pdf)[http://www . users . waitrose . com/~ Hindley/some papers _ pdf/2006 car hin，HistlamRp.pdf](http://www.users.waitrose.com/~hindley/SomePapers_PDFs/2006CarHin,HistlamRp.pdf) ，第 9 页)

![](img/20a87c11a9b82b197b83c04d54b1b59e.png)

埃文·丹尼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**随机森林**

在掌握了 lambdas、tuples 和 panda bears(哦，我的天)之后，我准备开始运行机器学习模型，比如随机森林。“随机森林”这个名字可能是数据科学中最不随机的术语。该模型由一组决策树组成，每一个决策树使用从相同数据中随机选择的特征来预测结果。所有树的预测被合并以产生最终的预测。

**迭代**

机器学习模型需要调整才能产生最佳结果，这意味着要一遍又一遍地运行它们，只做微小的改动，看看哪个版本是最好的。这就是为什么数据科学家一直使用“迭代”这个词。术语“迭代”有几个不同的[定义](https://www.google.com/search?q=iterate+definition&oq=iterate+defi&aqs=chrome.0.0i433j69i57j0i20i263j0l2j0i22i30l5.1993j1j4&sourceid=chrome&ie=UTF-8)，其中最基本的是“重复”，源自拉丁语 *iterum* (再次)。在计算机科学中，迭代意味着“重复使用数学或计算过程，每次都将其应用于前一次应用的结果。”机器学习算法被构建为迭代，直到它们找到最佳结果。显然，人们也可以迭代——他们可以反复调整模型，并从结果中学习以改进模型。因此有这样一句话，“让我们迭代这个模型。”

**正交**

哦，正交的。不知何故，这个术语从数学和统计学中脱离出来，进入了软件工程，并从那里进入了商业和法律术语。我很欣赏说“正交”是多么有趣，但我不认为它提供了比更普遍理解的词，如“不相关”或“独立”更精确的含义。但是听起来确实很酷。

"[正交](https://en.wikipedia.org/wiki/Orthogonality#:~:text=In%20Euclidean%20space%2C%20two%20vectors,to%20spaces%20of%20any%20dimension.)"最初是一个数学术语，意思是成直角，源自希腊语 *orthos* (直立)和 *gonia* (角度)。在数学中，“正交”用于描述垂直线，以及成直角、点积为 0 且互不影响的向量。在统计学中，它适用于不相关的变量(基于上面的向量定义)。在软件工程中(现在还有许多其他领域)，它被用来描述一个项目中互不影响的两个部分。2010 年，当一名律师在一场辩论中使用它时，它的用法让最高法院首席大法官约翰·罗伯茨感到困惑。

![](img/4f2a1252ec6139962548ccf5017522f3.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**男人**

虽然我没有打算探索数学和计算机科学的历史，但令人惊讶的是，这个故事中所有的数学概念和编程库都是由男性命名的。即使是流行文化参考(蒙蒂 Python，白宫风云)也大多是由男人写的关于男人的。我最近在谷歌上搜索了为什么数据科学图书馆 scikit-learn 在其文档中如此频繁地使用随机状态 42，并发现 42 是对“搭便车”书籍的引用，这本书也是由一个关于男人的男人写的。我绝不是在指责这些人故意宣扬男性主导的文化，只是注意到这种文化在计算机和编程领域的普遍性。令人不安的是，matrix(计算机科学中的一个基本概念)是以“用于繁殖的雌性动物”命名的。我希望很快有更多的女性编写编程语言和库，并为它们选择有趣的名称。