# Tensorflow 2.0 中的自定义回调函数(基本)

> 原文：<https://medium.com/analytics-vidhya/custom-callback-function-in-tensorflow-2-0-basic-dd0904a5f12d?source=collection_archive---------15----------------------->

如果你熟悉 Tensorflow，你应该听说过回调函数，所以这里有一个关于你自己写一个自定义函数的小博客。

![](img/ebd69066317f37fa2d05601eb728ed39.png)

## 回调函数到底是什么？

嗯根据 Tensorflow 官网

> 回调是一个强大的工具，可以在训练、评估或推断过程中定制 Keras 模型的行为。例子包括`[tf.keras.callbacks.TensorBoard](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/TensorBoard)`用 TensorBoard 可视化训练进度和结果，或者`[tf.keras.callbacks.ModelCheckpoint](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint)`在训练期间定期保存你的模型。

简而言之，这意味着您可以在满足条件时执行特殊的功能，例如在达到指定的精度后停止模型训练，或者保存模型以检查其进度。

既然已经定义了回调，我将演示一个自定义回调函数，该函数在模型达到 99%的准确率后停止(因为我使用的是经典的时尚 MNIST 数据集，所以准确率会很高)。

[](https://www.datadriveninvestor.com/2020/11/19/how-machine-learning-and-artificial-intelligence-changing-the-face-of-ecommerce/) [## 机器学习和人工智能如何改变电子商务的面貌？|数据驱动…

### 电子商务开发公司，现在，整合先进的客户体验到一个新的水平…

www.datadriveninvestor.com](https://www.datadriveninvestor.com/2020/11/19/how-machine-learning-and-artificial-intelligence-changing-the-face-of-ecommerce/) 

**我们开始进口**

我需要 TensorFlow API，以及创建一个密集的神经网络(DNN)模型的密集和扁平化层。

**自定义回调函数**

正如我上面提到的，回调函数在获得 99%的精确度后停止模型的训练

**导入、处理和创建模型**

就像标题一样导入 mnist 数据集，处理它，为它创建一个序列模型

## 编译并运行

我编译该模型，并将 Adam 算法定义为优化器，将稀疏分类交叉熵定义为损失函数。最后，我将模型与训练数据进行拟合。在这里，您可以将回调包含在一个列表中，并且可以使用我上面写的自定义回调。随着模型的训练，回调将具有内部状态的视图，并在满足其条件时触发。

你可以从 [**这里**](https://github.com/Pavankunchala/Deep-Learning/blob/master/Tensorflow_Basics/fashion_mnist.py) 找到博客的代码

结束了！(至少目前如此)

**PS** :如果你有任何疑问，你可以给我发邮件[这里](http://pavankunchalapk@gmail.com/)，你可以从[这里**在我的 linkedin 上联系我**](https://www.linkedin.com/in/pavan-kumar-reddy-kunchala/) ，你可以从 [**这里**](https://github.com/Pavankunchala) 在我的 Github 上查看我的其他代码(它有非常酷的东西)

我也在寻找深度学习和计算机视觉领域的自由职业机会，如果你愿意合作，请给我发邮件([pavankunchalapk@gmail.com](mailto:pavankunchalapk@gmail.com))

祝你有美好的一天！

获取专家视图— [**订阅 DDI 英特尔**](https://datadriveninvestor.com/ddi-intel)