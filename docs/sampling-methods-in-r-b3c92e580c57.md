# R 中的取样方法

> 原文：<https://medium.com/analytics-vidhya/sampling-methods-in-r-b3c92e580c57?source=collection_archive---------2----------------------->

![](img/bcda40a93ec200ff0e076d3707d83c46.png)

保罗·伯格梅尔在 [Unsplash](https://unsplash.com/s/photos/stadium-seats?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**什么是采样？**

假设我们有一个规模为 N 的总体，样本只不过是从该总体中提取的数据的子集。选择样本的过程称为抽样。

**为什么要采样？**

取样可用于以下任何两种情况

1.  当无法获得全部人口数据时

在这种情况下，我们可能不得不使用可用的数据样本来对整个总体进行推断。

2.当人口数据太大时

在这种情况下，我们可以使用以下任何一种抽样技术从总体中抽取样本，从而可以做出进一步的推断。

**注:**确保选择理想的样本很重要，因为不正确的样本可能导致与总体不相关的推断。

**采样方法:**

取样方法分为两大类

1.  概率抽样
2.  非概率抽样

**概率抽样:**

在这种抽样中，样本是随机选择的，每个样本都有一个已知的被选中概率。

概率抽样进一步分为

1.  简单随机抽样
2.  系统抽样
3.  分层抽样
4.  巢式抽样法

## **1。简单随机抽样:**

随机抽样是最流行和最常用的抽样方法之一。在简单的随机抽样中，总体中的每个案例都有相等的概率被抽样选中。

可以通过顺序标记所有病例并生成均匀随机数以从总体中选择病例来获得随机样本。

可以使用 sample()函数生成 R 中的简单随机样本，如下所示。

示例函数定义如下

样本(x，size，replace = FALSE，prob = NULL)

从数字 1 到 10 的大小为 10 的样本可以如下生成。

```
> sample(1:10,10)[1] 8 2 9 10 3 7 6 5 1 4
```

如上图所示，样本默认是不替换生成的，即曾经被抽样的项目不会再次被抽样。可以通过将替换参数设置为 TRUE 来创建替换样本，如下所示。

```
> sample(1:10, replace=T)[1] 8 8 8 9 3 5 3 2 10 7
```

正如我们在上面的示例中看到的，8 在示例中重复了三次，3 重复了两次。

理想情况下，在随机抽样中，所有项目被选中的概率相等(在上面的例子中 p = 0.1in)。但是在 sample()方法中，我们还可以为样本中要选择的项目分配概率。

假设我们有一个包含两个项目(红色、绿色)的列表，默认情况下，红色和绿色都有 50% (p=0.5)的机会被选中。假设我们在样本中需要更多的红色而不是绿色，这可以通过使用样本()中的“prob”参数来实现，如下所示

```
> sample(c(“red”,” green”),10,replace=T,prob=c(0.6,0.4))[1] “red” “red” “red” “red” “red” “red” “green” “red” “green”[10] “red”
```

如我们所见，样本中红色比绿色多，因为红色被选中的概率(p=0.6)比绿色(p=0.4)高。

## 2.系统取样:

系统抽样用于人口数据是有序列表或按时间排列的情况。例如，为了分析一家商店在所有星期天的平均销售额，可以通过选择一周中所有第 7 天(星期天)的平均销售额数据包括在样本中来使用系统抽样。

即，在系统抽样中，以固定的时间间隔从总体数据中选择个体。为了从大小为 p 的总体中创建大小为 n 的样本，将固定的区间(k)作为 p/n

即 k=p/n

即，对于规模为 1000 的人群，为了创建规模为 100 (1000/100)的样本，可以从任何随机起点选择每 10 个项目包括在样本中。

现在，让我们看看如何在 R 中创建一个系统样本，

**R 中的系统样本:**

为了在 R 中创建一个系统样本，使用了“教学采样”包的 S.SY()函数。

```
install.packages("TeachingSampling")  
library(TeachingSampling)
P <- c("Mon-8", "Tues-4", "Wed-4", "Thurs-6", "Fri-7","Sat-45","Sun-34","Mon-21", "Tues-11","Wed-34","Thurs-16","Fri-10","Sat-17","Sun-19")
#systematic sample from a population of 14 with every 2nd included from the populaion P
systematic_sample <- S.SY(14,2)
systematic_sample
P[systematic_sample]
> P[systematic_sample]
[1] "Mon-8"    "Wed-4"    "Fri-7"    "Sun-34"   "Tues-11"  "Thurs-16" "Sat-17"
```

使用上面的 R 代码，从包含 14 天期间内一周中所有日子的销售量的总体 P 中，我们创建了一个系统样本，其中仅包含隔日售出的销售量。

注:系统抽样比简单的随机抽样更容易实施。然而，在系统抽样中，并非每个项目都有同等的机会被选中，因此许多项目可能永远不会被选中。此外，如果总体具有周期性趋势，则系统样本的有效性取决于周期性区间和系统抽样区间之间的关系。

## 3.分层抽样:

在分层抽样中，根据一些最能描述整个人口的共同因素，如年龄、性别、收入等，将人口分成更小的子群。这样形成的群体被称为地层。

例如，为了分析男性和女性用户每天发送消息所花费的时间量，可以将阶层视为男性和女性用户，并且可以使用随机抽样来选择男性和女性阶层中的项目。

注:与随机抽样相比，分层抽样给出了精确的估计值，但最大的缺点是它需要了解人口的适当特征(其细节并不总是可用的)，并且很难决定根据哪些特征进行分层。

**R 中分层抽样:**

**使用 dplyr**

让我们看看如何使用 iris 数据集创建分层样本，每个物种有 3 个样本。

```
library(dplyr)
set.seed(1)
iris %>%
  group_by (Species) %>%
  sample_n(., 3)Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1          4.3         3.0          1.1         0.1     setosa
2          5.7         3.8          1.7         0.3     setosa
3          5.2         3.5          1.5         0.2     setosa
4          5.7         3.0          4.2         1.2 versicolor
5          5.2         2.7          3.9         1.4 versicolor
6          5.0         2.3          3.3         1.0 versicolor
7          6.5         3.0          5.2         2.0  virginica
8          6.4         2.8          5.6         2.2  virginica
9          7.4         2.8          6.1         1.9  virginica
```

## 使用 strata():

上述相同的分层样本也可以使用取样包的 strata 函数创建，如下

```
library(sampling)  
stratas = strata(iris, c("Species"),size = c(3,3,3), method = "srswor")
stratasSpecies ID_unit   Prob Stratum17      setosa      17 0.06       124      setosa      24 0.06       145      setosa      45 0.06       165  versicolor      65 0.06       279  versicolor      79 0.06       295  versicolor      95 0.06       2114  virginica     114 0.06       3116  virginica     116 0.06       3128  virginica     128 0.06       3
```

注意:在上面的函数中，方法代表了在地层中选择单个样本的方法。一般使用以下方法。

没有替换的简单随机抽样

简单随机抽样替换

泊松-泊松抽样

系统-系统取样

## **4。整群抽样:**

如果人口数据本质上是地理数据，或者根据人口统计、习惯、背景等在人口中有一些预定义的群体，则通常使用整群抽样。

在整群抽样中，首先将总体分成称为群的小组，然后选择随机群来创建样本。

如果样本中包含了所选类别的所有元素，则称之为单阶段整群抽样；如果样本中包含了从每个类别中随机选择的元素，则称之为两阶段整群抽样。

例如，假设一个组织想要分析一种药物在美国的副作用，在这种情况下，可以先将整个人口划分为几个城市(其中每个城市的数据都包含该药物对所有患者的副作用的详细信息),然后随机选择这些城市中的患者纳入样本，从而执行两阶段整群抽样。

## R 中的整群抽样:

为了执行整群抽样，我们使用了 SDaA 包中的小学教师工作量数据集。工作量数据集包含不同地区不同学校教师的工作量详细信息，如工作时间、准备时间等。

```
install.packages(“SDaA”)
library(SDaA)
data(“teachers”)
> head(teachers) dist school hrwork size preprmin assist1 large     12  35.00   26      210      02 large     12  35.00   18       75      03 large     12  35.00   27      300      04 large     12  34.60   34       90      05 large     12  33.75   30      180      06 large     12  35.00   27      300      0#list of all the school_ids 
> unique(teachers[,2])
[1] 12 13 20 21 22 36 38 41 11 30 31 32  4 23  7 15 16 28 29  6  2 18 19 33 34  1  8  9  3 24 25#creating a cluster sample with 7 randomly selected clusters.
#Here we have formed clusters using the school variable.Hence each cluster contains the workload data of 7 randomly selected schools.
set.seed(123456)
cl=cluster(teachers,clustername=c("school"),size=7,method="srswor")
cl_data = getdata(teachers, cl)> head(cl_data)dist hrwork size preprmin assist school ID_unit      Prob260 sm/me  30.00    9       NA      0      1     260 0.2258065243 large  35.00   20      225    600     18     243 0.2258065244 large  35.00   16       90    300     18     244 0.225806519  large  37.50   25      180      0     20      19 0.225806518  large  38.75   24      240      0     20      18 0.225806517  large  38.35   24      120      0     20      17 0.2258065#list of the randomly selected schools
> unique(cl_data[,6])[1]  1 18 20 25 28 31 41#count of workload details within each school clusters
> table(cl_data$school)
8 12 16 21 28 34 38
5 13 24  7 18  7 10 #random sampling of clusters with a sample size of 5, so that each cluster contains 5 randomly selected workload details per school cluster.
cl_sam <- cl_data %>% group_by(school) %>% sample_n(size = 5)#Each of the 7 clusters have 5 randomly selected workload data.
> table(cl_sam$school)
8 12 16 21 28 34 38
5  5  5  5  5  5  5
```

使用上面的 R 代码，我们创建了 7 个随机簇，每个簇包含一个特定学校的工作量数据。以 5 的样本大小进一步随机采样聚类。因此，对于每个选定的学校集群，每个集群有 5 个工作量数据。

## 分层抽样和整群抽样的区别:

分层抽样和整群抽样的主要区别在于，在整群抽样中，组/群自然出现，如城市、地区等，这些选择的群元素作为一个整体用于抽样，例如，对于工作量数据，最初选择了 7 个学校群，这些群的所有元素单独用于进一步抽样。也就是说，只有 7 所学校的工作量数据被用于抽样调查，而忽略了其余学校的工作量细节。

然而，在分层抽样中，群体(在这种情况下是阶层)最初并不存在，来自每个阶层的元素被选择包括在样本中。例如，对于上述 iris，样本中包含了三个可用物种中每个物种的 3 个元素，没有一个物种像整群抽样一样被整体忽略。