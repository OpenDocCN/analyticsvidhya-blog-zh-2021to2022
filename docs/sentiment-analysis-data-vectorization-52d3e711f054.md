# 情感分析、数据矢量化

> 原文：<https://medium.com/analytics-vidhya/sentiment-analysis-data-vectorization-52d3e711f054?source=collection_archive---------9----------------------->

## 文本到矢量

![](img/34c1fb48a8ca03b046e3e4434e59cf86.png)

## 介绍

你有没有想过机器是如何读取数据的？他们是怎么理解的？他们不像人类那样理解文本，但是他们擅长数学运算。这就是为什么我们要把文本转换成数字。有很多方法可以做到这一点。我强烈推荐你读一读 Paritosh Pantola 的文章。他详尽地描述了这个话题。但为了公平起见，我将从我的网站上补充几句:)。

[](/@paritosh_30025/natural-language-processing-text-data-vectorization-af2520529cf7) [## 自然语言处理:文本数据矢量化

### 机器学习的特点基本上是数字属性，任何人都可以从中执行一些数学…

medium.com](/@paritosh_30025/natural-language-processing-text-data-vectorization-af2520529cf7) 

这篇文章是我介绍情感分析项目的第二部分。如果你没有看过第一部分，请查看下面的链接。我们讨论了数据预处理，并创建了开始处理它的所有必要步骤。

