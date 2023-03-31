# 如何处理数据集中的类不平衡

> 原文：<https://medium.com/analytics-vidhya/how-to-deal-with-class-imbalance-in-dataset-a8e1c957602b?source=collection_archive---------16----------------------->

处理数据不平衡的简单技巧

![](img/fc342d7d9f53bc98c0dc40fa9db197aa.png)

吉尔特·皮耶特斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**什么是阶层失衡**

类别不平衡是机器学习任务中数据的不平等分布和变化，其中一个类别往往比其他类别或分布具有更多的值。由于从单个地理区域收集数据而导致的数据的不一致和有偏差的重新采样，或者由于问题的性质，例如欺诈交易，由于在整个交易中欺诈的发生和流行较少，欺诈交易倾向于在类别之间具有有偏差的数据分布，因此可能出现类别不平衡。不平衡的出现也是因为少数民族阶层的存在，如种族、民族或部落。

因此，让我们从上传数据集开始，然后打印**性别**列的值计数。

```
df=pd.read_csv('titanic_train.csv')
df.Sex.value_counts()
```

输出[1]:

```
male      577
female    314
Name: Sex, dtype: int64
```

在上面的输出中，由于数据的不平衡，男性类比女性类有更多的值。

**处理数据集中的类不平衡**
1-对少数类进行过采样
2-对多数类进行欠采样

**对少数类进行过采样**
在过采样中，我们为少数类创建了更多的合成数据，以便少数类有更多的数据来匹配多数类，如果要处理的数据较少，这是值得推荐的。

在[ 1]中:

```
from sklearn.utils import resample
Male=df[df.Sex=='male']
Female=df[df.Sex=='female']female_upsampled = resample(Female,
 replace = True,
 n_samples = len(Male), # Match the number  with the majority class
 random_state=20)df = pd.concat([Male, female_upsampled])
```

现在让我们看看这个柱子

```
df.Sex.value_counts()
```

输出[2]:

```
female    577
male      577
Name: Sex, dtype: int64
```

现在，女性和男性阶层的人数相等。

**欠采样多数类**
在这种采样方法中，我们从多数类中移除一些数据以匹配少数类，但如果要处理的数据较少，这可能会影响模型的性能。

```
male_downsampled = resample(Male,
 replace = True, # Sample with replacement
 n_samples = len(Female), # Match number with the minority class
 random_state=20)df = pd.concat([women, men_downsampled])
```

让我们现在也看看**性别**一栏

```
df.Sex.value_counts()
```

输出[2]:

```
female    314
male      314
Name: Sex, dtype: int64
```

**结论**

过采样和欠采样方法是处理数据不平衡的非常常见和有效的技术之一，但也可以使用其他技术，如合成少数过采样技术(SMOTH)。

[https://alaminmusamagaga . medium . com/simple-way-to-create-a-machine-learning-app-with flask-69a 532663 FD 5](https://alaminmusamagaga.medium.com/simple-way-to-create-a-machine-learning-app-with-flask-69a532663fd5)

[https://medium . com/analytics-vid hya/natural-language-processing-NLP-and-process-modeling-in-precision-medicine-a 55 fa 9 EC 9818](/analytics-vidhya/natural-language-processing-nlp-and-process-modeling-in-precision-medicine-a55fa9ec9818)