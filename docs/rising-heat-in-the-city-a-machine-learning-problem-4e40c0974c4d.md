# 城市升温——一个机器学习问题

> 原文：<https://medium.com/analytics-vidhya/rising-heat-in-the-city-a-machine-learning-problem-4e40c0974c4d?source=collection_archive---------18----------------------->

最近，我参加了由特里希(NITT)国家理工学院计算机科学与工程(CSE)系组织的机器学习竞赛。这是一场名为 Data Crunch 的[比赛](https://www.hackerearth.com/datacrunch-vortex)，我们得到了一个[数据集](https://he-s3.s3.ap-southeast-1.amazonaws.com/media/hackathon/datacrunch-vortex/test-40/0c4d3086-7-25871c56-7-dataset.zip?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=d81c7b6903922c4c4b71de358cf445ec7cd5755a85386726ee1499e74de0bcb8&X-Amz-Date=20210308T064136Z&X-Amz-Credential=AKIA6I2ISGOYH7WWS3G5%2F20210308%2Fap-southeast-1%2Fs3%2Faws4_request)，我们必须使用机器学习模型来预测“热量”变量。还提供了一个测试集来进行一些预测并提交它们。这些提交的正确性基于 R2 分数来判断。在这篇文章中，我将谈谈我是如何处理这个问题的，并概述我对这个问题的解决方案。

![](img/f341af58e637d6208f90495dcc976081.png)

图片:[印度时报](https://timesofindia.indiatimes.com/city/chennai/hot-days-ahead-for-chennai-to-last-till-weekend/articleshow/73716720.cms)

我们将从竞赛链接中的确切问题陈述开始。声明如下:

*“jaya VI 在这座城市里四处走动，她觉得夏天即将来临，这座城市需要为越来越热的天气做好准备。因此，她决定使用提供给她的数据建立一个模型，根据数据集中的给定因素预测热量。帮助她建立模型。*

*数据集有训练和测试数据集。在训练数据集上训练您的模型，并在测试数据集上运行它以生成提交文件。评分将基于 R2 分数。”*

我已经在介绍中链接了数据集和竞争。我们将使用 Jupyter 笔记本在 Python 3 中编写代码。让我们从导入一些库开始。

```
import pandas as pd
import numpy as np
```

然后我们用熊猫来读取数据。

```
data = pd.read_csv("/content/drive/MyDrive/Datasets/Data Crunch Vortex/TRAIN.csv")
test = pd.read_csv("/content/drive/MyDrive/Datasets/Data Crunch Vortex/TEST.csv")
```

让我们看看训练数据集的前几列是什么样的。

```
data.head()
```

我们得到以下输出。

![](img/122054e37292a1d2e15eab011295fd10.png)

前几行训练数据。

我们可以看到我们的目标变量**热量**。我们还将其他因素定义如下:

*   **idx** :每次观察的 Id。
*   **UNIXTime** :指定特定的时刻。具体来说，它是自 1970–01–01 00:00:00 GMT 以来经过的秒数。在该数据集中，观测时间为格林威治时间 1970–01–01 00:00:00。
*   **数据**:指定观察的日期。
*   **时间**:指定一天中进行观测的时间。
*   **湿度**:湿度是空气中水蒸气含量的量度。
*   **温度**:观察时记录的温度。
*   **压力:**是观察时记录的压力。
*   **WindDegree** :指定观测时的风向。
*   **风速**:表示观测时的风速。
*   **日出时间**:观察日出的时间。
*   **日落时间**:观察到日落的时间。

我们将继续对这些数据进行一些基本的观察。让我们从任何缺失的值开始。

```
data.isnull().mean()
```

我们得到以下输出:

```
idx            0.0
UNIXTime       0.0
Data           0.0
Time           0.0
Heat           0.0
Temperature    0.0
Pressure       0.0
Humidity       0.0
WindDegrees    0.0
WindSpeed      0.0
RiseTime       0.0
SetTime        0.0
dtype: float64
```

不错！所以没有丢失的值。我们来看一些数据的描述性统计。

```
data.describe()
```

我们得到以下输出。

![](img/1a866fe32baf97a90bd3edcace76f8de.png)

描述统计学

我们可以看到数据集有 26，149 个观察值。还可以观察到其他细节，如平均值、第 25 百分位、最小值和最大值。让我们检查每个变量的数据类型。

```
data.dtypes
```

我们得到以下输出。

```
idx              int64
UNIXTime         int64
Data            object
Time            object
Heat           float64
Temperature      int64
Pressure       float64
Humidity         int64
WindDegrees    float64
WindSpeed      float64
RiseTime        object
SetTime         object
dtype: object
```

在数据的前几行中，我们可以观察到观察时间是以 UNIX 时间和 24 小时制表示的。他们可能来自不同的时区。让我们看看我们是不是对的。为此，我们必须从数据列中提取日期，并将其与时间列相结合，以获得 UTC 格式的观测时间。然后我们必须从 UNIX 时间中减去这个时间(转换成 UTC 格式)。如果差值为零，则所有观测都是在同一时区进行的，即 GMT(在这种情况下)。我们将创建数据的副本，以避免更改原始数据。

```
temp = data.copy()
temp[['Date','Midnight','AM or PM']] = temp['Data'].str.split(' ',expand=True)
temp['Data'] = pd.to_datetime(temp['Date'] + ' ' + temp['Time'])
temp['UNIXTimeDate'] = pd.to_datetime(temp['UNIXTime'],unit='s')
temp['ZoneDiff'] = temp['UNIXTimeDate'] - temp['Data']
temp.head()
```

我们得到以下输出。

```
0   0 days 10:00:00
1   0 days 10:00:00
2   0 days 10:00:00
3   0 days 10:00:00
4   0 days 10:00:00
Name: ZoneDiff, dtype: timedelta64[ns]
```

我们注意到 GMT 比这个时区早 10 个小时。因此，我们必须对观测时间、设定时间和上升时间进行时区校正。同样，处理日期列也非常困难。所以最好将它们转换成更容易处理的数据类型。在这种情况下，我们可以将日期转换成 UNIX 格式，这将确保我们所有的时间都是 GMT 格式，更重要的是，它们将是整数。我们在下面的函数中完成所有这些预处理。

```
def cleanData(temp):
   temp[['Date','Midnight','AM or PM']] = temp['Data'].str.split(' ',expand=True)
   temp['Data'] = pd.to_datetime(temp['Date'] + ' ' + temp['Time'])
   temp['SetTime'] = pd.to_datetime(temp['Date'] + ' ' + temp['SetTime'])
   temp['RiseTime'] = pd.to_datetime(temp['Date'] + ' ' + temp['RiseTime'])
   temp = temp.drop(['Date','Time','Midnight','AM or PM'],axis=1)
   temp['UNIXTimeDate'] = pd.to_datetime(temp['UNIXTime'],unit='s')
   temp['ZoneDiff'] = temp['UNIXTimeDate'] - temp['Data']
   temp['Data'] = temp['UNIXTimeDate'] - temp['ZoneDiff']
   temp['SetTime'] = temp['SetTime'] + temp['ZoneDiff']
   temp['RiseTime'] = temp['RiseTime'] + temp['ZoneDiff']
   temp = temp.drop(['UNIXTime','ZoneDiff'],axis=1) 
   temp['Data'] = pd.to_datetime(temp['Data']).astype(int) / 10**9
   temp['SetTime'] = pd.to_datetime(temp['SetTime']).astype(int) / 10**9
   temp['UNIXTimeDate'] = pd.to_datetime(temp['UNIXTimeDate']).astype(int) / 10**9
   temp['RiseTime'] = pd.to_datetime(temp['RiseTime']).astype(int) / 10**9
   return temptrain_clean = data.copy()
train_clean = cleanData(data)
train_clean.head()
```

我们得到以下输出。

![](img/4abcf75c30ff78ee9b9ac9f805fa8f1e.png)

已清理训练集。

我们的数据集现在看起来整洁多了。我们可以开始模拟我们的数据。让我们导入所需的库。我们将使用一个梯度助推器(特别是 XGBoost)来建模我们的数据。梯度增强器是处理结构化数据的最佳模型之一。

```
from sklearn.model_selection import train_test_split,RandomizedSearchCV
from sklearn.preprocessing import StandardScaler,MinMaxScaler
from sklearn.metrics import r2_score
from sklearn.pipeline import Pipeline
from xgboost import XGBRegressor
```

我们将创建三个数据子集。80%用于培训，10%用于验证，10%用于测试(除了可提交的测试集)。

```
X_train, X_test, y_train, y_test = train_test_split(train_clean.drop(['Heat','idx'],axis=1),train_clean['Heat'],test_size=0.1,random_state=42)X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, test_size=0.11, random_state=42)X_train.head()
```

我们得到以下输出。

![](img/dce4f488968aa6f6d9fd5a8668cb9ae7.png)

训练数据。

对于任何数值数据，最好对数据进行缩放，以免影响模型性能。为此，我们将使用最小最大缩放器。我们还将声明一个基本模型来进行一些预测，看看性能是否可以提高。我们将使用 scikit-learn 管道来完成所有这些工作。

```
heat_pipe = Pipeline([('scaler', MinMaxScaler()),('model', XGBRegressor())])
```

让我们拟合我们的管道并做一些预测。

```
heat_pipe.fit(X_train,y_train)
X_train_preds = heat_pipe.predict(X_train)
X_val_preds = heat_pipe.predict(X_val)
X_test_preds = heat_pipe.predict(X_test)
print("Train R2_score: {}".format(r2_score(y_train,X_train_preds)))
print("Validtation R2_score: {}".format(r2_score(y_val,X_val_preds)))
print("Test R2_score: {}".format(r2_score(y_test,X_test_preds)))
```

我们得到以下输出。

```
Train R2_score: 0.7713148026068124
Validtation R2_score: 0.7629794446793128
Test R2_score: 0.7561512410326393
```

对于一个基本模型来说，我们的分数看起来还不错。但正如他们所说，如果有改进的空间，不要退而求其次:)。

我们将尝试调整我们的模型，并创建一些新的功能，这可能有助于我们获得更好的结果。现在在这个结合点上，我想提一下，特征工程是基于直觉和领域知识的。没有硬性规定。有一些像 [featuretools](https://www.featuretools.com/) 这样的工具，可以通过在列之间执行所有可能的操作来帮助您稍微自动化这个过程。如果你想探索更多关于 featuretools 的东西，我建议你去看看这篇[的帖子](https://www.analyticsvidhya.com/blog/2018/08/guide-automated-feature-engineering-featuretools-python/)。这篇文章非常详细，涉及了 featuretools 的使用。

也就是说，使用您的领域知识创建的功能几乎总是会给出更好的结果。所以，我建议花点时间更好地理解你的变量，这样你就可以使用它们创建更好的特性。

在我们的例子中，我们可以看到我们的数据谈到了天气条件，特别是热量属性。在这里运用一点常识会告诉你，热量的多少取决于太阳升起后的时间。例如，下午比早上热。当然，我们可以利用这些信息，对吗？让我们创建一个新变量“TimeSinceSunRise”来存储太阳升起后的时间。cleanData 函数修改如下:

```
def cleanData(temp): temp[['Date','Midnight','AM or PM']] = temp['Data'].str.split(' ',expand=True)
   temp['Data'] = pd.to_datetime(temp['Date'] + ' ' + temp['Time'])
   temp['SetTime'] = pd.to_datetime(temp['Date'] + ' ' + temp['SetTime'])
   temp['RiseTime'] = pd.to_datetime(temp['Date'] + ' ' + temp['RiseTime'])
   temp = temp.drop(['Date','Time','Midnight','AM or PM'],axis=1)
   temp['UNIXTimeDate'] = pd.to_datetime(temp['UNIXTime'],unit='s')
   temp['ZoneDiff'] = temp['UNIXTimeDate'] - temp['Data']
   temp['Data'] = temp['UNIXTimeDate'] - temp['ZoneDiff']
   temp['SetTime'] = temp['SetTime'] + temp['ZoneDiff']
   temp['RiseTime'] = temp['RiseTime'] + temp['ZoneDiff']
   temp = temp.drop(['UNIXTime','ZoneDiff'],axis=1) 
   temp['Data'] = pd.to_datetime(temp['Data']).astype(int) / 10**9
   temp['SetTime'] = pd.to_datetime(temp['SetTime']).astype(int) / 10**9
   temp['UNIXTimeDate'] = pd.to_datetime(temp['UNIXTimeDate']).astype(int) / 10**9
   temp['RiseTime'] = pd.to_datetime(temp['RiseTime']).astype(int) / 10**9
   temp['TimeSinceSunRise'] = temp['RiseTime'] - temp['Data']
   return temp
```

我们可以用它来清理我们的训练数据，并如上所述对其建模。对这个新数据集进行预测会得到以下结果。

```
Train R2_score: 0.8975578362931129
Validtation R2_score: 0.8889480708391524
Test R2_score: 0.8849217036129068
```

哇哦。我们的 R2 分数有了巨大的提升！看起来我们的特色“日出时间”帮助我们的模型做出了非常好的预测！

但是我们不要就此打住！我们可以通过调整模型的参数来进一步改善这一点。XGBoost 有很多可以调优的超参数。我们将重点关注其中的五个，并对它们进行如下调整:

```
params={
"learning_rate": [0.1, 0.01 ,0.001],
"n_estimators": [100,250,500,1000],
"gamma" : [0.01, 0.1, 0.3, 0.5, 1, 1.5, 2],
"max_depth": [2,4,7,10],
"min_child_weight" : [1,3,5,7]
}
model = XGBRegressor()
xgb_rscv = RandomizedSearchCV(model, param_distributions=params, scoring='r2', cv=10, verbose=3, random_state=42)
model_xgboost=xgb_rscv.fit(X_train,y_train)print("Learning Rate: ", model_xgboost.best_estimator_.get_params()["learning_rate"])print("Gamma: ", model_xgboost.best_estimator_.get_params()["gamma"])print("Max Depth: ", model_xgboost.best_estimator_.get_params()["max_depth"])print("Minimum Sum of the Instance Weight Hessian to Make a Child: ",model_xgboost.best_estimator_.get_params()["min_child_weight"])print("Number of Trees: ", model_xgboost.best_estimator_.get_params()["n_estimators"])
```

我们得到以下输出:

```
Learning Rate: 0.1
Gamma: 0.5
Max Depth: 10
Minimum Sum of the Instance Weight Hessian to Make a Child: 1
Number of Trees: 100
```

让我们将这些值插入管道，看看会得到什么结果。

```
heat_pipe = Pipeline([('scaler', MinMaxScaler()),('model', XGBRegressor(n_estimators=100, gamma=0.5,learning_rate=0.1, max_depth=10,min_child_weight=1))])heat_pipe.fit(X_train,y_train)
X_train_preds = heat_pipe.predict(X_train)
X_val_preds = heat_pipe.predict(X_val)
X_test_preds = heat_pipe.predict(X_test)
print("Train R2_score: {}".format(r2_score(y_train,X_train_preds)))
print("Validtation R2_score: {}".format(r2_score(y_val,X_val_preds)))
print("Test R2_score: {}".format(r2_score(y_test,X_test_preds)))
```

我们得到以下输出:

```
Train R2_score: 0.9908897998162235
Validtation R2_score: 0.941652850962926
Test R2_score: 0.9312753509058348
```

瞧啊。这进一步将我们在测试集上的分数提高到 0.93，在验证集上的分数提高到 0.94。也许我们可以进一步提高我们的成绩，但那是以后的事了。:)

为了对竞赛中提交的测试集进行预测，我们可以执行以下操作:

```
submission = test_clean.drop(['idx'],axis=1)
X_submission_preds = heat_pipe.predict(submission)
submission_df = pd.DataFrame(X_submission_preds)
submission_df.rename(columns={0 : "Heat"},inplace=True)
submission_df['idx'] = test_clean['idx']
submission_df = submission_df[['idx','Heat']]
submission_df.to_csv("/content/drive/MyDrive/Datasets/Data Crunch Vortex/submission_1.csv",index=False)
```

创建的 CSV 文件可以提交给比赛，以获得我们的表现得分。

因此，我们分析了提供给我们的数据集，并对其进行了一些很好的预测。谢谢你坚持到这里！干杯！回头见！