[](https://wiktorowski-dev.medium.com/sentiment-analysis-data-processing-part-1-c4387a8f3bc2) [## 情感分析、数据处理

### 处理数据—第 1 部分

wiktorowski-dev.medium.com](https://wiktorowski-dev.medium.com/sentiment-analysis-data-processing-part-1-c4387a8f3bc2) 

为了这个项目的需要，我创建了一个专门的类来准备整个步骤。另外，我用手套来嵌入单词。在简短的描述中，我们将使用所有手套数据集作为我们在嵌入层中的输入权重，但更准确地说，我们将讨论一个代码示例。然而，什么是[手套](https://nlp.stanford.edu/projects/glove/)？老实说，对你来说最好的是阅读一些广泛的文章，如[这篇](https://towardsdatascience.com/light-on-math-ml-intuitive-guide-to-understanding-glove-embeddings-b13b4f19c010)，这将有助于你复杂地理解它是如何工作的。

不过，我可以建议你去看看这个网站。它向你展示了单词嵌入的 3D 模型，这里你有一些关于它的文章。

[](https://ai.googleblog.com/2016/12/open-sourcing-embedding-projector-tool.html) [## 开源嵌入式投影仪:一个可视化高维数据的工具

### 机器学习(ML)的最新进展显示了令人印象深刻的结果，其应用范围从图像…

ai.googleblog.com](https://ai.googleblog.com/2016/12/open-sourcing-embedding-projector-tool.html) 

## 库和设置

在我们的数据矢量化项目中，我们将使用来自[*tensor flow*](https://www.tensorflow.org/)*库中的标记器，但是对于一些预处理操作，我们将使用来自 [*scikit-learn*](https://scikit-learn.org/stable/) 的单个函数。当然，我们不能忘记处理数字的最好的库之一— [NumPy](https://numpy.org/) 和 [P *和 as*](https://pandas.pydata.org/) 用于读取我们先前创建的。csv 文件。*

## *功能概述*

*现在我们可以专注于代码方面。正如我之前所说的，我创建了一个类来使矢量化变得更加容易。我们可以这样声明:*

*如你所见，这有点复杂，但是相信我，很容易处理。*

```
*data_set_text -> Input list of sentences
data_Set_tags_array -> Input np.array with tags, positive/negative
test_size -> Ratio test/train sentences
max_length_output -> Maximum size of output vector sentence
num_words -> Maximum number of words for Tokenizer to deal with, we will discuss it later
random_state -> Random seed to shuffle input data to train/test
glove_path -> Path to the GloVe file
process_glove -> Declaration about processing the GloVe
*embedding_matrix_file_name* -> Path for embedding matrix file
embedding_key_to_val_file_name -> Path for embedding key to value file
save_glove_post_processing_files -> Declaration to save or no, post 
GloVe process output*
```

*如果你已经熟悉了声明和输入，我们可以谈谈函数的主体。开始时，我们有一个简单的手套文件操作检查器。如果某些东西不正确，它将引发错误。*

## *数据处理*

*现在我们可以开始处理数据了。首先，我们将把输入数据集分为训练集和测试集。*

*由于我们已经有了分离的数据，我们必须设置记号赋予器和*【feed】*him。首先，我们通过设置 num_words 变量创建一个 tokenizer 对象。现在我们有很好的机会近距离观察它。基于词频，[*num _ words*](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/text/Tokenizer)*变量被声明为分词器要保留的最大字数。换句话说，它是通过删减冷门词汇来实现的。**

**例如，假设我们将 num_word 设置为 3。我们的输入文本看起来像这样:“狗狗猫猫天气”，我们可以看到有两个不同的单词重复，“狗”和“猫”。这个输入*句子*将在输出上给我们这样的内容:' *[2，2，1，1，1]'。*我们可以假设 2 是狗，1 是‘猫’，其他的词都去掉了，因为它们的指数≥ 3。**

**下面是如何在分词器中查看 *word_index* 的示例。**

```
**{'cat': 1,
 'dog': 2,
 'is': 3,
 'great': 4,
 'the': 5,
 'weather': 6,
 'my': 7,
 'and': 8,
 'your': 9,
 'also': 10}**
```

**如果我们有了分词器，我们就必须用我们的文本*“喂”他。这将在他自己创造词频计数和索引。***

***现在最神奇的事情之一是，把单词变成数字。我们只用一个函数来改变它。请注意，分词器会删除出现频率太低的单词。***

***这意味着我们必须记住它是基于词频指数的。也就是说，如果我们继续它一次，并且我们将输入权重的嵌入设置到我们的模型中，我们不能再次覆盖它们。也就是说，如果我们对文本使用不同的数据集，它可能包含不同的词频。例如，在我们的第一个数据集中，我们有单词“pony ”,这个单词的索引为 152，在第二个数据集中，我们有相同的单词，但索引为 921(因为出现的频率较低),它为第一个数据集中的上述单词获得了不同的向量。***

***为了防止模型出现这种情况，我们将使用整个手套文件，就像我们的嵌入矩阵一样。这使得我们可以通用这个模型。在我们将 tokenizer 整数转换为基于手套的整数之前，我们有几个步骤要做。首先，我们必须将句子的长度设置为输入层的大小，在我们的例子中是 *100。*我们将使用 [*scikit-learn*](https://scikit-learn.org/stable/) *中的这个函数。我们应该在这里申报三样东西。我们的输入向量列表，在这里我们添加零，在句末添加“post ”,输出长度。****

## **手套准备**

**如果我们已经准备好数据矢量化项目中的数据处理，我们就可以开始准备手套了。当我们第一次这样做时，我们必须创建嵌入矩阵来插入到模型中，但是对于以后的步骤，我们不再需要它了。这就是为什么我们可以把它保存到我们的本地存储，然后我们有可能加载它，而不需要不必要的处理。**

**它的工作方式很简单，它读取 GloVe 文件并遍历它。每一行都保存为向量，并作为嵌入矩阵中索引的关键等价物。等价键将为我们提供将记号赋予器整数转换为手套整数的基础。在这个函数的末尾，如果我们正确地设置了一个变量，它将把我们的输出保存在声明的路径中。**

**现在是非常重要的一步，把我们的 tokenizer 整数换成 GloVe 整数。即使在这里，我们也没有什么复杂的东西。我们正在迭代输入数据中的每个句子和句子中的每个单词，因为我们必须从分词器中获取单词名称。之后，如果我们有了这个带字母的单词，我们就可以从等价键中得到适合这个向量的索引。**

**为了美好的结局。我们必须得到最大整数索引+ 1 的输入维度。**

## **结论**

**我希望在本文中，您能获得一些关于矢量化的有用信息。如果我们知道如何使用每种工具，这并不复杂。在下一篇文章中，我们将谈论学习过程，讨论一些模型等。一如既往，请留下您的反馈，包括正面和负面的。希望很快能见到你:)。**

**![](img/b1c629a277e8e8242b930f0cc2a58ad4.png)**

**[Github](https://github.com/wiktorowski-dev?tab=repositories)**

**[*我的网站*](https://wiktorowski.dev)[*本项目在 Github 上*](https://github.com/wiktorowski-dev/Sentiment-Analysis-project)**