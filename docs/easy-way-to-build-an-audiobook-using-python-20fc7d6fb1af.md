# 使用 Python 构建有声读物的简单方法

> 原文：<https://medium.com/analytics-vidhya/easy-way-to-build-an-audiobook-using-python-20fc7d6fb1af?source=collection_archive---------11----------------------->

![](img/498e103daa7b666187f0bdf3f8ea8543.png)

Lena Kudryavtseva 在 [Unsplash](https://unsplash.com/s/photos/audiobook?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

有声读物只不过是以音频格式录制的书。也可以说是一本正在大声朗读的书。有声读物有助于即兴发挥一个人的词汇，理解和单词发音。希望你们中的一些人是书迷，但是懒得自己去读。所以是时候使用 Python 的少量代码来创建您的有声读物了。这将使你享受有声读物，而不需要任何订阅费，就像 Audible，Scribd 等平台一样。

# 先决条件

Python 有大量包含可重用代码的模块，这些代码在被调用时执行所需的功能。在使用其功能之前，必须将其安装到您的系统中。所以在这里，我们将从大量流行的 python 模块中选取 2 个模块。

*   安装 PyPDF2 模块，使用

```
*pip install PyPDF2*
```

*   安装 pyttsx3 模块，使用

```
*pip install pyttsx3*
```

# PyPDF2 模块

它是一个纯 Python 库，可以在任何 Python 平台上运行，没有任何外部依赖性。它完全在 StringIO 上工作，而不是使用文件流。PyPDF2 模块对 PDF 文件执行各种操作。PyPDF2 模块可以执行以下一些任务，

*   获取文档信息，如标题、作者等。
*   逐页拆分和合并文档。
*   将多页合并成一页。
*   将页面裁剪到所需的比例。
*   PDF 文件的加密和解密。

# pyttsx3 模块

它是 python 中最流行的模块之一，将文本作为输入，将语音/音频作为输出。甚至这个 pyttsx3 模块也可以脱机工作，证明它是一个比其他模块更友好的模块。它与 Python 版本 2 以及 Python 版本 3 兼容。

# 导入模块

必须调用上述模块才能在程序中使用它们的功能。使用 import 语句导入模块，例如

```
*import PyPDF2**import pyttsx3*
```

# 阅读 PDF 文件

必须先打开 PDF 文件才能操作其内容。pdf 文件可以读/写，可以嵌入附件，在 PDF 文件中添加书签等等。使用 Python 可以对 pdf 文件执行各种操作。我们可以从 PDF 文件中检索一些有用的信息，如页数、正在使用的页面的布局，可以借助页码检索页面等等。Python 中的 PyPDF2 模块继承了所有这些操作。

要读取 PDF 文件，请使用命令，

```
*variable_name = PyPDF2.PdfFileReader(open(‘file_name’,’rb’))*
```

其中变量名称→变量的名称

PdfFileReader()→py pdf 2 模块下的类。

open( ) →用于打开文件的函数。

file_name →需要打开的文件的名称。

rb →文件模式(以二进制格式打开文件进行读取)

# 初始化扬声器

接下来必须初始化扬声器，以便我们可以使用 pyttsx3 模块将文本转换为音频格式。使用命令，

```
*speaker=pyttsx3.init()*
```

初始化扬声器。

# 提取文本

首先也是最重要的是借助页码获取页面。所需页面的页码作为参数传递给 getPage()方法。

该命令可以写成，

```
*text=readpdf.getPage(pagenumber).extractText()*
```

其中 getPage(pagenumber) →借助页码检索页面

extractText() →从指定页面提取文本。

# 文本到音频

现在是时候将所有提取的文本作为参数传递给 pyttsx3 模块中名为 say()的方法了，该方法有助于将文本转换为音频格式。该命令如下所示，

```
*speaker.say(text)*
```

其中在前一个命令中提取的文本作为参数传递给 say()方法。

# 将语音保存到文件中

上述命令生成的声音可以保存到 mp3 文件中。该文件将被保存在我们保存代码的确切位置。因此，保存音频文件将有助于用户在将来访问它。

将语音保存到文件的命令如下:

```
*speaker.save_to_file(text,’filename.mp3')*
```

其中 speaker →已经初始化的变量。

save_to_file( text，' filename.mp3') →用于保存音频文件的方法。

这里有书迷吗？？？？？？评论你的名字和你想把它做成有声读物的书。是时候自己建立一本有声读物了！！！

# 代码片段

*原载于 2021 年 10 月 4 日*[*【https://gettoknowpython.blogspot.com】*](https://gettoknowpython.blogspot.com/2021/10/easy-way-to-build-audiobook-using-python.html)*。*