# 谷歌翻译中的搜索自动化:用 Selenium 下载翻译

> 原文：<https://medium.com/analytics-vidhya/search-automation-in-google-translate-download-translations-with-selenium-3a8c8e136b0e?source=collection_archive---------2----------------------->

![](img/5c6cfc480c84c1e184273e994e2d7065.png)

Sergey Isakhanyan 在 [Unsplash](https://unsplash.com/s/photos/sign-in-foreign-language?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) (亚美尼亚埃里温)拍摄的照片。从上到下:“罗斯托夫唐”市场——700 万，斯捷潘纳克特——332 公里，迪拜——1951 公里)

链接到这篇文章[葡萄牙语翻译在这里。](https://fabriciusbr.medium.com/automatiza%C3%A7%C3%A3o-de-pesquisas-no-google-tradutor-download-de-tradu%C3%A7%C3%B5es-com-selenium-b9b860496d02)

介绍

在我之前的一篇文章中，我开发了一个程序，可以自动打开谷歌翻译网站，将特定文本翻译成另一种语言。然而，给出的代码有一个很大的局限性，因为没有复制翻译结果以便以后使用这些数据的功能。

我们将通过使用 Selenium 库来添加这样的资源，Selenium 库是 web 抓取和自动化测试的最佳可用库之一。在我们开始分析我们的代码之前，有一点很重要:要使用这样一个库，除了惯用的命令 *pip install selenium* 之外，还需要将 geckodriver 文件下载到本地机器上。该文件应该与您的 Chrome 浏览器版本相对应，并且来自您的计算机使用的同一操作系统(Linux、Windows 或 macOS)。有关这些程序的更多详细信息，请参考官方文档的以下页面:

[https://selenium-python.readthedocs.io/installation.html](https://selenium-python.readthedocs.io/installation.html)

对 Windows 用户来说，另一个重要的细节是:你不应该运行 chromedriver.exe 文件(它是 Chrome / Windows 设置的 geckodriver 名称)。你应该把它保存在你的机器里，就这样。例如，我的 geckodriver 放在我的 Windows 10 分区桌面上。这种错误比较常见，这也是我在这里提到它的原因。我自己已经深受这个问题的困扰。请记住您保存 geckodriver 文件的位置，因为您将需要在后面的代码中重现它的绝对路径。

首次代码修改

下面，我将展示与我上一篇文章中相同的程序部分，以及我们现在需要的额外的导入语句。还需要强调的是，从现在开始，旧的 open_google_trans()函数将被称为 search_google_trans()。如果您已经阅读了另一篇文章，请跳过这段代码复制，继续前进。

现在，我们将使用 Selenium 打开 Google Translate 格式化的 URL，它保存在 link 变量中。所以，我们需要创建一个 Chrome 浏览器实例，并在这个实例上调用 get()方法。我们还会要求程序暂停 15 秒钟。下面的代码行就是这样做的。请记住使用保存在您机器中的 geckodriver 文件路径来更改 executable_path 参数值。

该程序将打开谷歌翻译网站，显示原文和相应的翻译成选定的语言。

现在，我们将在页面上找到 copy translation 按钮，并使用 Selenium 单击它，以便将翻译复制到剪贴板(也安装下面的库: *pip install pyperclip* )。接下来，我们将把这个翻译粘贴到一个变量中，并将这个变量值保存到一个. txt 输出文件中。所有这些步骤都由下面的代码执行:

尽管我的整个程序在测试期间运行顺利，但在使用 Selenium 时编写错误处理代码总是一个好主意，因为在运行 web 抓取时可能会频繁出现问题。在这种情况下，亲爱的读者，如果你认为有必要的话，我把做出这种改进的挑战留给你。

创建两个辅助功能

可以通过隔离一些可能构成独立函数的部分来改进 search_google_trans()函数代码。在这里，两个代码部分是突出的潜在候选部分:一方面是输入类型检查，另一方面是生成输出文件并将其保存在正确文件夹中的过程。

因此，我们将创建函数 check_input_type()和 save_output_as_txt()，然后在 search_google_trans()函数中调用它们。此外，search_google_trans()参数序列将被更改，以方便将来的功能实现，同时还会添加一些新参数，以便我们以后使用。这种代码更改摘录如下:

翻译超过 5K 个字符的文本

现在，我们将转到代码实现部分，它自动翻译超过 5 千个字符的. txt 文本，这可能对一些用户是一个有用的资源。为了做到这一点，我们需要创建一个系统来将文本分成更小的单元，然后调用 search_google_translate()函数来翻译这些文本块。因此，我们在下面给出了函数 translate_any_text:

在下面的链接中，你会找到完整的网络抓取代码。我不知道你，但我花了相当多的时间看日语和韩语的最终翻译，被这些语言的美丽所震惊。也许有一天我会有勇气学一点。谁知道呢！

[https://gist . github . com/fabric ius 1/fcf4e 630938 FD 4d 790 a9 bbacb 75 FB 6 f 3](https://gist.github.com/fabricius1/fcf4e630938fd4d790a9bbacb75fb6f3)

非常感谢您阅读我的文章。

*快乐编码*！

附注:你会在 LinkedIn、Medium 和 Github 上找到更多关于我工作的信息:

https://www.linkedin.com/in/fabriciobrasil

【https://fabriciusbr.medium.com/ 

[https://github.com/fabricius1](https://github.com/fabricius1)