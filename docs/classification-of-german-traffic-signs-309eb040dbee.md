# 德国交通标志的分类

> 原文：<https://medium.com/analytics-vidhya/classification-of-german-traffic-signs-309eb040dbee?source=collection_archive---------24----------------------->

![](img/4803cec7bf6cfc0c91336c31ce5630e0.png)

# 背景

随着自动驾驶汽车开发的研究继续进行，关键挑战之一是计算机视觉，使这些汽车能够从数字图像中了解他们的环境。特别是，这涉及到识别和区分路标的能力——停止标志、限速标志、让行标志等等。

在这个项目中，我将使用 TensorFlow 建立一个神经网络，根据这些标志的图像对路标进行分类。为此，我将使用一个带标签的数据集:一组已经按照路标分类的图像。

存在几个这样的数据集，但对于这个项目，我们将使用德国交通标志识别基准(GTSRB)数据集，它包含 43 种不同类型的道路标志的数千张图像。

# 模型

构建卷积神经网络，对 GTSRB 数据集提供的路标进行训练和分类。

**模型 1**

![](img/a1badd53800270d450e2fb5178a6c77e.png)

**图 1.1 —模型 1**

该模型包括一个利用 ReLu 激活的 32–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层和一个利用 softmax 激活的具有 43 个节点的最终输出层，以将输出分类为 43 种不同的路标。

```
tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(
        32, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dropout(0.50),
        tf.keras.layers.Dense(NUM_CATEGORIES, activation="softmax")
    ])
```

该模型的准确率为 96.96%。

![](img/223e5b9b8c9ddc2fc2855101b7fc8d9a.png)

**图 1.2 —模型 1:训练结果**

![](img/097803296a3ede4236211245951b23e7.png)

**图 1.3 —模型 1:模型总结**

模型 1 提供了 96.96 的精度，决定修改卷积层。

**型号 2**

![](img/3614e37714d82d5366635e63db99e858.png)

**图 2.1 —模型 2**

该模型包括一个利用 ReLu 激活的 64–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层和一个利用 softmax 激活的具有 43 个节点的最终输出层，以将输出分类为 43 种不同的路标。

该模型的准确率为 97.57%。

```
tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(
        64, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dropout(0.50),
        tf.keras.layers.Dense(NUM_CATEGORIES, activation="softmax")
    ])
```

![](img/9d86c52ee3adf0bb64a81151bd7dd4c7.png)

**图 2.2—模型 2:训练结果**

![](img/21f557ab14dbfa1433d4e9b77e502ef4.png)

**图 2.3 —模型 2:模型总结**

前两个模型产生的准确率不到 98%。修改了网络，看看能不能提高准确率。

**模型 3**

![](img/63bfdbc2bd561e80d7002fcd66f22747.png)

**图 3.1 —模型 3**

该模型包括一个利用 ReLu 激活的 32–3x 3 过滤器的卷积层，接着是另一个利用 ReLu 激活的 64–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层和一个利用 softmax 激活的具有 43 个节点的最终输出层，以将输出分类为 43 种不同的路标。

该模型的准确率为 99.11。

```
tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(
        32, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.Conv2D(
        64, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dropout(0.50),
        tf.keras.layers.Dense(NUM_CATEGORIES, activation="softmax")
    ])
```

![](img/c6ab8577cbb6220db954dfc542adcbe7.png)

**图 3.2 —模型 3:训练结果**

![](img/0799557cf50e7651e679c3468f1dbdbe.png)

**图 3.3 —模型 3:模型总结**

最后一个模型的准确率不到 99%。修改了网络，看看能不能用更少的参数提高准确率。

**模式四**

![](img/d95cb0f4558d688d7f4d8eea818a2d39.png)

**图 4.1 —模型 4**

该模型包括一个利用 ReLu 激活的 32–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是另一个利用 ReLu 激活的 64–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层和一个利用 softmax 激活的具有 43 个节点的最终输出层，以将输出分类为 43 种不同的路标。

该模型的准确率为 98.60。

```
tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(
        32, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Conv2D(
        64, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dropout(0.50),
        tf.keras.layers.Dense(NUM_CATEGORIES, activation="softmax")
    ])
```

