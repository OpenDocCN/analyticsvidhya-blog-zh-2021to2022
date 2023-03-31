# 用 Python 中的 NLTK 库删除停用词

> 原文：<https://medium.com/analytics-vidhya/removing-stop-words-with-nltk-library-in-python-f33f53556cc1?source=collection_archive---------0----------------------->

![](img/0b2e6a49c223c507598f552d6b16680d.png)

由[桑切斯·阿梅斯夸](https://unsplash.com/@samchezamezcua?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 介绍

在 NLP 中处理文本数据时，我们通常需要在执行主要任务之前对数据进行预处理。

我们采取的一个常见的预处理步骤是删除停用词，这就是我将在本文中向您展示的内容。

我们开始吧。

# 到底什么是停用词？

**停用词**是任何语言或语料库中频繁出现的词。对于某些 NLP 任务，它们不会为包含它们的文本提供任何额外的或有价值的信息。像*一个*，*他们*，*这个*，*就是*，*一个*等等。通常被认为是停用词。

我们以这篇文章的标题为例:

> *如何用 Python 中的 NLTK 库移除停用词*

像*如何*、*到*、*带*、中的*等词语，并没有明确表述文章的主题。然而，像 *remove* 、 *stop words* 、 *NLTK* 、 *library* 和 *Python* 这样的关键字，可以更清楚地了解本文的内容。*

有趣的是，其中一些关键字是本文标签的一部分:)

# 删除停用词

虽然 NLP 中没有通用的停用词列表，但 Python 中的许多 NLP 库都提供了它们的列表。我们也可以决定创建自己的停用词列表。

这里我们将使用 NLTK 库提供的停用词列表，所以我们不必自己编写。

然而，在我们可以使用 NLTK 库中的这些停用词之前，我们需要先下载它。

```
import nltknltk.download('stopwords')
```

接下来，我们将文本转换成小写，并将其拆分成单词列表。然后，我们创建一个新的列表，包含不在停用词列表中的词。

```
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize# Add text
text = "How to remove stop words with NLTK library in Python"
print("Text:", text)# Convert text to lowercase and split to a list of words
tokens = word_tokenize(text.lower())
print("Tokens:", tokens)# Remove stop words
english_stopwords = stopwords.words('english')
tokens_wo_stopwords = [t for t in tokens if t not in english_stopwords]
print("Text without stop words:", " ".join(tokens_wo_stopwords))
```

输出应该是这样的:

```
Text: "How to remove stop words with NLTK library in Python"
Tokens: ['how', 'to', 'remove', 'stop', 'words', 'with', 'nltk', 'library', 'in', 'python']
Text without stop words: "remove stop words nltk library python"
```

# 专门从事

有时，您可能需要在停用字词列表中添加或删除字词。

例如，假设你正试图根据关注的食物种类对食物杂志进行分类。现在，你可能会想到*食物*这个词(或者类似的词)会被提到很多次。这些*不会提供有价值的信息*。

因此，*食物*是一个停用词，你可以考虑把它添加到你的停用词列表中。

幸运的是，`stopwords.words('english')`返回了一个常规的 Python 列表，我们可以很容易地修改它。请记住，这不会改变您下载到磁盘的停用字词。

```
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize# It returns a regular Python list
english_stopwords = stopwords.words('english')# Add a list of words
english_stopwords.extend(['food', 'meal', 'eat'])# Add a single word
english_stopwords.append('plate')# Remove a single word
english_stopwords.remove('not')
```

# 不仅仅是英语

NLTK 的停用词语料库的一个令人兴奋的事情是，有 16 种不同语言的停用词。

我们可以获得可用语言的列表，并如下所示使用它们。

```
from nltk.corpus import stopwords# Print the list of available languages
print(stopwords.fileids())# Use any of the available languages
french_stopwords = stopwords.words('french')
spanish_stopwords = stopwords.words('spanish')
italian_stopwords = stopwords.words('italian')
```

# 警告

虽然删除停用词听起来是个好主意，但有时并不可取。对于像文本分类、聚类、推荐系统这样的任务，删除停用词可能会很好，因为它们可能对确定模型的输出没有太大贡献。

其他任务，如机器翻译、文本摘要、问答系统都是不可取的例子，可能会损害模型的性能。

# 结论

本文到此为止。希望现在你已经很好地理解了什么是停用词，如何移除它们，以及何时移除它们。

感谢阅读！