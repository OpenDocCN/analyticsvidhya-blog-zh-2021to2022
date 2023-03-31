# 文本的数据扩充[带代码]

> 原文：<https://medium.com/analytics-vidhya/data-augmentation-for-text-with-code-6da46aad443d?source=collection_archive---------3----------------------->

![](img/673a08f2dec8b295cd7891fed8f74fa5.png)

图片来自 [unsplash](https://unsplash.com/@randyfath)

本文将展示如何在 PyTorch 中编码，数据增强技术用于深度学习问题，如文本分类、文本生成等。

对于与图像相关的数据增强问题，可以在这里 找到实现 [**。**](/analytics-vidhya/learn-to-code-in-tensorflow2-fe735ad46826?source=friends_link&sk=e112de37ddacbf54982daf7eb803f1fe)

# 数据扩充技术

## 1.回译

## 2.随机插入

## 3.随机删除

## 4.随机交换

**注**:文本的数据扩充是一个代价很高的操作，如果我们试图在训练循环中使用它，将会显著增加训练时间。与图像不同，对于文本增强，首先，我们需要替换单词，然后对该单词使用相关单词嵌入。因此，我们将事先创建一个增强，并在训练中使用它。

我已经为英语做了数据扩充，但是这些技术是语言不可知的。

# 回译

这个想法是让不同的句子表达相同的意思来进行训练。

**第一步**:选择英文句子。

```
The Best Way To Get Started Is To Quit Talking And Begin Doing.
```

**步骤 2** :选择一种随机语言(朝鲜语)，并使用语言翻译将句子转换成该语言。

```
시작하는 가장 좋은 방법은 말하기를 그만두고 시작하는 것입니다.
```

**第三步:**现在把它翻译回英文。

```
The best way to start is to stop talking and start.
```

现在，你可以看到你的句子意思相同，但结构不同。

为了实现这一点，我们不需要训练语言翻译模型，但我们可以利用**谷歌翻译**。

让我们安装所需的库:

```
sudo pip install google_trans_new
```

让我们编码:

请记住，在使用谷歌翻译 API 时，你可以转换的句子数量是有限制的。所以有 2 种处理方式:
1。获取 API
2 的付费订阅。多次对数据集进行数据扩充。比方说如果 800 个电话是极限，那么就对 800 个句子进行数据扩充并保存它。过一段时间后，再对接下来的 800 个句子进行扩充，以此类推。

# 随机插入

这里的想法是找到句子中的非停用词，并随机用它们的同义词替换几个词。

**第一步**:选择英文句子。

```
The Best Way To Get Started Is To Quit Talking And Begin Doing.
```

第二步:过滤掉停用词。

```
["Best", "Way", "Started", "Quit", "Talking", "Begin", "Doing"]
```

**第三步**:随机选择 3 个单词，找出它们最接近的同义词。

```
words = ["Way", "Quit", "Talking"]
synonyms = ["Method", "Leave", "Speaking"]
```

**第四步**:替换原句中的那些词。

```
The Best **Method** To Get Started Is To **Leave** **Speaking** And Begin Doing.
```

为了实现这一点，我们可以使用现有的预训练单词嵌入。然而，为了获得更好的结果，您应该使用定制的单词嵌入或语言模型。

下载并提取 300 维的 Google word2vec。

```
wget -c "https://s3.amazonaws.com/dl4j-distribution/GoogleNews-vectors-negative300.bin.gz"
gzip -d GoogleNews-vectors-negative300.bin.gz
```

让我们编码:

这可以一次性完成，但仍需要时间来生成所有数据集。

# 随机删除

这里的想法是从句子中随机删除几个单词。

**第一步**:选择一个句子

```
The Best Way To Get Started Is To Quit Talking And Begin Doing.
```

**第二步**:给所有单词分配随机概率

```
0.3 The 
**0.2 Best** 0.3 Way 
0.8 To 
0.8 Get 
0.3 Started 
0.7 Is 
**0.2 To** 0.3 Quit 
0.9 Talking 
0.4 And 
0.4 Begin 
0.9 Doing.
```

**第三步**:去掉所有得分低于 0.3 的单词

```
The Way To Get Started Is Quit Talking And Begin Doing.
```

让我们编码:

# 随机交换

这里的想法是一次随机选择两个单词，并交换它们在句子中的位置。这可以做 n 次。

**第一步**:选择句子

```
The Best Way To Get Started Is To Quit Talking And Begin Doing.
```

**第二步**:选择单词并交换

```
The Started Way To Get Quit Is To Best Talking And Begin Doing.
```

让我们编码:

一旦我们准备好了所有的数据，我们就可以开始训练了。上面定义的所有方法都是通用方法。要将其应用于数据集，首先需要了解数据集，然后应用定制的增强技术。

这些技术也可以混合使用。