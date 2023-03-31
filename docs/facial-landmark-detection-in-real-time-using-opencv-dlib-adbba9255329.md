# 利用 OpenCV & Dlib 实时检测人脸标志点

> 原文：<https://medium.com/analytics-vidhya/facial-landmark-detection-in-real-time-using-opencv-dlib-adbba9255329?source=collection_archive---------6----------------------->

![](img/4112714f07e10b5fea7af25b63ee9f8d.png)

面部标志可以帮助我们预测许多事情，它可以用于情绪检测，痛苦检测，甚至是司机在驾驶时是否睡着了。我们将使用预训练的模型来检测面部周围的标志。

## **导入素材**

让我们导入所需的库如 **OpenCV、**[**dlib**](https://pypi.org/project/dlib/)**&numpy**

## 定义地标

我们将在该模型上绘制总共 68 个地标(我们也可以替换它，以便在边缘设备中实时检测仅 5 个地标)

## 绘制形状

现在让我们定义一些函数来画出地标的点和形状

## 模型和预测器

现在让我们初始化模型(我们有两个模型，一个用于检测 68 个地标，另一个用于 5 个地标，我将注释掉 5 个地标)。我们还将使用 **dlib 的形状预测函数**来检测给定人脸中的标志

## 启动摄像头

可以用 **Cv2。VideoCapture(0)** 用于访问您计算机中的网络摄像头，但为了方便起见，我在视频中使用了这一模式，我们将保存输出，但您也可以使用上述功能实时使用它。

## 运行并保存

让我们使用给定的视频输入来运行这个模型，我将把输出保存为***output.mp4***，但是您也可以使用 **cv2.imshow()** 函数来实时查看输出

你可以从 [**这里**](https://github.com/Pavankunchala/Deep-Learning/blob/master/Facial_landmarks/facial_landmarks_dlib.py) 找到博客的代码

**PS** :如果你有任何疑问可以在这里给我发邮件[，你可以在我的 linkedin 上从](http://pavankunchalapk@gmail.com/) [**这里**](https://www.linkedin.com/in/pavan-kumar-reddy-kunchala/) 联系我，你也可以在我的 Github 上从 [**这里**](https://github.com/Pavankunchala) 查看我的其他代码(里面有很酷的东西)

我也在寻找深度学习和计算机视觉领域的自由职业机会，如果你愿意合作，请给我发邮件([pavankunchalapk@gmail.com](mailto:pavankunchalapk@gmail.com))

祝你有美好的一天！