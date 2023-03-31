# 训练一个自定义的 YOLOv4 对象检测器(使用 Google Colab)

> 原文：<https://medium.com/analytics-vidhya/train-a-custom-yolov4-object-detector-using-google-colab-61a659d4868?source=collection_archive---------0----------------------->

![](img/7f6df591e0873bd47cd92c4ee3be30da.png)

## (初学者教程)

![](img/8d04d44b0dd60c31461fa0ada4c523be.png)

# **在本教程中，我们将**使用 YOLOv4 和 Darknet 训练我们的自定义检测器进行屏蔽检测

# **我在 YouTube 上的视频！**

# **如何开始？**

*   **✅Subscribe 到我的 YouTube 频道👉🏻【https://bit.ly/3Ap3sdi 😁😜**
*   **在你的浏览器上打开我的 [Colab 笔记本](https://colab.research.google.com/drive/1zqRb08ljHvIIMR4fgAXeNy1kUtjDU85B?usp=sharing)。**
*   **点击菜单栏中的**文件**并点击**在驱动器**中保存一份副本。这将在您的浏览器上打开我的 Colab 笔记本的副本，您现在可以使用它了。**
*   **接下来，打开我的笔记本副本并连接到 Google Colab VM 后，点击菜单栏中的**运行时**并点击**更改运行时类型**。选择 **GPU** 并点击保存。**

# **我已经在我的网站上上传了我定制的 YOLOv4 砝码，用于检测面膜。您可以将它用于测试目的。**

**[](https://techzizou.com/train-a-custom-yolov4-detector-using-google-colab-tutorial-for-beginners/#mask_detection_weights) [## 使用 Google Colab - TECHZIZOU 训练自定义的 YOLOv4 检测器

### 屏蔽检测权重

techzizou.com](https://techzizou.com/train-a-custom-yolov4-detector-using-google-colab-tutorial-for-beginners/#mask_detection_weights)** 

# **按照这 12 个步骤使用 YOLOv4 训练一个物体检测器**

**(**注意:**对于本 YOLOv4 教程，我们将在 google drive 的一个文件夹中克隆 Darknet git 存储库)**

1.  **[在你的 google drive](/p/61a659d4868#065e) 中创建***yolov 4*T24**和*培训*文件夹******
2.  **[挂载驱动器，链接你的文件夹，导航到 ***yolov4*** 文件夹](/p/61a659d4868#4894)**
3.  **[克隆**暗网** git 仓库](/p/61a659d4868#f7ec)**
4.  **[创建&将我们培训需要的文件(即“ **obj.zip** ”、“ **yolov4- custom.cfg** ”、“ **obj.data** ”、“ **obj.names** ”和“ **process.py** ”)上传到你的驱动器](/p/61a659d4868#4be1)**
5.  **[在 Makefile 中进行更改，以启用 **OPENCV** 和 **GPU** 和](/p/61a659d4868#a777)**
6.  **[运行 **make** 命令构建暗网](/p/61a659d4868#ba6f)**
7.  **[将 **obj.zip** 、 **yolov4-custom.cfg** 、 **obj.data** 、 **obj.names** 、 **process.py** 文件从 ***yolov4*** 文件夹复制到 ***darknet*** 目录](/p/61a659d4868#5316)**
8.  **[运行 **process.py** python 脚本创建**train . txt**&**test . txt**文件](/p/61a659d4868#b7b4)**
9.  **[下载预先训练好的 **YOLOv4** 重量](/p/61a659d4868#d4cc)**
10.  **[训练探测器](/p/61a659d4868#e5b4)**
11.  **[检查性能](/p/61a659d4868#1a83)**
12.  **[测试您的自定义对象检测器](/p/61a659d4868#4842)**

****注意:如果由于某种原因断开连接或丢失会话，每次都必须再次运行步骤 2、5 和 6 来挂载驱动器、编辑 makefile 并构建 darknet，否则 darknet 可执行文件将无法运行**。**

# **我们开始吧！！**

**![](img/fc5fd53fb380447de021ffb601856a0b.png)**

**来自 [Pexels](https://www.pexels.com/photo/4612297/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 原创视频**

# **1)在您的驱动器中创建“yolov4”和“training”文件夹**

**在你的 google drive 中创建一个名为 ***yolov4*** 的文件夹。接下来，在 ***yolov4*** 文件夹内创建另一个名为***training****的文件夹。这是我们将保存我们训练过的权重的地方(这个路径在我们稍后将上传的 ***obj.data*** 文件中提到)***

**![](img/af2c21d2b6254565749bcf2352865cf8.png)**

# **2)安装驱动器并导航到驱动器中的“yolov4”文件夹**

## **安装驱动器**

```
%cd ..
from google.colab import drive
drive.mount('/content/gdrive')
```

## **链接您的文件夹**

**运行以下命令创建一个符号链接，这样现在路径**/content/g Drive/My \ Drive/**等于 **/mydrive****

```
!ln -s /content/gdrive/My\ Drive/ /mydrive
```

****导航到/mydrive/yolov4 文件夹****

```
%cd /mydrive/yolov4
```

# **3)克隆 Darknet git 存储库**

**在您的驱动器上的 ***yolov4*** 文件夹中克隆 Darknet git 库。**

```
!git clone [https://github.com/AlexeyAB/darknet](https://github.com/AlexeyAB/darknet)
```

**![](img/aee072dc3ea5b14e7d1fc7bd4310bd2d.png)**

****克隆了 yolov4 文件夹中的 Darknet 库****

**您也可以从 Colab 中查看该文件夹，因为驱动器已经安装。见下图。**

**![](img/f830f968b931963e8321b82d017bb602.png)**

**在 Colab 文件部分**

# **4)创建并上传以下文件，我们需要这些文件来培训定制检测机**

```
**a. Labeled Custom Dataset
b. Custom cfg file
c. obj.data and obj.names files
d. process.py file (to create train.txt and test.txt files for training)**
```

**我已经在我的**[**GitHub**](https://github.com/techzizou/yolov4-custom_Training)**上传了我的用于遮罩检测的自定义文件。**我正在处理两个类，即“带 _ 掩码”和“不带 _ 掩码”。****

## ****标注数据集****

****输入图像示例(**Image1.jpg**)****

****![](img/e33752052dc77f2bf86cff17d66ee853.png)****

****[阿里·帕扎尼](https://www.pexels.com/@alipazani?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的原始照片来自 [Pexels](https://www.pexels.com/photo/close-up-photo-of-woman-biting-her-lower-lip-2878373/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)****

****您可以使用任何软件进行贴标，如 [**labelImg**](https://github.com/tzutalin/labelImg#labelimg) 工具。****

****![](img/8cf8d4f94d2ae48993d39d93c1552ff7.png)****

******Image1.jpg 标签图形用户界面******

****我使用一个叫做 **OpenLabeling** 的开源标签工具，它有一个非常简单的用户界面。****

****![](img/b8e332608d87e0722d3117a9a3ade322.png)****

******打开贴标工具 GUI******

****点击下面的链接，了解更多关于贴标过程和其他软件的信息:****

*   ****[**影像数据集标注介质条**](https://techzizou007.medium.com/image-dataset-labeling-annotation-bec3390eda2d)****

******注:**垃圾入=垃圾出。选择和标记图像是最重要的部分。尽量找质量好的图片。数据的质量在很大程度上决定了结果的质量。****

****输出的 YOLO 格式标签文件如下所示。****

****![](img/76f3e0ac41bd4e4a369dba2a09c5d8d1.png)****

******Image1.txt******

## ******4(a)创建带有标签的自定义数据集“obj.zip”文件，并上传至硬盘上的“yolov4”文件夹******

****把所有输入的图像”。jpg "文件及其对应的 YOLO 格式标注"。txt "文件放在名为 **obj** 的文件夹中。****

****创建它的 zip 文件 **obj.zip** 并上传到你硬盘上的 ***yolov4*** 文件夹。****

****![](img/255b2d4f57263ce57e9ad1a17571cd15.png)****

******包含输入图像文件和 YOLO 标签文本文件的 obj 文件夹******

## ******4(b)创建您的自定义配置文件，并将其上传到硬盘上的“yolov4”文件夹******

****从***darknet/CFG****目录*，*下载***yolov 4-custom . CFG***文件，并对其进行修改，上传到您硬盘上的****yolov 4/data***文件夹。******

******你也可以从 [AlexeyAB 的 Github](https://www.github.com/AlexeyAB/darknet) 下载定制配置文件。******

********在自定义配置文件中进行以下更改:********

******![](img/7e5171d397855b9171b5f1214d27dc24.png)******

*   ******将行批更改为批=64******
*   ******将线条细分改为细分=16******
*   ******设置网络大小宽度=416 高度=416 或 32 的任意倍数******
*   ******将行 max_batches 更改为(类*2000，但不小于训练图像数，且不小于 6000)，例如，如果训练 3 个类，max_batches=6000******
*   ******将生产线步骤更改为 max_batches 的 80%和 90%，即步骤=4800，5400******

******![](img/013669bf7a9f08093df6992f80d38c04.png)******

*   ******在每个[yolo]层之前的 **3** **【卷积】**中，将[filters=255]更改为 filters=(classes + 5)x3，请记住，它只需是每个[yolo]层之前的最后一个[卷积]。******
*   ******在每个**3【yolo】**层中，将线条类别=80 改为你的对象数量。******

******因此，如果 classes=1，那么它应该是 filters=18。如果 classes=2，则编写 filters=21。******

******一旦你理解了训练过程的基本原理，你也可以调整其他参数值，如学习率、角度、饱和度、曝光和色调。对于初学者来说，以上改动就足够了。******

## ********注:** **什么是细分？********

*   ******这是我们分成的许多小批量的数量。******
*   ******Batch=64 ->一次迭代加载 64 幅图像。******
*   ******细分=8 ->将批次分为 8 个小批次，因此每个小批次有 64/8 = 8 个图像，这 8 个图像被发送进行处理。该过程将被执行 8 次，直到该批完成，并且新的迭代将从 64 个新图像开始。******
*   ******如果您使用的是低内存的 GPU，请为细分设置一个较高的值(32 或 64)。这显然需要更长的时间来训练，因为我们正在减少加载的图像数量以及小批量的数量。******
*   ******如果您有一个高内存的 GPU，设置一个较低的细分值(16 或 8)。这将加速训练过程，因为每次迭代加载更多的图像。******

## ******4(c)创建您的“对象数据”和“对象名称”文件，并将它们上传到您的驱动器******

## ******对象数据******

*********obj.data*** 文件有:******

*   ******班级的数量。******
*   ******我们后面要创建的 ***train.txt*** 和 ***test.txt*** 文件的路径。******
*   ******包含类名的 ***obj.names*** 文件的路径。******
*   ******保存训练权重的 ***训练*** 文件夹的路径。******

## ******对象名称******

******包含对象的名称，每个名称占一行。确保分类的顺序与标记图像时使用的 class_list.txt 文件中的顺序相同，以便每个分类的索引 id 与标记的 YOLO 文本文件中提到的相同。******

## ******4(d)将<process.py>脚本文件上传到硬盘上的 yolov4 文件夹</process.py>******

******(将所有图像文件分成 2 部分。90%用于训练，10%用于测试)******

******这个 ***process.py*** 脚本创建文件***train . txt***&***test . txt***其中***train . txt****文件具有指向 90%图像的路径，而 ***test.txt*** 具有指向 10%图像的路径。*******

******可以从我的 [**GitHub**](https://github.com/techzizou/yolov4-custom_Training) **下载 ***process.py*** 脚本。********

********* *重要提示:“*process . py”*脚本只有。jpg "格式写在它的第 20 行，所以其他格式如"。png“，”。jpeg”，甚至是”。JPG”(大写)不会被承认。如果您正在使用任何其他格式，请在 *process.py* 脚本中进行相应的更改。********

********process.py 脚本********

******现在我们已经上传了所有的文件，我们驱动器上的***yolov 4****文件夹应该是这样的:*******

*******![](img/94fc153bca52e7de9acf53fc1e6f568b.png)*******

# *******5)在 makefile 中进行更改，以启用 OPENCV 和 GPU。*******

*******(还要将 CUDNN、CUDNN_HALF 和 LIBSO 设置为 1)*******

```
*****%cd darknet/
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile
!sed -i 's/CUDNN_HALF=0/CUDNN_HALF=1/' Makefile
!sed -i 's/LIBSO=0/LIBSO=1/' Makefile*****
```

# *******6)运行 make 命令构建暗网*******

```
*****!make*****
```

# *******7)将所有文件从'`yolov4'`文件夹复制到' darknet '目录*******

*******当前工作目录是**/my drive/yolov 4/darknet*********

*******清理 ***数据*** 和 ***cfg*** 文件夹，除了 ***数据*** 文件夹内的**标签**文件夹，这是在检测盒上写标签名称所需要的。*******

******因此，只需从 ***data*** 文件夹中删除所有其他文件，并彻底清理 ***cfg*** 文件夹，因为我们的驱动器上的 ***yolov4*** 文件夹中已经有了我们的自定义配置文件。******

********这一步是可选的。********

```
****%cd data/
!find -maxdepth 1 -type f -exec rm -rf {} \;
%cd ..%rm -rf cfg/
%mkdir cfg****
```

******7(a)解压 ***obj.zip*** 数据集及其内容，使其现在位于 **/darknet/data/** 文件夹中******

```
****!unzip /mydrive/yolov4/obj.zip -d data/****
```

******7(b)复制您的***yolov 4-custom . CFG***文件，使其现在位于 **/darknet/cfg/** 文件夹中******

```
****!cp /mydrive/yolov4/yolov4-custom.cfg cfg****
```

******7(c)复制 ***obj.names*** 和 ***obj.data*** 文件，使它们现在位于 **/darknet/data/** 文件夹中******

```
****!cp /mydrive/yolov4/obj.names data
!cp /mydrive/yolov4/obj.data  data****
```

******7(d)将 ***process.py*** 文件复制到当前 ***darknet*** 目录下******

```
****!cp /mydrive/yolov4/process.py .****
```

# ******8)运行 process.py python 脚本，在数据文件夹中创建 train.txt 和 test.txt 文件******

******当前工作目录是**/my drive/yolov 4/darknet********

```
****!python process.py****
```

******列出 ***数据*** 文件夹的内容，检查 ***train.txt*** 和 ***test.txt*** 文件是否已经创建。******

```
****!ls data/****
```

******上面的 ***process.py*** 脚本创建了两个文件 ***train.txt*** 和 ***test.txt*** ，其中 ***train.txt*** 有指向 90%图像的路径，而 ***test.txt*** 有指向 10%图像的路径。我正在使用的[***process . py***](https://github.com/techzizou/yolov4-custom_Training/blob/main/yolov4/process.py)脚本文件中写入了路径 ***data/obj*** ，因为当前工作目录是 **/mydrive/yolov4/darknet。在步骤 7(a)中，我们将图像解压缩到 ***data/obj*** 文件夹中。*****train . txt***和 ***test.txt*** 文件看起来如下所示。**********

******![](img/e4e2c5a630ca7d779e2a06aa00843e7f.png)******

********train.txt & test.txt 文件********

# ******9)下载预训练的 YOLOv4 重量******

******这里我们用迁移学习。我们没有从头开始训练模型，而是使用预先训练的 YOLOv4 权重，这些权重已经被训练到 137 个卷积层。运行以下命令下载 YOLOv4 预训练权重文件。******

```
****!wget [https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137](https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137)****
```

# ******10)培训******

## ******训练您的定制检测器******

******为了获得最佳结果，如果可能的话，当平均损失小于 0.05 或者至少持续低于 0.3 时，应该停止训练，否则训练模型，直到平均损失暂时没有任何显著变化。******

```
****./darknet detector train data/obj.data cfg/yolov4-custom.cfg yolov4.conv.137 -dont_show -map****
```

******这里的**图**参数给了我们 **M** 平均 **A** 平均 **P** 精度。**图**越高，越有利于物体检测。******

******你可以访问官方的 AlexeyAB Github 页面，它给出了关于何时停止训练的详细解释。点击下面的链接跳转到该部分。******

******[](https://github.com/AlexeyAB/darknet/#user-content-when-should-i-stop-training) [## AlexeyAB/darknet

### https://arxiv.org/abs/2004.10934 纸 YOLO v4:https://arxiv.org/abs/2011.08036 纸缩放 YOLO v4:用于复制…

github.com](https://github.com/AlexeyAB/darknet/#user-content-when-should-i-stop-training) 

**注意:如果您由于某种原因断开连接或丢失会话，您必须再次运行步骤 2、5 和 6 来挂载驱动器，编辑 makefile 并每次都构建 darknet，否则 darknet 可执行文件将无法工作。**

## 重新开始您的培训(以防培训未结束而断开连接)

![](img/e093410b227f63e0959bbf594a0842e1.png)

如果您断开连接或丢失会话，您不必再次从头开始训练模型。你可以从你停止的地方重新开始训练。使用上次保存的重量。每 100 次迭代将权重保存为驱动器上 **yolov4/training** 文件夹中的**yolov 4-custom _ last . weights***。(我们在“obj.data”文件中作为备份给出的路径)。*

**所以要重启训练运行步骤 2、5、6，然后运行以下命令:**

```
!./darknet detector train data/obj.data cfg/yolov4-custom.cfg /mydrive/yolov4/training/yolov4-custom_last.weights -dont_show -map
``` 

# ******使用这个简单的黑客自动点击，以避免被踢出 Colab 虚拟机******

******按(Ctrl + Shift + i)。转到控制台。粘贴以下代码，然后按 Enter 键。******

```
****function ClickConnect(){
console.log("Working"); 
document
  .querySelector('#top-toolbar > colab-connect-button')
  .shadowRoot.querySelector('#connect')
  .click() 
}
setInterval(ClickConnect,60000)****
```

# ******11)检查性能******

## ********定义助手功能 imShow********

```
****def imShow(path):import cv2
import matplotlib.pyplot as plt
%matplotlib inline
image = cv2.imread(path)
height, width = image.shape[:2]
resized_image = cv2.resize(image,(3*width, 3*height), interpolation = cv2.INTER_CUBIC)
fig = plt.gcf()
fig.set_size_inches(18, 10)
plt.axis(“off”)
plt.imshow(cv2.cvtColor(resized_image, cv2.COLOR_BGR2RGB))
plt.show()****
```

## ********查看培训图表********

******您可以通过查看***chart.png***文件来检查所有训练过的重量的性能。然而，***chart.png***文件仅在训练没有中断的情况下显示结果，即，如果您没有断开连接或丢失会话。如果从保存的点重新开始训练，这将不起作用。******

```
****imShow('chart.png')****
```

******如果这不起作用，还有其他方法来检查你的表现。其中之一是通过检查训练权重的地图。******

## ********检查地图(平均平均精度)********

******您可以检查每 1000 次迭代保存的所有权重的贴图，例如:- yolov4-custom_4000.weights、yolov4-custom_5000.weights、yolov4-custom_6000.weights 等等。这样，您可以找出哪个权重文件给你最好的结果。地图越高越好。******

******运行以下命令来检查特定已保存权重文件的映射，其中 **xxxx** 是其迭代编号。(例如:- 4000，5000，6000，…)******

```
****!./darknet detector map data/obj.data cfg/yolov4-custom.cfg /mydrive/yolov4/training/yolov4-custom_**xxxx**.weights -points 0****
```

# ******12)测试您的自定义对象检测器******

## ******对自定义配置文件进行更改，将其设置为测试模式******

*   ******将行批更改为批=1******
*   ******将线细分改为细分=1******

******您可以手动完成，也可以简单地运行下面的代码******

```
****%cd cfg
!sed -i 's/batch=64/batch=1/' yolov4-custom.cfg
!sed -i 's/subdivisions=16/subdivisions=1/' yolov4-custom.cfg
%cd ..****
```

## ******对图像运行检测器******

******上传一张图片到你的 google drive 进行测试。******

******使用此命令对图像运行您的自定义检测器。(阈值标志设置对象检测所需的最低精度)******

```
****!./darknet detector test data/obj.data cfg/yolov4-custom.cfg /mydrive/yolov4/training/yolov4-custom_best.weights /mydrive/mask_test_images/image1.jpg -thresh 0.3imShow('predictions.jpg')****
```

******![](img/bda615cad3b73ee137082221845f2b29.png)******

******来自 [Pexels](https://www.pexels.com/photo/men-putting-food-on-a-thermal-bag-4393665/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Norma Mortenson](https://www.pexels.com/@norma-mortenson?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的原始照片******

## ******对网络摄像头图像运行检测器******

******要在网络摄像头捕捉的图像上运行检测器，请运行以下代码。这是 Google Colab 为相机捕捉提供的代码片段，除了最后两行，它们像我们在上面的命令中所做的那样对保存的图像运行检测器。******

******![](img/872d3ab359117b0b1308ada1b0fc1242.png)******

********检测网络摄像头上的图像********

## ******对视频运行检测器******

******上传一段视频到你的 google drive 进行测试。******

******使用此命令对视频运行您的自定义检测器。(阈值标志设置对象检测所需的最低精度)******

```
****!./darknet detector demo data/obj.data cfg/yolov4-custom.cfg /mydrive/yolov4/training/yolov4-custom_best.weights -dont_show /mydrive/mask_test_videos/test1.mp4 -thresh 0.5 -i 0 -out_filename /mydrive/mask_test_videos/results1.avi****
```

******![](img/5a2dd8bec0917cacbff1f2a10c3b4a70.png)******

******来自 [Pexels](https://www.pexels.com/photo/woman-pushing-a-shopping-cart-full-of-toilet-papers-4318387/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Pavel Danilyuk](https://www.pexels.com/@pavel-danilyuk?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 原创视频******

******![](img/b939a6533b07fe9aad3883827dc176dd.png)******

******来自 [Pexels](https://www.pexels.com/photo/people-showing-how-to-wear-face-mask-while-the-man-is-being-funny-3960181/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 原创视频******

## ******在实时网络摄像头上运行检测器******

******首先，导入依赖项，定义助手函数，加载自定义的 YOLOv4 文件，然后在网络摄像头上运行检测器。******

******运行下面的代码。(**注**:将第 22 行的自定义文件调整为您的文件)******

******![](img/cb032c8bbe4a039dccf2cec8312a5900.png)******

********实时网络摄像头检测********

********注意:**我为掩模检测收集的数据集主要包含特写图像。你可以在网上搜索更多的长镜头图片。有很多网站可以下载有标签和无标签的数据集。我在数据集来源下面给出了一些链接。我也给出了一些掩膜数据集的链接。其中一些有超过 10，000 张图片。******

******虽然我们可以对我们的训练配置文件进行某些调整和更改，或者通过增强为每种类型的对象类向数据集添加更多图像，但我们必须小心，以免导致过度拟合，从而影响模型的准确性。******

******对于初学者，你可以简单地使用我上传到我的GitHub **上的配置文件开始。**我还上传了我的屏蔽图像数据集以及 YOLO 格式的标签文本文件，虽然这可能不是最好的，但会给你一个如何使用 YOLO 训练你自己的定制探测器模型的良好开端。你可以找到一个质量更好的带标签的数据集或者一个不带标签的数据集，以后自己标注。******

# ******我的 GitHub******

******我已经在下面的 GitHub 链接上上传了我的自定义 mask 数据集和训练自定义 YOLOv4 检测器所需的所有其他文件。******

******[](https://github.com/techzizou/yolov4-custom_Training) [## techzizou/yolov 4-自定义 _ 培训

### 该存储库中的 yolov4 文件夹包含所需的 4 个自定义文件。(即…

github.com](https://github.com/techzizou/yolov4-custom_Training) 

# 我的标注数据集(obj.zip)

[](https://www.kaggle.com/techzizou/labeled-mask-dataset-yolo-darknet) [## 带标签的掩膜数据集(YOLO 暗网)

### YOLO 格式注释

www.kaggle.com](https://www.kaggle.com/techzizou/labeled-mask-dataset-yolo-darknet) 

# 我的 Colab 笔记本

[](https://colab.research.google.com/drive/1zqRb08ljHvIIMR4fgAXeNy1kUtjDU85B?usp=sharing) [## 谷歌联合实验室

YOLOv4 定制培训教程](https://colab.research.google.com/drive/1zqRb08ljHvIIMR4fgAXeNy1kUtjDU85B?usp=sharing) 

# 如果你觉得这篇文章有帮助，请订阅我的 YouTube 频道，并考虑在 YouTube 或🖖媒体上支持我

[](https://www.youtube.com/techzizou) [## 泰克齐祖

### 创建人工智能、机器学习、深度学习、计算机视觉、物体检测、图像等方面的视频教程

www.youtube.com](https://www.youtube.com/techzizou) 

# 信用

## 参考

*   [阿列克谢 AB GitHub](https://github.com/AlexeyAB/darknet)
*   [pjreddie Github](https://github.com/pjreddie/darknet)
*   [代码 Github](https://github.com/theAIGuysCode/YOLOv4-Cloud-Tutorial)
*   [纸质 YOLOv4](https://arxiv.org/abs/2004.10934)
*   [pjreddie 站点](https://pjreddie.com/darknet/yolo/)

## 数据集源

您可以从下面提到的网站下载许多对象的数据集。这些网站还包含许多种类的对象的图像以及它们的多种格式的注释/标签，例如 YOLO _ 黑暗网文本文件和帕斯卡 _VOC XML 文件。

*   [通过谷歌打开图像数据集](https://storage.googleapis.com/openimages/web/index.html)
*   [Kaggle 数据集](https://www.kaggle.com/datasets)
*   [Roboflow 公共数据集](https://public.roboflow.com/)
*   [可视化数据数据集](https://www.visualdata.io/discovery)

## 屏蔽数据集源

我将这 3 个数据集用于我的标记数据集:

*   [般若 Github](https://github.com/prajnasb/observations)
*   [约瑟夫·尼尔森·罗博弗洛](https://public.roboflow.com/object-detection/mask-wearing)
*   [X-张洋 Github](https://github.com/X-zhangyang/Real-World-Masked-Face-Dataset)

更多掩膜数据集

*   Prasoonkottarathil Kaggle(20000 张图片)
*   Ashishjangra27 Kaggle (12000 张图片)
*   [Andrewmvd Kaggle](https://www.kaggle.com/andrewmvd/face-mask-detection)

## 视频源

*   [https://www.pexels.com/](https://www.pexels.com/)

# 我在 YouTube 上的视频！

## 别忘了留下👏

## 祝您愉快！！！✌

## ⚡♕·特奇佐·♕⚡

![](img/265e36124d8063ab6b38b85f071c4a20.png)

来自[像素](https://www.pexels.com/photo/a-young-woman-wearing-a-face-mask-4873251/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[前方无物](https://www.pexels.com/@ian-panelo?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的原始视频******