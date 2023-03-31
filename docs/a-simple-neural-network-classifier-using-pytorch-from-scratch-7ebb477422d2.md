# 一个简单的使用 PyTorch 的神经网络分类器，从零开始

> 原文：<https://medium.com/analytics-vidhya/a-simple-neural-network-classifier-using-pytorch-from-scratch-7ebb477422d2?source=collection_archive---------0----------------------->

![](img/381f9a0753d22a31355b5bd7658216e1.png)

来源:[https://i0 . WP . com/msalimali . com/WP-content/uploads/2019/06/connected-artificial-neural-network-nodes-msalimali . jpg？ssl=1](https://i0.wp.com/msalimali.com/wp-content/uploads/2019/06/connected-artificial-neural-network-nodes-msalimali.jpg?ssl=1)

在本文中，我们将使用 PyTorch 构建一个简单的神经网络分类器模型。在本文中，我们将涵盖以下内容:

*   步骤 1:生成并分割数据
*   步骤 2:处理生成的数据
*   步骤 3:从头开始构建神经网络分类器
*   步骤 4:训练神经网络分类器
*   步骤 5:保存训练好的模型
*   步骤 6:加载保存的模型
*   步骤 7:测试训练好的模型

# **依赖关系**

*   PyTorch v1.10.0
*   sci kit-learn 1 . 0 . 2 版
*   Numpy v1.19.5

# 步骤 1:生成并分割数据

让我们使用 Scikit-learn 制作或生成我们的分类数据集

```
from sklearn.datasets import make_classificationX, Y = make_classification(
  n_samples=100, n_features=4, n_redundant=0,
  n_informative=3,  n_clusters_per_class=2, n_classes=3
)
```

我们只生成很少的样本`100`，这可以通过改变`n_samples`参数来增加

接下来，让我们将数据分为训练和测试。33 %的数据用于测试。

```
from sklearn.model_selection import train_test_splitX_train, X_test, Y_train, Y_test = train_test_split(
  X, Y, test_size=0.33, random_state=42)
```

# 步骤 2:处理生成的数据

一旦获得训练和测试数据集，我们就使用 PyTorch `Dataset`和`DataLoader`处理数据。`Dataset`存储样品及其相应的标签，`DataLoader`在`Dataset`周围包裹一个可重复标签，以便于获取样品。

```
import numpy as np
import torch
from torch.utils.data import Dataset, DataLoaderclass Data(Dataset): def __init__(self, X_train, y_train):
    # need to convert float64 to float32 else 
    # will get the following error
    # RuntimeError: expected scalar type Double but found Float
    self.X = torch.from_numpy(X_train.astype(np.float32))
    # need to convert float64 to Long else 
    # will get the following error
    # RuntimeError: expected scalar type Long but found Float
    self.y = torch.from_numpy(y_train).type(torch.LongTensor)
    self.len = self.X.shape[0]

  def __getitem__(self, index):
    return self.X[index], self.y[index] def __len__(self):
    return self.len
```

我们创建了一个继承了`torch.utils.data.Dataset`属性的类。然后，训练数据按如下方式传递:

```
traindata = Data(X_train, Y_train)
```

现在，可以使用索引轻松访问培训数据:

```
print(traindata[25])
'''
# Output
(tensor([-0.9528,  1.6890, -0.6810,  0.7165]), tensor(1))
'''
```

我们还可以对训练数据进行如下分割:

```
print(traindata[25:34])
'''
# Output
(tensor([[-0.9528,  1.6890, -0.6810,  0.7165],
         [ 0.6994,  4.5166,  0.5078, -2.0575],
         [ 0.8508,  1.6109,  0.3014,  0.9455],
         [ 1.1293, -0.8988,  1.6426, -0.0171],
         [-0.2316,  1.9337, -0.9727, -0.1864],
         [-1.0156,  1.1438, -0.0883,  0.6976],
         [ 1.2509, -1.6992,  1.8562, -1.7159],
         [ 1.1714,  0.9062, -1.5627, -0.5184],
         [-1.1780, -2.7274, -1.0570,  1.9610]]),
 tensor([1, 2, 0, 0, 1, 1, 0, 0, 1]))
'''
```

接下来，我们使用`DataLoader`加载训练数据，我们将 batch_size 设置为 4。

```
batch_size = 4
trainloader = DataLoader(traindata, batch_size=batch_size, 
                         shuffle=True, num_workers=2)
```

# 步骤 3:从头开始构建神经网络分类器

现在让我们建立我们的神经网络分类器

```
import torch.nn as nn# number of features (len of X cols)
input_dim = 4
# number of hidden layers
hidden_layers = 25
# number of classes (unique of y)
output_dim = 3class Network(nn.Module):
  def __init__(self):
    super(Network, self).__init__()
    self.linear1 = nn.Linear(input_dim, hidden_layers)
    self.linear2 = nn.Linear(hidden_layers, output_dim) def forward(self, x):
    x = torch.sigmoid(self.linear1(x))
    x = self.linear2(x)
    return x
```

我们可以通过调用它来初始化分类器:

```
clf = Network()
```

我们还可以列出如下网络参数:

```
print(clf.parameters)
'''
# Output
<bound method Module.parameters of Network(
  (linear1): Linear(in_features=4, out_features=25, bias=True)
  (linear2): Linear(in_features=25, out_features=3, bias=True)
)>
'''
```

接下来让我们定义我们的损失函数和优化器

```
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(clf.parameters(), lr=0.1)
```

# 步骤 4:训练神经网络分类器

现在我们已经为我们的训练做好了准备，让我们为我们的训练编码:

```
epochs = 2
for epoch in range(epochs):
  running_loss = 0.0
  for i, data in enumerate(trainloader, 0):
    inputs, labels = data
    # set optimizer to zero grad to remove previous epoch gradients
    optimizer.zero_grad()
    # forward propagation
    outputs = clf(inputs)
    loss = criterion(outputs, labels)
    # backward propagation
    loss.backward()
    # optimize
    optimizer.step()
    running_loss += loss.item()
  # display statistics
  print(f'[{epoch + 1}, {i + 1:5d}] loss: {running_loss / 2000:.5f}')
```

出于演示的目的，我正在为`2`纪元进行培训，它可以根据需要进行更改。输出将如下所示:

```
[1,    17] loss: 0.00522
[2,    17] loss: 0.00508
```

# 步骤 5:保存训练好的模型

现在让我们保存我们训练好的模型:

```
# save the trained model
PATH = './mymodel.pth'
torch.save(clf.state_dict(), PATH)
```

# 步骤 6:加载保存的模型

然后可以加载本地保存的模型进行推理，使用以下代码:

```
clf = Network()
clf.load_state_dict(torch.load(PATH))
'''
# Output
<All keys matched successfully>
'''
```

# 步骤 7:测试训练好的模型

一旦模型被加载，我们就可以测试我们训练好的模型。让我们测试一个小批量。

```
testdata = Data(X_test, Y_test)
testloader = DataLoader(testdata, batch_size=batch_size, 
                        shuffle=True, num_workers=2)
```

从`DataLoader`获取一个小批量

```
dataiter = iter(testloader)
inputs, labels = dataiter.next()
```

测试输入将如下所示:

```
print(inputs)
'''
# Output
tensor([[ 1.6876, -1.2382,  1.5971, -2.2628],
        [ 0.7683,  0.3534,  0.0460, -1.2109],
        [-1.0097,  1.1584, -0.0593,  0.7738],
        [ 1.7332,  0.1764,  0.5259, -2.3073]])
'''
```

测试标签将如下所示:

```
print(labels)
'''
# Output
tensor([0, 2, 1, 0])
'''
```

现在让我们做推论

```
outputs = clf(inputs)
__, predicted = torch.max(outputs, 1)
print(predicted)
'''
# Output
tensor([0, 0, 1, 0])
'''
```

看起来我们的代码像预期的那样工作，让我们对整个测试数据集进行推断。

```
correct, total = 0, 0
# no need to calculate gradients during inference
with torch.no_grad():
  for data in testloader:
    inputs, labels = data
    # calculate output by running through the network
    outputs = clf(inputs)
    # get the predictions
    __, predicted = torch.max(outputs.data, 1)
    # update results
    total += labels.size(0)
    correct += (predicted == labels).sum().item()
print(f'Accuracy of the network on the {len(testdata)} test data: {100 * correct // total} %')'''
# Output
Accuracy of the network on the 33 test data: 75 %
'''
```

该模型可以进一步改变以提高精确度。

编码快乐！！！