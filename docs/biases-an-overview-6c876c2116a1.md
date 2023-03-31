# 偏见:概述

> 原文：<https://medium.com/analytics-vidhya/biases-an-overview-6c876c2116a1?source=collection_archive---------0----------------------->

人类在本质上是有偏见的。怎么会？？？

这是我们长期培养的一部分。阅读下面的习语

```
**Blood Is Thicker Than Water**
```

这种说法最早的记录可以追溯到**12 世纪的德国**。

尽管我喜欢把它写成一篇社会研究文章，但它是一篇技术文章: )

上面的习语只是散布在人类本性中使他们有偏见的众多例子之一。但是当这种习惯偏离了我们实验的结果，它就阻碍了我们实验寻求真理的目标。

为了避免偏见，第一步是意识到可能发生的偏见。以下是一些最常见的偏见。

# 报道偏差

当数据集中只捕获了结果的子集时，就会出现报告偏差。这可能是由于取样者的某些偏好或他们的数据收集可能已经有偏差。报告偏差的各种类型有:

*   **发表偏倚:**这种偏倚发生在只有当作者认为研究成果值得发表时才发表的时候。这往往会导致过度夸大结果，或者为了更有利的结果而歪曲实验。简而言之，具有积极/值得注意的发现的研究比那些具有消极或无关紧要的发现的研究更有可能被发表。
*   **引用偏倚:**这种偏倚是发表偏倚的涓滴效应的结果。如果有人将他们的实验建立在已经有偏见的出版物上，这可能会导致结果的偏斜。
*   **重复发表偏倚:**当发表较多的研究比发表较少的研究具有更高的偏好/重要性时，就会出现这种偏倚。这是因为更多发表的研究被认为是相对较高的质量。
*   **位置偏差:**当某些研究比其他研究更容易获得时，就会出现这种偏差。这也是发表偏倚的涓滴效应。与阴性或无效结果相比，具有统计学意义的结果平均发表在影响更大的期刊上。
*   **语言偏见:**当人们只考虑那些以自己能够理解的语言发表的研究时，就会产生这种偏见。
*   **结果报告偏倚:**当您发表结果的可能性不是基于其有效性，而是基于外部因素，如需要更具统计显著性的结果才能发表等，就会出现这种偏倚。

# 自动化偏差

自动化偏差的发生是因为人们倾向于偏爱自动化系统产生的结果，而不是非自动化系统。

```
**Example**: 
Software engineers working for a sprocket manufacturer were eager to deploy the new "groundbreaking" model they trained to identify tooth defects, until the factory supervisor pointed out that the model's precision and recall rates were both 15% lower than those of human inspectors.
```

# 选择偏差

当实验数据的分布不能反映真实世界的分布时，就会产生选择偏差。

选择偏差的类型:

*   **采样偏差**:当数据采集过程中没有发生随机选择时，就会出现这种偏差。

```
**Example**:
A model is trained to predict future sales of a new product based on phone surveys conducted with a sample of consumers who bought the product and with a sample of consumers who bought a competing product. Instead of randomly targeting consumers, the surveyor chose the first 200 consumers that responded to an email, who might have been more enthusiastic about the product than average purchasers.
```

*   **收敛偏差:**这种偏差发生在数据采样不当的时候。

```
**Example**: 
A model is trained to predict future sales of a new product based on phone surveys conducted with a sample of consumers who bought the product. Consumers who instead opted to buy a competing product were not surveyed, and as a result, this group of people was not represented in the training data.
```

*   **无回答偏倚:**当数据收集来源的一个子集由于他们的不参与而丢失时，这种偏倚就会发生。

```
**Example**: 
A model is trained to predict future sales of a new product based on phone surveys conducted with a sample of consumers who bought the product and with a sample of consumers who bought a competing product. Consumers who bought the competing product were 80% more likely to refuse to complete the survey, and their data was underrepresented in the sample.
```

# **隐性偏见**

当我们自己的假设阻碍了决策，导致偏好或厌恶某个结果时，隐性偏见就会发生。

隐性偏见的类型:

*   **确认偏差或实验者的偏差:**当模型构建者试图将他们模型的结果与他们预先存在的信念和假设相一致时，就会出现这种偏差。

```
**Example**: 
An engineer training a gesture-recognition model uses a [head shake](https://wikipedia.org/wiki/Head_shake) as a feature to indicate a person is communicating the word "no." However, in some regions of the world, a head shake actually signifies "yes".
```

# 群体归因偏差

当一个人根据自己的假设概括一个群体的属性/品质时，就会出现群体归因偏差

群体归因偏差的类型

*   **群体内偏见:**这种偏见发生在你更喜欢自己群体的成员而不是其他人的时候。这可能只是基于感知，或者你们可能有共同的兴趣。

```
**Example:**
An employer considers people passed from his alma matter superior in knowledge than those from other colleges. He puts this belief of his in hiring process.
```

*   **群体外偏见:**当你避开不属于你的其他群体的人时，就会产生这种偏见。

```
**Example:**
People’s predilection towards a certain academic preference for hiring in particular job roles instead of focusing on 
the candidates experience because they themselves have the said academic background.
```