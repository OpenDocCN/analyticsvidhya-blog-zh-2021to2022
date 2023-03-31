# 为什么要对计算机视觉中的图像进行灰度处理？

> 原文：<https://medium.com/analytics-vidhya/why-grayscale-the-images-in-computer-vision-936091b734d5?source=collection_archive---------10----------------------->

![](img/1edf8620e5d1f8fca9b7839600f24011.png)

我知道这是一个非常基本的概念(确实如此)，但是我想从基础开始，然后写一些关于更高级内容的博客。

## 所以你想知道为什么吗？

您是否想知道为什么我们要对图像进行灰度处理，以完成边缘检测或从描述符中提取信息等任务？许多人认为彩色转灰度的方法没什么影响，但事实并非如此。

灰度图像**简化了**算法，**降低了**计算要求。作为颜色，增加了模型的复杂性，提供了不必要的信息。让我们尝试一个真正基本的灰度实现

我们可以尝试使用 OpenCv 的[**CVT color**](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html)**函数将图像转换成灰度**

**嗯，我知道它真的很短(但在我的辩护中，我真的涵盖了基本的主题)**

**PS:如果你有任何疑问，你可以给我发邮件[这里](http://pavankunchalapk@gmail.com)，你可以从 [**这里**](https://www.linkedin.com/in/pavan-kumar-reddy-kunchala/) 在我的 linkedin 上联系我，你也可以从 [**这里**](https://github.com/Pavankunchala) 在我的 Github 上查看我的其他代码(它有非常酷的东西)**

**我也在寻找深度学习和计算机视觉领域的自由职业机会，如果你愿意合作，请给我发邮件([pavankunchalapk@gmail.com](mailto:pavankunchalapk@gmail.com))**

**祝你有美好的一天！**