![](img/e534fa4eb5fbb3b94965827c0d1fcd74.png)

**图 4.2 —模型 4:训练结果**

![](img/d5c5e8ba4fb501e3af52ff874257ca82.png)

**图 4.3 —模型 4:模型总结**

修改了网络，看看我是否可以通过引入另一个完全连接的密集层，用更少的参数来提高准确率。

**型号 5**

![](img/4a4a2fca2022db24bba71699eee19b31.png)

**图 5.1 —模型 5**

该模型包括一个利用 ReLu 激活的 32–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是另一个利用 ReLu 激活的 64–3x 3 过滤器的卷积层，接着是一个 2x2 的 MaxPooling 层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层，接着是一个利用 ReLu 激活的具有 128 个节点的全连接密集层，最后是一个利用 softmax 激活的具有 43 个节点的输出层，以将输出分类为 43 种不同的路标。

该模型的准确率为 98.32。

```
tf.keras.models.Sequential([
        tf.keras.layers.Conv2D(
        32, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Conv2D(
        64, (3, 3), activation='relu', input_shape=(IMG_WIDTH, IMG_HEIGHT, 3)),
        tf.keras.layers.MaxPooling2D(pool_size=(2, 2)),
        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dense(128, activation="relu"),
        tf.keras.layers.Dropout(0.50),
        tf.keras.layers.Dense(NUM_CATEGORIES, activation="softmax")
    ])
```

![](img/02e23dceaa3efcf122e328c828c81135.png)

**图 5.2 —模型 5:训练结果**

![](img/6a384161a275fd8a10aeb59e5ce8e029.png)

**图 5.3 —模型 5:模型总结**

使用 5x5 过滤器，用卷积层重复上述 5 个模型。最好的结果是模型三，准确率为 99.30

**型号 6**

![](img/78314d430f11bdfee8ce0e70c78dce7a.png)

**图 6.1 —模型 6**

![](img/cbfaf9dd4cd4fc40917806c552237bf8.png)

**图 6.2 —模型 6:训练结果**

![](img/59da3d04fd6fdefd9a737479393c4712.png)

**图 6.3 —模型 6:模型总结**

**型号 7**

![](img/5787ceaeae1c60ffe1207e359620f4b7.png)

**图 7.1 —模型 7**

![](img/e7526a3f9972c7d64eafc8043d986f7e.png)

**图 7.2 —模型 7:训练结果**

![](img/f8c36741ee7bed11548d3571eec4ff02.png)

**图 7.3 —模型 7:模型总结**

**型号 8**

![](img/23e806ab03f38b198961ba0f5731c9fc.png)

**图 8.1 —模型 8**

![](img/6e548d57dff80e4602fdd3494e691a9c.png)

**图 8.2 —模型 8:训练结果**

![](img/b760b632c6b55f9ad9aac10155742288.png)

**图 8.3 —模型 8:模型总结**

**型号 9**

![](img/a6e81125316c621c7f43e345293675b8.png)

**图 9.1 —模型 9**

![](img/a23c6941651e1b3bfb32670a9178cda0.png)

**图 9.2 —模型 9:训练结果**

![](img/e261f7cd20e3772ec7da7f7e788bed82.png)

**图 9.3 —模型 9:模型总结**

**型号 10**

![](img/42f04a97f73ee07d3283a629dde8e4ac.png)

**图 10.1 —模型 10**

![](img/22dc8a4c896c188ecdc28334228c1efe.png)

**图 10.2 —模型 10:训练结果**

![](img/abefd4065645f72ff95f527ae2e3ea88.png)

**图 10.3 —模型 10:模型总结**

# 结果和讨论

下表总结了卷积神经网络中各种组合的实验结果。

![](img/01a77d67d5047d6eac991f17651fd997.png)

我在卷积层使用了 5x5 滤镜，保持所有其他超参数不变，得到了更好的结果。

# 结论

使用模型 8 进行预测。

![](img/fa9098df2a03b4e435299a9676a2d01a.png)

**图 12 —利用模型 8** 的预测

*完整代码点击* [*此处*](https://github.com/Nbandhi/AI-ML/blob/main/Road-Signs/traffic.py)