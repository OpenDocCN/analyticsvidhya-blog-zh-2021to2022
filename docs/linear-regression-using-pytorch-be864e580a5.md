# 使用 PyTorch 进行线性回归

> 原文：<https://medium.com/analytics-vidhya/linear-regression-using-pytorch-be864e580a5?source=collection_archive---------23----------------------->

![](img/4eb99523a77fa780a0fe33d40bbdecdd.png)

正如我们所知，线性回归是一种在机器学习中实现的统计模型，用于确定一个或多个解释变量的标量响应。使用程序员可用的各种各样的包，实现这样的模型变得更加容易。今天，我将向您展示如何使用 PyTorch(一个 python 库)来实现线性回归模型。要了解 PyTorch 的更多信息，请点击[此处](https://pytorch.org/)。

我们将使用 Kaggle 上的波士顿定价数据集进行演示。在我们开始编码我们的模型之前，让我们导入所需的包。

```
import pandas as pd           #Package to work with DataFrames
import numpy as np            #Package to work with Arrays
import torch                  #Package to work with tensors 
import torch.nn as nn         #Package in which the model is present
```

现在，我们将波士顿住房数据集导入到数据框中。

```
data = pd.read_csv(r'../input/bostonhoustingmlnd/housing.csv')
```

现在让我们选择预测列和目标列。因为我们必须表示波士顿房子的价格，所以“MEDV”被作为代表中间值的目标列，而所有其他列都是预测值。

```
inputs = data.drop('MEDV', axis = 1)   
#The axis is given as 1 to demonstrate it as column
targets = data['MEDV']
```

我们现在有了预测所需的数据。但是，它是熊猫数据帧的形式，PyTorch 要工作，我们需要将它们转换成张量。

```
'''We are changing the type of data to float32 from the default double64 while converting them to tensors'''
inputs = torch.tensor(inputs.values.astype(np.float32))
targets = torch.tensor(targets.values.astype(np.float32))
```

获得张量后，我们需要将它们添加到 TensorDataset 中，以便将输入数据分成训练和测试数据。这可以使用 torch 库来完成。DataLoader 用于将整个数据集转换成批处理。

```
from torch.utils.data import DataLoader, TensorData
train_ds = TensorDataset(inputs, targets)    #TensorDataset
batch_size = 10  #The dataset is divided into 10 parts'''We now get the training dataset with 10 splits, "shuffle" is set to true to make sure that the dataset is shuffled before being split into batches'''train_dl = DataLoader(train_ds, batch_size, shuffle = True) 
```

接下来，我们开始创建线性回归模型。调用线性模型后，我们传递两个参数，第一个是输入的数量，第二个是输出的数量。torch 库使用 randn 方法设置权重和偏差，我们可以看到初始值。

```
model = nn.Linear(3, 1)
print(model.weights, model.bias) #Initial values of weight and bias
```

现在已经建立了线性回归模型。我们现在可以针对不同的输入进行测试。训练数据作为用于预测的参数被传输，因为数据被分成批，并且模型将一直面对不同的数据集。

```
preds = model(inputs)
```

我们现在需要优化模型以获得更好的结果。我们使用均方差来检验模型的误差，并使用标准梯度下降法进行优化。所需的方法可以在 torch 库中找到，实现如下所示。

```
import torch.nn.functional as F
loss_function = F.mse_loss     
loss = loss_function(preds, targets)
opt = torch.optim.SGD(model.parameters(), lr = 1e-8)  
#lr-Learning Rate
```

我们现在创建一个函数，在需要时调用这个模型。backward()函数用于回溯变量的值，并找到它的导数。

```
'''
This function takes the number of iterations(epochs), the type of neural network(model), the loss function used to determine
the accuracy(loss_fn), the optimzation factor(opt), and the training dataset(train_dl). After all the opertaions are done,
it returns the predicted values with minimum loss. If the results are not yet accurate, alter minor values like epoch and opt.
'''
def fit(epochs, model, loss_fn, opt, train_dl):
    for e in range(epochs):
        for xb, yb in train_dl:
            pred = model(xb)
            loss = loss_fn(pred, yb)
            loss.backward()
            opt.step()
            opt.zero_grad()
if e % 10 == 0:
print(loss)
```

恭喜您，您已经使用 PyTorch 成功构建了一个工作的线性回归模型。要在 Kaggle 上访问我的笔记本，请点击这里的。