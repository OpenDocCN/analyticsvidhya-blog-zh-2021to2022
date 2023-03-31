# 计算机视觉和深度学习——第三部分

> 原文：<https://medium.com/analytics-vidhya/computer-vision-and-deep-learning-part-3-862173b3e249?source=collection_archive---------19----------------------->

# 简介:

欢迎回到这个以计算机视觉和深度学习为主要目标的机器人系列。在这篇文章中，我将向你介绍一些高级的图像处理概念，即图像平滑，形态变换，图像梯度和 Canny 边缘检测。在我们开始之前，相信我，这些都是简单的概念，如果你理解了一个，另一个就会变得可预测。

![](img/f77c80fd1b4bada47c9b2f5c9b02118c.png)

图 3.1 原图。图片来源: [Vix](https://www.vix.com/en/ovs/someone-there/58149/what-do-happy-kids-have-in-common)

考虑上面显示的一个快乐的孩子的图像，我们将使用它来理解这篇文章中的概念。

# 图像平滑:

![](img/1b49cfb8e812e839af3db09c8f0f7043.png)

图 3.2 上图所示的 10x10 像素强度值矩阵是灰度图像黄色框中值的粗略估计。矩阵的目的是提供更好的概念理解。

图像平滑用于去除图像中的噪声。为了消除噪声，我们需要了解什么是噪声。考虑图像 3.2，仔细观察图像中的黄色圆圈。你有没有发现他们有什么奇怪的地方？让我们考虑一下图像中亮度为 90 左右的黄色圆圈。像素周围的所有亮度都超过了 200，而这个像素却莫名其妙地得了 90 分！人们需要理解强度增加或减少的模式。如果没有模式和突然的小故障，那就是噪声。现在，考虑红色标记的像素强度。当阳光落在孩子的手臂上时，它显示为一条细白线，由 255 强度值表示。与左侧相比，红色标记区域右侧的值具有较低的强度值。此外，当你从右向左移动时(下方的蓝色箭头)，我们发现强度不断降低(图像变暗)。而当我们从右向左移动时(上方蓝色箭头),我们发现强度持续增加。在框架中引入新的实体之前，您总是会发现值的连续性。观察粉红色的矩形框，255 的左边是 230(帧中的 kid)，而右边是 124(绿色背景)，这种强度的突然变化(230 →124)具有连续性(像素强度在左边是 230 的顺序，在得分 124 值之后在右边连续下降)。因此，像素强度的显著变化是帧中新对象的边缘。因此，我们可以得出结论，如果强度的变化是无处不在的，则它是噪声，否则变化的像素强度意味着检测到新的边缘(边界)。

## 内核和 2D 卷积；

内核是与图像进行卷积以获得所需结果的过滤器。核的元素在确定用于平滑图像的技术中是至关重要的。考虑下面的图像来理解如何使用 2D 核执行 2D 卷积。

![](img/e45d434dbe410616ea72759e2de9536c.png)![](img/1f14e00c54565ea052919541afd50e28.png)

内核从图像的左端滑到右端(在图像的整个宽度上)

![](img/86a997a9f68952a217e7d3c88f58a87f.png)![](img/202c75899501c904dc9e0b2406adf1dd.png)

图 3.3 每完成一行后，内核向下滑动另一行，从左向右横向移动

为了更好地理解，内核显示为红色。内核的重叠元素的每个元素与相应的重叠像素相乘，并且从整个计算中获得的结果被放置在黄色像素中。这样，内核可以在图像的整个宽度和高度上滑动

## 图像平滑技术:

a) Average : cv2.blur(input_image，kernel _ dimension _ for _ Average)；这里，内核下所有像素值的平均值位于黄色像素处

b)高斯模糊:cv2。GaussianBlur(input_image，kernel_size，sigmaX[，output _ image _ for _ size _ reference[，sigmaY[，border type]])；要应用的高斯滤波器的细节。

c)中值滤波器:cv2.medianBlur(input_image，size _ of _ kernel)；有效去除椒盐噪声。获得 kernel 下的值的中值，并将其放在黄色像素的位置。

d)双边过滤:cv2.bilateralFilter(输入 _ 过滤，双边 _ 过滤 _ 宽度，西格玛 _ 颜色，西格玛 _ 空间)；双边滤波器用于在去除噪声的同时保留边缘。在空间域中使用高斯滤波器来去除噪声，并且使用乘法高斯滤波器(像素强度差的函数)来保留边缘。

![](img/37e1abe0b97a6cc408783e9603b46569.png)

图 3.4 噪声输入图像

```
import cv2
import numpy as npcv_image = cv2.imread("/home/rupali/tutorials/noise.jpg")
average_filter = cv2.blur(cv_image,(5,5))
gaussian_filter = cv2.GaussianBlur(cv_image,(5,5),0)
median_filter = cv2.medianBlur(cv_image, 5)
bilateral_f = cv2.bilateralFilter(cv_image,9,75,75)cv2.imshow("original_image", cv_image)
cv2.imshow("Average Filter", average_filter)
cv2.imshow("Gaussian Filter", gaussian_filter)
cv2.imshow("Median Filter", median_filter)
cv2.imshow("Bilateral Filter", bilateral_f)cv2.waitKey(1000)
```

