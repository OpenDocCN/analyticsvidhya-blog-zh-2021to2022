# 使用 Tensorflow 训练模型—顺序 API 和子分类

> 原文：<https://medium.com/analytics-vidhya/training-a-model-with-tensorflow-sequential-api-and-sub-classing-de986b11288f?source=collection_archive---------15----------------------->

![](img/d311c9bd129170e049a6cbcda2d719e4.png)

本博客是使用 Tensorflow 2.0 训练模型的代码演练，以及使用 Keras 训练模型的两种不同技术的演练。

# 顺序 API

Keras 中的[序列类](https://keras.io/api/models/sequential/)可以帮助你快速原型化和训练模型。顾名思义，顺序模型只是不同的 [Keras 层](https://keras.io/api/layers/)的集合。顺序类将一系列线性层组合成一个 [Keras 模型](https://keras.io/api/models/model/)。

# 子分类

[子分类](https://www.tensorflow.org/guide/keras/custom_layers_and_models)方法让您可以自由定制 [Keras 层](https://keras.io/api/layers/)和[模型](https://keras.io/api/models/model/)。您可以创建自己的图层或使用现有图层，只需创建一个继承 Keras 图层类的自定义图层，还可以创建一个继承 Keras 模型类的自定义模型。

[子类](https://www.tensorflow.org/guide/keras/custom_layers_and_models)方法更多用于研究工作，也用于构建复杂模型，因为它有助于轻松定制神经网络的不同组件。

# 1.代码-顺序 API

在本演练中，我们将考虑 Cifar-10 数据集。要了解更多关于数据集的信息，请点击[此处](https://keras.io/api/datasets/cifar10/)。

**导入所有库**

```
import warnings
warnings.filterwarnings(‘ignore’)import numpy as np
import pandas as pdimport timeimport matplotlib.pyplot as plt
%matplotlib inlineimport tensorflow as tf
from tensorflow.keras.datasets import cifar10 
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow.keras.models import Sequential, Model, load_model
from tensorflow.keras.layers import Input, Conv2D, MaxPooling2D, Flatten, Dense, Layer, InputLayer
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.losses import SparseCategoricalCrossentropy
from tensorflow.keras.callbacks import ReduceLROnPlateau
```

**加载数据集**

```
(X_train, y_train), (X_test, y_test) = cifar10.load_data()
NUM_OUTPUTS = len(np.unique(y_train))
```

由于我们希望模型在数据批次中进行训练，因此我们定义了批次大小。

```
BATCH_SIZE = 128
```

**创建一个数据生成器**

我们需要一个数据生成器来根据批量大小提供数据。这里，我们将 [ImageDataGenerator](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator) 来准备训练和测试数据集。

```
train_dataset = ImageDataGenerator(rescale=1/255).flow(X_train, y_train, batch_size=BATCH_SIZE, shuffle=True)
test_dataset = ImageDataGenerator(rescale=1/255).flow(X_test, y_test, batch_size=BATCH_SIZE)
```

由于 X_train 和 X_test 都包含像素值范围从 0 到 255 的图像，我们使用 1/255 的重新缩放因子将像素从 0 缩放到 1。

**使用顺序 API 定义模型**

我们将使用 3 个卷积块，每个卷积块由一个具有 64 个过滤器和 3x3 内核大小的[卷积层](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv2D)和一个[最大池](https://www.tensorflow.org/api_docs/python/tf/keras/layers/MaxPool2D)层组成。

最后一个卷积与一个[展平](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Flatten)层连接，该层进一步与一个具有 128 个节点的[密集](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense)层连接，其后是一个具有 10 个节点的输出[密集](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense)层(因为我们的数据中有 10 个类，所以 NUM_OUTPUTS=10)和一个 [softmax](https://www.tensorflow.org/api_docs/python/tf/keras/activations/softmax) 激活层。

```
cifar_model = Sequential()
cifar_model.add(Input(shape=(32,32,3)))
# CONV-LAYER1
cifar_model.add(Conv2D(64, kernel_size=(3,3)))
cifar_model.add(MaxPooling2D())
# CONV-LAYER2
cifar_model.add(Conv2D(64, kernel_size=(3,3)))
cifar_model.add(MaxPooling2D())
# CONV-LAYER3
cifar_model.add(Conv2D(64, kernel_size=(3,3)))
cifar_model.add(MaxPooling2D())cifar_model.add(Flatten())
cifar_model.add(Dense(128, activation=’relu’))
cifar_model.add(Dense(NUM_OUTPUTS, activation=’softmax’))cifar_model.compile(optimizer=Adam(lr=1e-3), metrics=[‘accuracy’], loss=[‘sparse_categorical_crossentropy’])
cifar_model.summary()
```

**训练序列模型**

最后，我们定义模型将训练的时期的数量和每个时期的步骤。我们调用 fit_generator 函数并提供训练和测试数据。

```
EPOCHS = 10
STEPS_PER_EPOCHS = (X_train.shape[0] // BATCH_SIZE)cifar_model.fit_generator(train_dataset, steps_per_epoch=STEPS_PER_EPOCHS, epochs=EPOCHS, validation_data=test_dataset)
```

一旦模型训练完毕，我们就可以保存模型了。这就完成了我们使用顺序 API 的模型训练。

# **2。代码-子类方法**

对于顺序模型，我们创建了一个训练和测试数据集，它是 ImageDataGenerator 的对象。这里，我们将使用一个 [tensorflow 数据集](https://www.tensorflow.org/api_docs/python/tf/data/Dataset)对象来创建训练/测试数据集。

```
def parse_data(img, label):
 img = tf.cast(img, tf.float32)
 img = img/255.0
 return img,labeltrain_dataset = tf.data.Dataset.from_tensor_slices((X_train, y_train)).map(parse_data).batch(BATCH_SIZE)
test_dataset = tf.data.Dataset.from_tensor_slices((X_test, y_test)).map(parse_data).batch(BATCH_SIZE)
```

**卷积块**

在顺序 API 中，我们重复了 Conv2D 和 MaxPooling2D 层三次。这里，我们创建一个卷积模块，它由 Conv2D 层和 MaxPooling2D 层组成。然后我们将在模型类中使用这个块三次，就像我们之前做的那样。

```
class ConvolutionLayer(Layer):

 def __init__(self, filters, kernel_size):
     super(ConvolutionLayer, self).__init__()
     self.__conv = Conv2D(filters, kernel_size, activation=’relu’)
     self.__max_pool = MaxPooling2D()

 def call(self, x):
     x = self.__conv(x)
     return self.__max_pool(x)
```

ConvolutionLayer 类继承了 Keras 层类，在它的构造函数中我们初始化了父类和其他将要使用的层类。根据从单个卷积层到最大池层的数据流来覆盖调用方法。

**预测块**

如果您与顺序模型进行比较，数据将从最后一个最大池层到扁平化层，然后是两个密集层。这里，我们创建一个类似的系统来制作最终的预测块。

```
class HeadLayer(Layer): def __init__(self, num_outputs):
     super(HeadLayer, self).__init__()
     self.__flatten = Flatten()
     self.__dense1 = Dense(128, activation=’relu’)
     self.__classifier = Dense(num_outputs, activation=’softmax’)

 def call(self, x):
     x = self.__flatten(x)
     x = self.__dense1(x)
     return self.__classifier(x)
```

**自定义模型类**

现在，我们把所有东西都放在一个地方，然后把每一层都连在一起。首先，我们创建一个输入层并定义 input_shape 参数。然后是 3 个卷积模块，最后是预测模块。

```
class CifarModel(Model):

 def __init__(self, input_shape, filters, kernel_size, num_outputs):
     super(CifarModel, self).__init__()
     self.__input = InputLayer(input_shape=input_shape)
     self.__conv_layer_1 = ConvolutionLayer(filters, kernel_size)
     self.__conv_layer_2 = ConvolutionLayer(filters, kernel_size)
     self.__conv_layer_3 = ConvolutionLayer(filters, kernel_size)
     self.__classifier = HeadLayer(num_outputs)

 def call(self, x):
     x = self.__input(x)
     x = self.__conv_layer_1(x)
     x = self.__conv_layer_2(x)
     x = self.__conv_layer_3(x)
     return self.__classifier(x)
```

**初始化模型**

下一步，我们初始化 CifarModel 类，并将必要的参数传递给模型。

```
cifar_model = CifarModel(input_shape=(32,32,3), filters=64, kernel_size=(3,3), num_outputs=NUM_OUTPUTS)
```

# 3.代码—构建定制培训渠道

因为 CifarModel 类继承了 Keras 模型类，所以我们可以直接调用 fit 方法来训练我们的模型。但是，为了以不同的方式做事，我们将建立一个定制的培训渠道。

**预设训练参数**

我们希望模型被训练 10 个时期，我们也提到了训练/测试步骤的数量。我们选择优化器为[亚当](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers/Adam)，损失对象为[稀疏分类交叉熵](https://www.tensorflow.org/api_docs/python/tf/keras/losses/SparseCategoricalCrossentropy)。

```
EPOCHS = 10
TRAIN_STEPS_PER_EPOCHS = (X_train.shape[0] // BATCH_SIZE)
TEST_STEPS_PER_EPOCHS = (X_test.shape[0] // BATCH_SIZE)adam = Adam(learning_rate=1e-3)
loss_object = SparseCategoricalCrossentropy(from_logits=False)
```

**设置检查点管理器**

[tensor flow check point manager](https://www.tensorflow.org/api_docs/python/tf/train/CheckpointManager)帮助您在每个训练时期跟踪您的最佳模型。即使您的训练由于某种原因失败，您也可以从最后一个检查点恢复，并从该特定点开始训练。

因此，您提到 tensorflow 将保存模型的路径，并且在 checkpoint 类中，您提到在检查点期间需要存储哪个对象状态。我们将保存模型和优化器对象。

然后，我们将检查点对象和 checkpoint_path 一起传递给 [CheckpointManager](https://www.tensorflow.org/api_docs/python/tf/train/CheckpointManager) ，我们提到 max_to_keep = 1。因此，检查点管理器将在整个训练迭代中只存储一个最佳模型。

```
checkpoint_path = “../models/ckpts/cifar_10/”
ckpt = tf.train.Checkpoint(cifar_model=cifar_model, optimizer=adam)
ckpt_manager = tf.train.CheckpointManager(ckpt, checkpoint_path, max_to_keep=1)
```

**定义训练/测试步骤**

train_step 函数将一批图像作为输入。这些图像批次被传递到 cifar_model 对象，该对象返回一个大小为(BATCH_SIZE，NUM_OUTPUTS)的预测数组。

然后，我们借助损失函数计算损失，并使用损失来计算梯度。

损耗的梯度由 [tf 计算。GradientTape()](https://www.tensorflow.org/api_docs/python/tf/GradientTape) 记录自动微分操作的函数。

我们在 adam 优化器的帮助下更新参数(权重和偏差), Adam 优化器将梯度和可训练变量作为输入。

最后，我们返回总批量损失和单个训练步骤的平均损失。

```
[@tf](http://twitter.com/tf).function
def train_step(img, label):
    train_loss = 0
    with tf.GradientTape() as tape:
        pred = cifar_model(img)
        train_loss = loss_object(label, pred)
    avg_loss = (train_loss / int(label.shape[1]))
    trainable_variables = cifar_model.trainable_variables
    gradients = tape.gradient(train_loss, trainable_variables)
    adam.apply_gradients(zip(gradients, trainable_variables))
    return train_loss, avg_loss
```

测试或 val 步骤类似于训练步骤。唯一的区别是，我们没有为测试步骤实现梯度更新和优化器功能。

```
[@tf](http://twitter.com/tf).function
def val_step(img, label):
    val_loss = 0
    pred = cifar_model(img)
    val_loss = loss_object(label, pred)
    avg_loss = (val_loss / int(label.shape[1]))
    return val_loss, avg_lossdef validation_loss(test_dataset):
    total_loss = 0
    for idx, (img, label) in enumerate(test_dataset):
        batch_loss, t_loss = val_step(img, label)
        total_loss += t_loss
    avg_test_loss = total_loss/TEST_STEPS_PER_EPOCHS
    return avg_test_loss
```

**火车环线**

为了训练，我们循环通过每个时期，并且在每个时期内，我们分批循环通过完整的数据集。每批图像被传递给训练步骤函数，该函数返回训练损失。类似地，将测试数据集传递给 validation_loss 函数，该函数将在每个训练时期后给出验证损失。

如果一个时期之后的验证损失小于前一个时期，我们使用我们的检查点管理器对象来保存模型。

最后，我们绘制损失图，培训结束。

```
loss_plot = []
test_loss_plot = []best_test_loss=100for epoch in range(0, EPOCHS):
    start = time.time()
    total_loss = 0
    for (batch, (img_tensor, target)) in enumerate(train_dataset):
        batch_loss, t_loss = train_step(img_tensor, target)
        total_loss += t_loss

    avg_train_loss = total_loss / TRAIN_STEPS_PER_EPOCHS
    loss_plot.append(avg_train_loss.numpy()) 
    test_loss = validation_loss(test_dataset)
    test_loss_plot.append(test_loss.numpy())

    print (‘For epoch: {}, the train loss is {:.3f}, & test loss is {:.3f}’.format(epoch+1,avg_train_loss,test_loss))
    print (‘Time taken for 1 epoch {} sec\n’.format(time.time() — start))

    if test_loss < best_test_loss:
        print(‘Val loss has been reduced from %.3f to %.3f’ % (best_test_loss, test_loss))
        best_test_loss = test_loss
        ckpt_manager.save()

plt.plot(loss_plot)
plt.plot(test_loss_plot)
plt.xlabel(‘Epochs’)
plt.ylabel(‘Loss’)
plt.title(‘Loss Plot’)
plt.show()
```

# 结论

这就完成了我们如何使用顺序和子类方法来训练 Tensorflow 模型的完整演练。此外，我们还看到了定制模型培训渠道。实际上，我们用子类方法建立的模型非常简单，但是在像[图像字幕](https://www.tensorflow.org/tutorials/text/image_captioning)这样的真实用例中，你必须小心使用编码器-解码器模型，子类方法将是唯一的选择。

请在这里浏览这个博客[的 python 笔记本。](https://nbviewer.jupyter.org/github/iamrajatroy/Data-Science-Lab/blob/main/notebook/Tensorflow%20Sequential%20API%20and%20Subclassing.ipynb)