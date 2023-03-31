# 使用 LSTMs 预测股票价格(PyTorch 版)

> 原文：<https://medium.com/analytics-vidhya/predict-stock-prices-using-lstms-pytorch-edition-aa9b3875f131?source=collection_archive---------7----------------------->

如果 Medium 在订阅等问题上困扰你，你可以使用这个链接:[https://gitlab.com/-/snippets/2059718](https://gitlab.com/-/snippets/2059718)。

在本文中，我们将使用 LSTMs 编写一个简单的股票预测器。神经网络使用过去半年左右的股票数据，并使用 LSTMs 来预测未来股票价格的值。
你可能已经从这篇文章的标题猜到了，这篇文章的 TensorFlow 版本即将问世。为什么要同时使用 TensorFlow 和 PyTorch 编写一个类似的程序？我对神经网络和深度学习真的很陌生，在看到一堆关于 Keras 和 PyTorch 如何优于另一个的资源后，我决定测试一下这两个，看看哪个更容易使用。

Jupyter 笔记本在[这里](https://gitlab.com/-/snippets/2059712)有售。

# LSTMs

什么是 LSTMs？LSTMs 是类似于 RNNs 的神经网络，它们获取一些输出并将它们“循环回”网络。这使得他们可以学习一些东西，比如部分或完全依赖于之前股票价格的股票价格。例如，如果一只股票的价格正在上涨，它可能在下一分钟也会上涨。

写有`# elided`的行被省略了，因为我觉得它们对于理解程序是不必要的。您仍然可以在笔记本上查看完整的源代码。

我们将从导入我们的库开始。

```
#@title Install/Import Libraries { vertical-output: true }def import_install(id: str):
  # elided defining import_install, which installs the module is not available and imports it.!python3 -m pip install --quiet --upgrade pipimport os
import time
import datetime
import pathlibsns = import_install('seaborn')
!python3 -m pip install --quiet --upgrade matplotlib
mpl = import_install('matplotlib')
plt = mpl.pyplot
torch = import_install('torch')
nn = torch.nn
np = import_install('numpy')
pd = import_install('pandas')
yf = import_install('yfinance')# elided configuring matplotlibimport seaborn
import matplotlib
import torch
import numpy
import pandas
import yfinance
```

现在，我们可以使用 Yahoo！使用`[yfinance](https://github.com/ranaroussi/yfinancehttps://github.com/ranaroussi/yfinance)` Python 模块进行财务。我们将从获取`TSLA`特斯拉的股票价格开始。

```
#@title Get Stock Data { vertical-output: true }
run = True #@param {type:"boolean"}
cache_path = "./data.csv" #@param {type:"string"}
ticker = "TSLA" #@param {type:"string"}
period = "1y" #@param {type:"string"}
interval = "1h" #@param {type:"string"}
auto_adjust = True #@param {type:"boolean"}
prepost = True #@param {type:"boolean"}
threads = 0 #@param {type:"integer"}
if threads == 0:
  threads = False
proxy = "" #@param {type:"string"}
if proxy == "":
  proxy = Noneif run:
  df = yf.download(
      # elided yfinance configuration
      )
  df.to_csv(cache_path)
  df = pd.read_csv(cache_path)
  # ensure consistency beyween run = True and False
  print('successfully fetched data from yfinance & saved to cache')
else:
  df = pd.read_csv(cache_path)
  print('successfully fetched data from cache')df = df.dropna()
df['Average'] = df[['High', 'Low']].mean(axis=1)
df['Date'] = pd.to_datetime(df['Date'])
df['DateNum'] = df['Date'].values.astype(np.int64) // 10 ** 9
print('successfully added columns: Average, DateNum')[*********************100%***********************]  1 of 1 completed
successfully fetched data from yfinance & saved to cache
successfully added columns: Average, DateNum
```

现在我们已经下载了我们的股票数据，我们可以看看它是什么样子的。我们首先从列表中获取每 20 个价格，然后绘制平均价格(从高到低)。

```
#@title Graph Data { run: "auto", vertical-output: true }dep_cols = ['Average'] #@param {type:"raw"}
indep_col = 'Date' #@param {type:"string"}start = 0 #@param {type:"integer"}
stop =  -1#@param {type:"integer"}
step =  20#@param {type:"integer"}plot_features = df[dep_cols]plot_features.index = df[indep_col]
_ = plot_features.plot(subplots=True)plot_features = df[dep_cols][start:stop:step]
plot_features.index = df[indep_col][start:stop:step]
_ = plot_features.plot(subplots=True)
```

我们现在将分割和标准化数据。由于神经网络只能预测从`-1`到`+1`的值，我们必须确保数据被标准化以适应这些值。我们还将把数据分成训练、验证和测试数据。由于模型可能会记住训练数据，我们需要模型不知道的数据来准确地测试它。

```
#@title Split & Normalize Datatrain = 0.7 #@param {type:"number"}
val = 0.2 #@param {type:"number"}
test = 0.1 #@param {type:"number"}normalize = True #@param {type:"boolean"}dep_cols =  ['Average'] #@param {type:"raw"}
indep_cols = ['DateNum'] #@param {type:"raw"}def without(d, *keys):
  # elided defining without which removes *keys from dict d.del_cols = list(df.columns) # columns to be removed
for dep_col in dep_cols:
  del_cols.pop(del_cols.index(dep_col))
for indep_col in indep_cols:
  del_cols.pop(del_cols.index(indep_col))
df_only = without(df, *del_cols)
avg_data = df_only['Average'].values.astype(float)l = len(avg_data)
train_data = avg_data[0:int(l*train)]
val_data = avg_data[int(l*train):int(l*(train+val))]
test_data = avg_data[int(l*(train+val)):]mean = avg_data.mean()
std = avg_data.std()if normalize:
  train_data = (train_data - mean) / std
  val_data = (val_data - mean) / std
  test_data = (test_data - mean) / stdprint(f'train:\t{len(train_data)}')
print(f'val:\t{len(val_data)}')
print(f'test:\t{len(test_data)}')train:	2911
val:	832
test:	416
```

现在我们可以把数据转换成张量，这样我们就可以在上面训练了。

```
#@title Convert Data to Tensorsdef create_inout_sequences(input_data, tw):
  inout_seq = []
  for i in range(len(input_data)-tw):
    train_seq = input_data[i:i+tw]
    train_label = input_data[i+tw:i+tw+1]
    inout_seq.append((train_seq ,train_label))
  return inout_seqraw_train_data = train_data
train_data = torch.FloatTensor(train_data).view(-1)
train_window = 12raw_val_data = val_data
val_data = torch.FloatTensor(val_data).view(-1)raw_test_data = test_data
test_data = torch.FloatTensor(test_data).view(-1)train_inout_seq = create_inout_sequences(train_data, train_window)print(train_inout_seq[:5])[(tensor([-1.1586, -1.1591, -1.1572, -1.1543, -1.1551, -1.1513, -1.1431, -1.1360,
        -1.1270, -1.1221, -1.1230, -1.1218]), tensor([-1.1165])), (tensor([-1.1591, -1.1572, -1.1543, -1.1551, -1.1513, -1.1431, -1.1360, -1.1270,
        -1.1221, -1.1230, -1.1218, -1.1165]), tensor([-1.1186])), (tensor([-1.1572, -1.1543, -1.1551, -1.1513, -1.1431, -1.1360, -1.1270, -1.1221,
        -1.1230, -1.1218, -1.1165, -1.1186]), tensor([-1.1109])), (tensor([-1.1543, -1.1551, -1.1513, -1.1431, -1.1360, -1.1270, -1.1221, -1.1230,
        -1.1218, -1.1165, -1.1186, -1.1109]), tensor([-1.1101])), (tensor([-1.1551, -1.1513, -1.1431, -1.1360, -1.1270, -1.1221, -1.1230, -1.1218,
        -1.1165, -1.1186, -1.1109, -1.1101]), tensor([-1.1067]))]
```

1

100

1

投入

LSTM

线性的

输出

现在我们可以制作模型了，它是`torch.nn.Module`的子类。

```
#@title Make Model { vertical-output: true } #@markdown Model: #@markdown ``` #@markdown LSTM( #@markdown (lstm): LSTM(1, 100) #@markdown (linear): Linear(in_features=100, out_features=1, bias=True) #@markdown ) #@markdown ``` class LSTM(nn.Module): def __init__(self, input_size=1, hidden_layer_size=100, output_size=1): super().__init__() self.hidden_layer_size = hidden_layer_size self.lstm = nn.LSTM(input_size, hidden_layer_size) self.linear = nn.Linear(hidden_layer_size, output_size) self.hidden_cell = (torch.zeros(1,1,self.hidden_layer_size), torch.zeros(1,1,self.hidden_layer_size)) def forward(self, input_seq): lstm_out, self.hidden_cell = self.lstm(input_seq.view(len(input_seq) ,1, -1), self.hidden_cell) predictions = self.linear(lstm_out.view(len(input_seq), -1)) return predictions[-1]
```

建立模型后，我们现在将使用训练数据训练网络。

```
#@title Train or Load Model { vertical-output: true }
#@markdown Check `load_model` to load the model. Uncheck to train the model.
load_model = True #@param {type:"boolean"}
load_model_from = './models/epoch-190.pt' #@param {type:"string"}
epochs = 200 #@param {type:"integer"}
save_model_to = './models-{datetime}' #@param {type:"string"}
show_status_every = 5 #@param {type:"integer"}# Make model
model = LSTM()
loss_function = nn.MSELoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
# elided
if load_model:
  # elided
  model.load_state_dict(torch.load(load_model_from))
else:
  # elided for epoch in range(epochs):
    # elided
    for seq, labels in train_inout_seq: # Train for an epoch
      optimizer.zero_grad()
      model.hidden_cell = (torch.zeros(1, 1, model.hidden_layer_size),
                      torch.zeros(1, 1, model.hidden_layer_size))
      y_pred = model(seq)
      single_loss = loss_function(y_pred, labels)
      single_loss.backward()
      optimizer.step() # Save model
    torch.save(
      model.state_dict(), 
      os.path.join(save_model_to, f'epoch-{epoch}-state.pt'),
    )
    # elided
```

现在，我们的模型已经训练好了。我们可以看到它在测试数据中的表现。`x`变量是`train_window`训练窗口中值的索引范围。通过这种方式，我们可以将这些值与真实数据对齐。

```
#@title Predict & Plot Predicted Data { vertical-output: true }
test_inputs = test_data[-train_window:].tolist()model.eval()for i in range(train_window):
    seq = torch.FloatTensor(test_inputs[-train_window:])
    with torch.no_grad():
        model.hidden = (torch.zeros(1, 1, model.hidden_layer_size),
                        torch.zeros(1, 1, model.hidden_layer_size))
        test_inputs.append(model(seq).item())actual_predictions = (np.array(test_inputs[train_window:]).reshape(-1, 1)) * std + meanplt.title('Predicted Data: Time v Price (all)')
plt.xlabel('Time')
plt.ylabel('Price')
plt.grid(True)
plt.autoscale(axis='x', tight=True)
plt.plot(test_data * std + mean)
x = np.arange(len(test_data)-train_window-1, len(test_data)-1, 1)
plt.plot(x,actual_predictions)
plt.show()# elided as it almost the same as above
```

模型的预测是图中的橙色线。如您所见，我们的模型能够捕捉价格趋势，并根据之前的价格做出合理的预测。

# 结论

LSTMs 广泛用于解决序列问题，例如预测股票。在本文中，我们介绍了如何实现 LSTM 网络并使用它来预测股票价格，并将其与实际价格进行比较的步骤。