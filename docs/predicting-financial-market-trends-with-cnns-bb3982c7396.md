# 用 CNN 预测金融市场趋势

> 原文：<https://medium.com/analytics-vidhya/predicting-financial-market-trends-with-cnns-bb3982c7396?source=collection_archive---------11----------------------->

![](img/de5dfe197617744f6895f1794e4c42ec.png)

# 介绍

在钻研预测和用神经网络预测时，我发现了对时间序列预测的直接兴趣。我决定开始着手一个项目，试图预测金融市场的积极和消极趋势。早期我选择尝试预测趋势，而不是未来价格。许多交易者更喜欢根据任何给定市场的总体积极或消极趋势进行交易，而不是使用非常具体的价格预测。这种方法还简化了神经网络的学习目标，因为正趋势和负趋势可以用 0 和 1 来表示。使用二元分类方法让我避免了回归问题。

虽然递归神经网络(RNN)是时间序列预测的典型选择，但我发现卷积神经网络(CNN)对于我所追求的目的更有效。这并不是说，随着进一步的研究，不能使用本文中概述的实践来开发有效的 RNN。

# 数据收集和预处理

对我来说，收集高质量的数据并对其进行适当的处理是一个艰难的过程。仅仅使用股票或货币对的连续价格数据是不够有效的；价格的变化会受到许多因素的影响，而神经网络无法获得这些信息。为了预测积极和消极的价格趋势，动量指标是一个显而易见的选择。虽然有很多可供选择，但我决定使用我更熟悉的方法:[简单移动平均线](https://www.investopedia.com/terms/s/sma.asp)(SMA)[指数移动平均线](https://www.investopedia.com/terms/e/ema.asp)(EMA)[移动平均线收敛发散](https://www.investopedia.com/terms/m/macd.asp)(MACD)[相对强弱指数](https://www.investopedia.com/terms/r/rsi.asp) (RSI)，以及[布林线](https://www.investopedia.com/terms/b/bollingerbands.asp) (BB)。

我使用了 Kaggle 用户 *Imetomi* 的[欧元美元外汇对历史数据(2002–2020)](https://www.kaggle.com/imetomi/eur-usd-forex-pair-historical-data-2002-2019)数据集。在数据集中是一个包含超过 500 万个时间步长的分钟分辨率文件，这对于深度学习来说是一个有利可图的选择，因为模型随着数据的增加而学习得更好。其他 API，如 Oanda 或 AlphaVantage，有大量高质量的数据可用于各种应用程序。

将文件导入并格式化到程序中是一个简单的过程。我下载了 CSV 文件，并把它作为熊猫的数据帧导入。然后，我每隔五分钟遍历一次开价，以获得五分钟的数据。以下代码导入了将在本教程中使用的库，并说明了如何处理和绘制数据帧。

```
**import** **tensorflow** **as** **tf**
**import** **matplotlib.pyplot** **as** **plt**
**import** **requests**
**import** **pandas** **as** **pd**
**import** **csv**
**import** **numpy** **as** **np**
**import** **time**
**import** **scipy**
**import** **math**
**import** **time**
**import** **datetime**
**from** **scipy** **import** statdf = pd.read_csv('/content/EURUSD_MN.csv') 

df = np.array(df['BO'][::5], dtype=np.float32) print('Dataframe Shape: ' + str(df.shape))

plt.plot(df)
plt.show()
```

导入数据帧并将其设置为 numpy 数组后，下一步是计算指标。我为每个指标编写了一个函数，它将数据帧作为输入，并输出一个计算出的指标数组。我不打算深入讨论每个指标的数学公式，因为它们在前面的部分有超链接。

```
**def** calc_macd_and_ema(X, display=**False**):
  *'''Calculates the Moving Average Convergence Divergence Indicator*
 *and the Exponential Moving Average Indicator'''*
  macd, ema = [-.1], [1.]

  **for** i **in** range(1, len(X)):
    e26 = X[i] * (2 / 27) + X[i-1] * (1 - (2 / 27))
    e12 = X[i] * (2 / 27) + X[i-1] * (1 - (2 / 13))
    m = e12 - e26

    ema.append(e12)
    macd.append(m)

  **if** display:
    plt.plot(macd)
    plt.title('MACD')
    plt.show()

    plt.plot(ema)
    plt.title('EMA')
    plt.show()

  **return** np.array(macd), np.array(ema)

**def** calc_sma(X, window, display=**False**):
  *'''Calculates the Simple Moving Average Indicator'''*
  sma = [1.]
  **for** i **in** range(1, len(X)):
    **if** i < window:
      sma.append((sum(X[:i])) / i)
    **else**: 
      sma.append((sum(X[i-window:i])) / window)

  **if** display:
    plt.plot(sma)
    plt.title('SMA')
    plt.show()  

  **return** np.array(sma)

**def** calc_rsi(X, display=**False**):
  *'''Calculates the Relative Strength Index'''*
  look_back = 24
  rsi = np.zeros((len(X)), dtype=np.float32)
  rsi[0] = 50.
  avu = [.001]
  avd = [.001]
  **for** i **in** range(1, len(X)):

    **if** X[i] > X[i-1]:
      avu.append(X[i]/X[i-1])
      **if** len(avu) > look_back: avu.pop(0)

    **else**:
      avd.append(X[i]/X[i-1])
      **if** len(avd) > look_back: avd.pop(0)
    r = (100 - (100 / (1 + (np.mean(avu) / np.mean(avd)))))

    **if** 50\. < r < 50.1: rsi[i] = (r) 
    **else**: rsi[i] = 50.

  **if** display:
    plt.plot(rsi)
    plt.title('RSI')
    plt.show()

  **return** np.array(rsi)

**def** calc_BB(X, ma, display=**False**):
  *'''Calculates the Bollinger Bands Indicator'''*
  window = 48
  bbu, bbd = np.zeros((len(X))), np.zeros((len(X)))
  bbu[0] = 0.
  bbd[0] = 0.

  **for** i **in** range(1, len(X)):
    **if** i < window:
      bbu[i] = (ma[i]* np.mean(X[:window]) + 2 * np.std(X[:window]))
      bbd[i] = (ma[i]* np.mean(X[:window]) - 2 * np.std(X[:window]))
    **else**:
      bbu[i]= (ma[i]*np.mean(X[i-window:i])+2*np.std(X[i-window:i]))
      bbd[i]= (ma[i]*np.mean(X[i-window:i])-2*np.std(X[i-window:i]))

  **if** display:
    plt.plot(bbu)
    plt.plot(bbd)
    plt.title('BBands')
    plt.show()

  **return** np.array(bbu), np.array(bbd)macd, ema = calc_macd_and_ema(df, display=**False**) 
sma = calc_sma(df, window=32, display=**False**) 
rsi = calc_rsi(df, display=**False**) 
bbu, bbd = calc_BB(df, sma, display=**False**)
```

我使用 Google Colab 来执行我的所有代码，并且我发现内存限制在这一步造成了一个问题。如果使用了所有五百万个数据点，那么当调用指示器函数时，会话将由于缺少可用 RAM 而崩溃。我发现减少使用的点数可以让程序无缝运行(因此以五分钟的间隔遍历数据帧)。

下一段代码显示了如何对我们到目前为止收集的数据进行整形、规范化、堆叠、拆分和标记。标准化输入是创建有效神经网络的关键步骤。如果没有标准化的输入，模型在计算损失和优化预测方面的效率会大大降低。Scipy 是 python 的一个科学库，它有大量的工具，它们的 z-score 工具可以很好地标准化数据。

分裂矩阵是一个特别适合于时间序列应用的卷积神经网络的过程。该函数在堆叠矩阵上滑动一个窗口，实质上是在窗口内所有值的每个时间步长拍摄快照。这个快照然后被堆叠到一个比输入矩阵大一个维度的数组中。这些快照充当 CNN 解读的图像。

```
**def** split_sequences(xs, ys, n_steps):
  *'''*
 *Takes an input sequence and splits it into multiple windows*
 *Input array will be shape [examples, features, 1]*
 *Output array will be shape [examples, time steps, features, 1]*
 *'''*
  X = np.zeros((xs.shape[0], n_steps, xs.shape[1], 1))
  Y = np.zeros((ys.shape[0], n_steps, 1))
  **for** i **in** range(len(xs)):
    end_ix = i + n_steps
    **if** end_ix == len(xs):
      **break**

    seq_x = xs[i:end_ix, :]
    seq_y = ys[i:end_ix]
    zeros = np.zeros((n_steps, 7, 1))

    **if** np.array_equal(seq_x, zeros):
      **break**
    X[i] = seq_x
    Y[i] = seq_y

  *#print(f'Split X Shape: {np.array(X).shape}')*
  *#print(f'Split Y Shape: {np.array(Y).shape}')*
  **return** np.array(X), np.array(Y)
```

```
macd = macd.reshape(df.shape[0], 1)
sma = sma.reshape(df.shape[0], 1)
ema = ema.reshape(df.shape[0], 1)
rsi = rsi.reshape(df.shape[0], 1)
bbu = bbu.reshape(df.shape[0], 1)
bbd = bbd.reshape(df.shape[0], 1)
price = df.reshape(df.shape[0], 1)
bin = np.zeros((df.shape[0], 1))

*#Normalize the data*
macd = scipy.stats.zscore(macd)
sma =  scipy.stats.zscore(sma)
ema = scipy.stats.zscore(ema)
rsi =  scipy.stats.zscore(rsi)

*#Create Binary Labels*
**for** i **in** range(df.shape[0]-1):
  **if** price[i+1] > price[i]:
    bin[i] = 1
  **elif** price[i+1] < price[i]:
    bin[i] = 0
  **else**: bin[i] = 1
  **if** i == df.shape[0]:
    bin[i] = 1

test_range = price.shape[0] - 100000
val_range = price.shape[0] - 50000

data = np.stack((macd,sma,ema,rsi,bbu,bbd,price), axis=1)
train = data[:test_range,...]
test = data[test_range:val_range,...]
val = data[val_range:,...]

*#Inputs*
X_train, _ = split_sequences(train, bin[:test_range], 32)
Y_train = bin[:test_range]
X_test, _ = split_sequences(test, bin[test_range:val_range], 32)
Y_test = bin[test_range:val_range]
X_val, _ = split_sequences(val, bin[val_range:], 32)
Y_val = bin[val_range:]
```

# 建立和训练 CNN

现在所有的输入都已经定义好了，是时候创建 CNN 了。我决定使用两个块的 CNN，每个块包含一个 Conv2D 和一个 MaxPool2D 层。然后，第二个模块的输出被平坦化，并被馈送到具有 s 形激活的单一密集单元中。这使得 CNN 可以预测二进制输出。我没有特别的理由使用内核大小和过滤器数量，因为我只是发现它们是有效的。每个卷积层也使用一个核正则化子。如果没有正则项，模型似乎会不断地找到局部极小值，并且不会收敛。然而，通过正则化，模型能够相当快地收敛。

```
**def** run_conv(X, Y):
  model = tf.keras.models.Sequential([
          tf.keras.layers.Conv2D(128, kernel_size=(7,16)            ,            padding='same'                                                                                                                        ,            kernel_regularizer=tf.keras.regularizers.l2(0.0001)),
          tf.keras.layers.MaxPool2D((2,2)),
          tf.keras.layers.Conv2D(32, kernel_size=(7,4)              ,            padding='same'                                         ,            kernel_regularizer=tf.keras.regularizers.l2(0.0001)),
          tf.keras.layers.MaxPool2D((2,2)),
          tf.keras.layers.Flatten(),
          tf.keras.layers.Dense(1, activation='sigmoid')
  ])

  model.compile(optimizer=tf.keras.optimizers.Adam(lr=0.001,)        ,               loss='binary_crossentropy', metrics=['accuracy'])
  history = model.fit(X,Y, validation_data = (X_val, Y_val)         ,               epochs=128, batch_size=32, callbacks=[callback])
  **return** history, modelhistory, model = run_conv(X_train, Y_train)
```

CNN 的损耗很快从 0.7%收敛到 0.2%，之后变得更慢。到第四或第五个纪元时，它已经将其准确度提高到 90%以上。虽然这些指标似乎表明取得了重大成功，但在模型训练时查看真实值和预测值也有助于衡量它是如何学习的。在模型编译时将这两个函数添加到度量列表中，将显示它在猜测什么以及它是如何学习的。

```
def true(y_true, y_pred):
  return y_true
def pred(y_true, y_pred):
  return y_predmodel.compile(optimizer=opt, loss=loss, metrics=['accuracy', true,     pred]
```

这是我参与过的最大的项目，它测试了我的知识和面对失败时的学习能力。我认为这个项目是成功的，因为该模型能够达到 97%的准确性，并在使用实时数据进行模拟交易时获得利润(我将很快就此写另一篇文章)。我对深度学习相当陌生，我知道这些代码中的大部分都可以简化和改进。因此，不要犹豫，主动提出建议、修改或新想法。

电子邮件:ajrogers3654@gmail.com

github:[https://github.com/ajrogers17](https://github.com/ajrogers17/Forex_Prediction/blob/main/Forex_Prediction.ipynb)

领英:【https://www.linkedin.com/in/alexrogers17/ 