![](img/7aee3f65d65fef3e8deca16987c68d1c.png)![](img/3a724f5a124ea7788ed0caad8f262c40.png)

图 3.5 左平均滤镜，右高斯滤镜

![](img/2706086ca3fd8e73b281fb9c3ae69eb8.png)![](img/4a85acb2dd60c01b1e46131b3e50588f.png)

图 3.6 左侧中值滤波器，右侧双边滤波器(边缘明显保留)

# 形态转换:

对图像执行形态学变换以增强其纹理。核与图像的 2D 卷积是这些操作的关键。结构化元素(内核)可以是矩形、椭圆形、菱形或圆形，这取决于应用。在下面的代码中，我们使用了一个填充了 1 的 5x5 内核。

```
import cv2
import numpy as npcv_image = cv2.imread("/home/rupali/tutorials/noise.jpg")
# you can select kernel of your choice, a structuring element can be a circle or ellipse
kernel = np.ones((5,5),np.uint8)
#erosion keeps zero at the yellow pixel(refer to the 2D convolve concept) on convolving the kernel if all image pixels under the kernel are not 1
ero = cv2.erode(cv_image,kernel,iterations = 1)
#Dilation is completely opposite. Even if just one pixel of the image under the kernel is 1, it keeps 1 at the yellow coloured pixel of the kernel
dil = cv2.dilate(cv_image,kernel, iterations = 1)
# open = first erosion and then dilation; opening decreases noise in the image
open_ = cv2.morphologyEx(cv_image, cv2.MORPH_OPEN, kernel)
#close = first dilation and then erosion ; 
close_=cv2.morphologyEx(cv_image, cv2.MORPH_CLOSE, kernel)
#morphological gradient; gives outline of the image
gradient = cv2.morphologyEx(open_, cv2.MORPH_GRADIENT, kernel)cv2.imshow("erosion", ero)
cv2.imshow("Dilate", dil)
cv2.imshow("opening", open_)
cv2.imshow("closing", close_)
cv2.imshow("Gradient", gradient)
cv2.waitKey(1000)
```

结果:

![](img/8b0d4adc4cfe458b74d36c5b404770e7.png)![](img/3965439d1ff805696bcc75f9fa3442de.png)![](img/ad065368623591d12c0733e4cf8fc828.png)

图像 3.7(从左开始)噪声输入图像、腐蚀结果、膨胀结果

![](img/9a4db0a9cf7d5b807af2e796c63084cc.png)![](img/892ce9ecadd326abc8f32e5101e5a5bb.png)![](img/75f953eff165e9c8ab01b71e8f4e4018.png)

图 3.8(从左开始)打开结果、关闭结果、渐变结果

# 图像渐变:

![](img/9134a7d9c130c62594c4db75ebda2403.png)

