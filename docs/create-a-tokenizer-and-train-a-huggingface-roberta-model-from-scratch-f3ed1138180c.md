# 创建一个分词器，从头开始训练一个 Huggingface RoBERTa 模型

> 原文：<https://medium.com/analytics-vidhya/create-a-tokenizer-and-train-a-huggingface-roberta-model-from-scratch-f3ed1138180c?source=collection_archive---------1----------------------->

![](img/429c47837c3ff0a6d7a50843781067cf.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/learn-scratch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 简介

这篇博文是我们希望使用 transformer 模型创建产品名称生成器的系列文章的第一部分。有几个星期，我在 Huggingface 中研究不同的模型和替代方案，以训练一个文本生成模型。我们有一个产品及其描述的候选名单，我们的目标是获得产品的名称。我用 Tensorflow 中的 Transformer 模型和 T5 摘要器做了一些实验。最后，为了深化 Huggingface transformers 的使用，我决定用一种更复杂的方法来解决这个问题，一种编码器-解码器模型。也许这不是最好的选择，但我想学习拥抱变形金刚的新知识。在本系列的下一篇文章中，我们将向您更深入地介绍这个概念。

在第一部分中，我们将展示如何从头开始训练一个标记器，以及如何使用屏蔽语言建模技术来创建一个 RoBERTa 模型。这个个性化模型将成为我们未来编码器-解码器模型的基础模型。

# 我们自己的解决方案

在我们的实验中，我们将从头开始训练一个 RoBERTa 模型，它将成为未来模型的编码器和解码器。但是我们的领域是非常具体的，关于衣服、形状、颜色的单词和概念……因此，我们感兴趣的是从我们的特定词汇中定义我们自己的标记器，避免包括来自其他领域或用例的与我们的最终目的无关的更常见的单词。

*我们可以用三个主要步骤来描述我们的培训阶段*:

*   使用与 RoBERTa 相同的特殊标记创建并训练一个字节级的字节对编码标记器
*   使用 MLM**屏蔽语言建模**从头开始训练一个**罗伯塔模型**。

代码可以在这个 [Github 库](https://github.com/edumunozsala/RoBERTa_Encoder_Decoder_Product_Names/blob/03c0456f03d8cff62e2d1b04f03029130694e18b/RoBERTa%20MLM%20and%20Tokenizer%20train%20for%20Text%20generation.ipynb)中找到。在这篇文章中，我们将只向你展示主要的代码部分和一些解释。

# 数据集

正如我们之前提到的，我们的数据集包含大约 31，000 个商品，涉及一家重要零售商的服装，包括一个长的产品描述和一个短的产品名称，这是我们的目标变量。首先，我们执行一个探索性的数据分析，我们可以观察到具有异常值的行的数量很少。单词数看起来像一个左偏分布，75%的行在 50-60 个单词的范围内，最多约 125 个单词。目标变量包含大约 3 到 6 个单词。

![](img/f5d5d7944880f7d9efd52c34b8c51b50.png)

照片由[波普&斑马](https://unsplash.com/@popnzebra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[挡泥板](https://unsplash.com/s/photos/token?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上拍摄

# 训练分词器

斯坦福 NLP 小组将标记化定义为:

> 给定一个字符序列和一个已定义的文档单元，标记化就是将它分割成若干部分，称为*标记*，也许同时丢弃某些字符，如标点符号

一个*记号分解器将一串字符，通常是文本的句子，分解成记号*，记号的整数表示，通常通过寻找空白(制表符、空格、换行符)。它通常将一个句子拆分成单词，但也有很多像子词这样的选项。

"*我们将使用一个* ***字节级字节对编码标记器*** *，字节对编码(BPE)是一种简单的数据压缩形式，其中最常见的一对连续数据字节被替换为一个不在该数据中出现的字节*，"[字节对编码](https://en.wikipedia.org/wiki/Byte_pair_encoding)"，维基百科定义。这种方法的好处是，它将从单个字符的字母表开始构建词汇，因此所有的单词都可以分解成记号。我们可以避免未知(UNK)令牌的出现。

在 Huggingface 文档[https://huggingface.co/transformers/tokenizer_summary.html](https://huggingface.co/transformers/tokenizer_summary.html)中可以找到对记号赋予器的很好的解释。

为了训练一个分词器，我们需要将数据集保存在一堆文本文件中。我们为每个描述值创建一个纯文本文件。

现在，我们可以在创建的文本文件上训练我们的标记器并包含我们的词汇表，我们需要指定词汇表的大小、要包含的标记的最小频率以及特殊标记。我们选择的 vocab 大小为 8，192，最小频率为 2(您可以根据自己的最大词汇量来调整这个值)。**特殊标志**取决于型号，对于 RoBERTa，我们提供了一个候选名单:

*   ~~或 BOS，句首~~
*   或 EOS，句子结束
*   <pad>填充令牌</pad>
*   <unk>未知令牌</unk>
*   <mask>掩蔽令牌。</mask>

样本数量很少，标记器训练非常快。现在我们可以将标记器保存到磁盘，稍后我们将使用它来训练语言模型。

我们现在有了一个`vocab.json`，它是一个按频率排列的最频繁的标记列表，用于将标记转换成 id，还有一个`merges.txt`文件，用于将文本映射成标记。

```
# vocab.json
{
    "<s>": 0,
    "<pad>": 1,
    "</s>": 2,
    "<unk>": 3,
    "<mask>": 4,
    "!": 5,
    "\"": 6,
    "#": 7,
    "$": 8,
    "%": 9,
    "&": 10,
    "'": 11,
    "(": 12,
    ")": 13,
    # ...
}

# merges.txt
l a
Ġ k
o n
Ġ la
t a
Ġ e
Ġ d
Ġ p
# ...
```

最棒的是，我们的标记器针对我们非常特殊的词汇进行了优化，而不是针对普通英语训练的通用标记器。

我们可以用这两个文件实例化我们的记号化器，并用数据集中的一些文本测试它。

稍后，当训练模型时，我们将使用`from_pretrained`方法初始化记号赋予器。

![](img/ffd883349f80168bd4ddf04fd7a683dc.png)

由 [Jason Leung](https://unsplash.com/@ninjason?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/language?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 从头开始训练语言模型

我们将训练一个 **RoBERTa 模型**，它类似于 BERT，有一些变化(查看[文档](https://huggingface.co/transformers/model_doc/roberta.html)了解更多细节)。简而言之:*“它建立在 BERT 的基础上，并修改了关键的超参数，删除了下一句话的预训练目标，并以更大的小批量和学习率进行训练”，*hugging face documentation on RoBERTa*。*

由于该模型类似于 BERT，我们将在 ***掩蔽语言建模*** 的任务上训练它。它涉及屏蔽部分输入，大约 10–20%的标记，然后学习一个模型来预测丢失的标记。MLM 经常在预训练任务中使用，以**给模型从未标记数据中学习文本模式的机会**。它可以针对特定的下游任务进行微调。主要的好处是我们不需要带标签的数据(很难获得)，为了预测丢失的值，不需要人工标注文本。

我们将从头开始训练这个模型，而不是从一个预先训练好的模型开始。我们为 RoBERTa 模型创建一个模型配置，设置主要参数:

*   词汇量
*   注意头
*   隐藏层

最后，*让我们使用配置文件*初始化我们的模型。由于我们是从零开始训练，我们从定义模型架构的配置开始初始化，但不恢复先前训练的权重。权重将被随机初始化。

我们使用上一步中训练和保存的记号赋予器重新创建记号赋予器。我们将使用一个`RoBERTaTokenizerFast`对象和`from_pretrained`方法来初始化我们的记号赋予器。

## 构建训练数据集

我们将构建一个 Pytorch 数据集，子类化 dataset 类。`CustomDataset`接收带有`description`变量值的 Pandas 系列和对这些值进行编码的标记器。数据集为系列中的每个产品说明返回一个令牌列表。

为了在训练期间评估模型，我们将生成训练数据集和评估数据集。

一旦我们有了数据集，一个**数据整理器将帮助我们屏蔽我们的训练文本**。这只是一个小助手，它将帮助我们将数据集的不同样本一起批处理到 PyTorch 知道如何对其执行反向投影的对象中。数据排序规则是通过使用数据集元素列表作为输入来形成一个批处理的对象，并且可以应用一些处理，如填充或随机屏蔽。`DataCollatorForLanguageModeling`方法允许我们设置在输入中随机屏蔽标记的概率。

# 初始化并培训我们的培训师

当我们想要训练一个 transformer 模型时，基本的方法是创建一个 Trainer 类，它提供一个用于功能完整训练的 API，并且包含基本的训练循环。首先，我们定义培训参数，它们有很多，但更相关的是:

*   `output_dir`保存模型工件的位置
*   `evaluation_strategy`何时计算验证损失
*   `num_train_epochs`
*   `per_device_train_batch_size`是训练的批量
*   `per_device_eval_batch_size`是评估的批量
*   `learning_rate`，初始化为 1e-4
*   `weight_decay`，0.01

最后，我们使用参数、输入数据集、评估数据集和定义的数据排序器创建一个`Trainer`对象。现在我们准备训练我们的模型。

因此，我们可以在训练时观察损失是如何减少的。

最后，我们保存模型和记号赋予器，以便它们可以被恢复用于未来的下游任务，我们的编码器-解码器模型。

# 使用管道检查已训练的模型

看着训练和评估损失下降是不够的，我们想应用我们的模型来检查我们的语言模型是否正在学习任何有趣的东西。一个简单的方法是通过`FillMaskPipeline`。

管道是标记化器和模型的简单包装器。**我们可以使用“填充-屏蔽”管道**，在这里我们输入一个包含屏蔽标记(`<mask>`)的序列，它返回一个最可能的填充序列列表，以及它们的概率。但只对只有一个标记被屏蔽的输入有效(见[拥抱脸官方文档](https://huggingface.co/transformers/main_classes/pipelines.html))。

这就是第一部分的全部内容，我希望你已经为你未来的项目找到了灵感，我希望几天后我们会带着第二部分回来。

代码可以在这个 [Github 库](https://github.com/edumunozsala/RoBERTa_Encoder_Decoder_Product_Names/blob/03c0456f03d8cff62e2d1b04f03029130694e18b/RoBERTa%20MLM%20and%20Tokenizer%20train%20for%20Text%20generation.ipynb)中找到。

# 参考

2020 年 2 月，“如何使用变形器和标记器从零开始训练一个新的语言模型”，Huggingface 博客。

“[编码器-解码器型号](https://huggingface.co/transformers/model_doc/encoderdecoder.html)”，Huggingface 官方文档

来自 Huggingface 的 [RoBERTa 文档](https://huggingface.co/transformers/model_doc/roberta.html)

[字节对编码](https://en.wikipedia.org/wiki/Byte_pair_encoding)，维基百科定义。