图 3.9 概念求斜率为 dy/dx。图片来源: [VivaxSolutions](https://www.vivaxsolutions.com/maths/aldifferentiation.aspx)

梯度是坡度的另一个术语。回想一下寻找斜率 dy/dx 的概念，即 x 方向上的每一个微小变化都会引起 y 方向幅度的变化。在图像中，为了找到梯度，我们将像素强度的变化视为 x 方向或 y 方向上的微小变化。但是有人可能会问，为什么我们要学习梯度/斜率/导数？答案是边缘。每当像素强度值发生急剧变化时(如图像平滑主题中所述)，即斜率“良好”，这意味着存在边缘。OpenCV 中有各种内置函数可以用来寻找图像中的梯度，即 cv2。索贝尔()，cv2。Scharr()，cv2。拉普拉斯算子()。
这些功能都是高通滤波器，即限制低频数据，允许高频数据。就图像而言，高通滤波器是那些通过增强图像中亮度或暗度有较大变化的对比度并忽略亮度或暗度的微小变化来帮助锐化图像的滤波器。因此，这些函数有助于检测图像中的“好”斜率。
索贝尔函数 2D 将输入图像与索贝尔核进行卷积，以获得输入图像的导数。它首先高斯平滑图像，然后寻找导数。特别是，为了获得精确的导数，Scharr 引入了 3x3 内核。通过指定 x 和 y 的顺序，我们可以求出 x 方向和 y 方向的导数。

## 索贝尔·沙尔衍生代码:

```
import cv2
import numpy as np #input image in gray scale
cv_image = cv2.imread('/home/rupali/tutorials/happy_kid.jpg',0)#x order is 1, y order is 1 and ksize = -1 implies using scharr kernel
sobel = cv2.Sobel(cv_image, cv2.CV_64F, 1,1,ksize = 3)cv2.imshow("sobel", sobel)
cv2.imwrite("sobel.jpg", sobel)cv2.waitKey(1000)
```

## 结果:

![](img/7ee8bd62575fdabc75ecaf2f0264f9eb.png)![](img/507dee45712553f445e27d0f55c821d8.png)

图像 3.10 原始图像及其 Sobel 导数

# 拉普拉斯导数；

![](img/65012a192ff4eb35ccb5fedfd00010c8.png)

用于计算 x 和 y 的双导数的公式

Opencv，cv2 中 2D 函数的拉普拉斯算子。使用拉普拉斯()函数。有人可能会问，为什么要用二重导数来寻找边缘，而用一次微分就可以了？原因是二阶导数对精细细节(即细线)有更强的响应(这就是为什么我们首先平滑图像，然后应用拉普拉斯算子)。二阶导数在灰度级的阶跃变化处产生双重响应(这有助于检测过零)。

![](img/79e6e3b6db35cc3f4cf30d5a4ab7f897.png)

图 3.11 图像二阶导数的零交叉。图片来源: [mipav.cit](https://mipav.cit.nih.gov/pubwiki/index.php/Edge_Detection:_Zero_X_Laplacian)

考虑图 3.11 及其一阶和二阶导数。在二进制图像的强度分布中，暗和亮之间的过渡以斜率的形式表示。当从暗到亮的过渡时，斜率被认为是正的，反之则是负的。在一阶导数中，这些边缘(黑与白相遇的地方)以正(黑到白)和负(白到黑)尖峰的形式表示。当我们对同一个函数求二阶导数时，我们会在图像中出现边缘的任何地方找到过零点。为了成功地增强图像，我们需要结合多种技术以获得最佳效果。没有一种技术优于另一种技术，图像中的细节决定了什么样的组合会产生最佳的增强效果。

为了成功的图像增强，我们需要结合技术以获得最佳效果。没有一种技术优于另一种技术，图像中的细节决定了什么样的组合会产生最佳的增强效果。

## 拉普拉斯码:

```
import cv2
import numpy as npcv_image = cv2.imread("/home/rupali/tutorials/happy_kid.jpg")
cv_image = cv2.GaussianBlur(cv_image,(3,3),0)
#resultant image is stored in the datatype 64F or 16S in order to retain positive as well as the negative values. If we directly store the output in 8U format then the negative slope values of an image will be lost
laplace = cv2.Laplacian(cv_image, cv2.CV_64F)cv2.imshow("Laplace_result", laplace)
cv2.imwrite("Laplace.jpg", laplace)
cv2.waitKey(10000)
```

![](img/7ee8bd62575fdabc75ecaf2f0264f9eb.png)![](img/93750bc1a1d4ea1d2313c5fcf88c9a03.png)

图像 3.12 原始图像及其拉普拉斯导数

# Canny 边缘检测:

就像我们遇到的其他边缘检测技术一样，Canny 边缘检测首先去除噪声，然后检测边缘。这项技术的创新之处在于它可以用来判断一个边缘是真实边缘还是一组噪声像素。阈值技术旨在确保不必要的区域不包括在边缘中，并且边缘应该尽可能地这样。参考梯度检测图(图 3.9)，梯度的角度为 arctan(dy/dx)。

![](img/dbf9cf547c5bb6cf5f49ea7243d33d84.png)![](img/7c7674b083c5de2e21a29fc165cbf019.png)

图 3.13 非最大抑制(左)和滞后阈值(右)。图片来源: [Opencv-python-tutorials](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html)

考虑图像 3.13，同时考虑渐变的方向(如通过 CAB 的箭头所示)，我们需要确定 C 点和 B 点的区域是否应该包含在边缘表示中(在二值图像中使其为白色)。为此，参考滞后阈值，两种灰度被用作阈值的上限和下限。如果任何像素的灰度值高于上限(maxVal ),则该像素被赋值为 1(白色),如果低于下限(minVal ),则该像素被赋值为零。如果像素中的强度值在上限和下限之间，那么连通性的概念就出现了。考虑图像 3.13(右)，点 C 连接到点 A(确定边缘)，因此点 C 将被分配 1(白色)，点 B 不连接到任何确定边缘像素将被赋予值零(黑色)。

## Canny 边缘检测代码:

```
import cv2
import numpy as npcv_image = cv2.imread("/home/rupali/tutorials/happy_kid.jpg", 0)
#canny function keeps in account the gaussian blurring and edge detection
canny_result = cv2.Canny(cv_image,100,200)
# 100 is lower limit of threshold and 200 is upper limit
cv2.imshow("Canny Result", canny_result)
cv2.imwrite("Canny.jpg", canny_result)
cv2.waitKey(1000)
```

## 结果:

![](img/7ee8bd62575fdabc75ecaf2f0264f9eb.png)![](img/f0606e8ca2618de136d8608bece9431a.png)

图 3.14 原始图像及其 Canny 检测结果

# 结论:

我很佩服你坚持一口气把以上都搞定！我知道这并不容易，但这种努力是值得的，这是本系列接下来的内容。今天，简单地说，我们学会了如何通过去除噪声和勾勒图像内容的边界来增强图像。在接下来的系列中，我们将提升计算机视觉的复杂程度。敬请关注，继续学习！