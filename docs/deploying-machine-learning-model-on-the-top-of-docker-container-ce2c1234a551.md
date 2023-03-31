# 在 Docker 容器顶部部署机器学习模型

> 原文：<https://medium.com/analytics-vidhya/deploying-machine-learning-model-on-the-top-of-docker-container-ce2c1234a551?source=collection_archive---------19----------------------->

![](img/67ae444262df03810cd5c78b816b37ac.png)

在本文中，我将创建一个机器学习模型，并将该模型部署在 docker 容器的顶部。

**什么是机器学习？**

**机器学习**属于人工智能(AI)的范畴，它为系统提供了自动学习和根据经验改进的能力，而无需显式编程。机器学习专注于开发可以访问数据并使用数据进行自我学习的计算机程序。

**什么是 Docker？**

Docker 是一组平台即服务(PaaS)产品，使用操作系统级虚拟化来交付称为容器的软件包中的软件。容器是相互隔离的，捆绑了它们自己的软件、库和配置文件；他们可以通过明确定义的渠道相互交流。因为所有容器共享单个操作系统内核的服务，所以它们比虚拟机使用更少的资源。

—任务描述📄

👉从 DockerHub 中提取 CentOS image 的 Docker 容器映像，并创建一个新的容器
👉在 docker 容器顶部安装 Python 软件
👉在容器中，您需要复制/创建您在 Jupyter 笔记本中创建的机器学习模型

![](img/1be7f41bad84c60eb8cdef0a80855b06.png)

让我们开始，我将一步一步地解释整个过程-

**步骤 1** —在这里，我使用 Jupyter Notebook 来创建一个机器学习模型，该模型具有预测估计工资的工资数据集。

![](img/210c5dca2356f73eb35db323922735f7.png)![](img/afdd8dad6fe6000b6101888b8683ed90.png)![](img/adb183c132c53d0080e88b737dc577ec.png)![](img/7651648cb7e2f64012b5fd995997b9a2.png)

我们成功地创建了一个工资预测模型。我们的下一步是在 docker 容器上部署这个模型

**步骤 2 —** 在 RHEL 顶部安装码头工人

1.  我们使用的是我们的 BaseOs——red hat Linux-8。
2.  转到***$ CD/etc/yum . repos . d***并创建一个 docker 库。

![](img/d9c608dc9939ab22c73190c614b043a4.png)![](img/be33af424404b8331a2acbdf42f653bd.png)

现在，我们已经创建了一个 docker repo。

![](img/989dd4fafa565c36e6ae6eb0d1850da7.png)![](img/13a77e911293e06d55872d802f91dc8d.png)

通过使用 ***$yum 安装 docker-ce -y — nobest*** 安装 docker。

![](img/0d6755b400ba789f561dbe17dee679eb.png)![](img/008f860d2142037fc9b04ff9c98c31b7.png)

docker 安装部分已经完成，现在检查 docker 服务是否正在运行如果没有运行，启动并启用 Docker 服务。

![](img/efb9da3f57f9fab550ce584585b0c7f9.png)![](img/3c533ae3705b4ac2df79abffd0f36a37.png)![](img/a72ba0909a391d03c767a2d48d17c1b0.png)

现在，通过***$ docker images***cmd 我们可以查看 docker images 的数量。

![](img/9fddca0ac1ee2c0dfe3f6c673c7d46bc.png)

通过***$ docker PS***cmd 我们可以检查正在运行的 docker os。

![](img/882af1f663c6e387516e51a9884edb02.png)

现在，我们将从 DockerHub 中提取 CentOS image 的 Docker 容器映像，并创建一个新容器。

cmd—***$ docker pull centos:最新***

![](img/c159a8d680c1ddaa9ffacdaf6c2a67d5.png)

现在我们用***$ docker run-I-t—name MLOPS centos:latest(I—interactive，t- terminal)*** 推出新的 os

![](img/8eedcfe4510e4a34051bb055681e0715.png)

***$ yum install net-tools*—**这样，我们就可以在 docker os 上得到 ifconfig cmd。

现在，我们已经在 RHEL 的顶部启动了 Centos 操作系统，我们可以用这两个操作系统 IP 进行验证。

![](img/70412e3a8ae7cf259912ad62edd1418a.png)

基本操作系统的 IP

![](img/c118c53f3e5f860f73b2cdbda6e79a47.png)

Docker 容器操作系统的 IP

现在，我们可以从 baseOs 查看我们的 **MLOPS docker 容器**的发布。

![](img/f37be0deec44717334be5686c38389a7.png)

**步骤 3 —** 在 docker 容器顶部安装 Python 软件。

cmd used-***$ yum install python 3***

![](img/abf2910db9b50a84ccfd9c79e87f0e81.png)

现在，我们可以验证 **python3** 是否安装在 docker 容器上。

![](img/b17dfc2af32c1411f6277ce1b544e6b0.png)

安装 python3 后，我们需要安装一些 ML 模型所需的库。

1.  ***$pip3 安装熊猫***
2.  ***$pip3 安装 sklearn***
3.  ***$pip3 安装数量***

![](img/335de20f97185a0ddf25fa8fd786c341.png)![](img/106bd815106941ed293e4f6b4ff6a7c2.png)![](img/2e94362207ccef77e023a3be3214558a.png)

现在，我们可以通过— ***$pip3 list*** 来查看我们的库列表

![](img/3f7367ef9d1c728df72afa2dcc2a8e5f.png)

**步骤 4 —** 在容器中，您需要复制/创建您在 Jupyter 笔记本中创建的机器学习模型。

在我的基本操作系统(RHEL-8)即 Docker 主机中，我已经为**工资预测**创建了机器学习模型，该模型基于使用数据集 **Salary_Data.csv** 的**简单线性回归模型**

现在，我将在我的 docker os 中创建一个工作区，在那里我将复制我的机器学习模型。

cmd—***$ mkdir task 1***

![](img/d7dda45c1e555eb345a38b749c79f13d.png)

现在，我们必须将训练好的模型复制到正在运行的 docker 容器中。

![](img/241dbe853992572b107061caa38811ac.png)

所以我们现在在 docker 容器中有了我们的模型。

![](img/cf8f1779321e89e257c730a57f97d505.png)

现在，我们必须创建一个 python 程序，在这个程序中，我们在 docker 容器的顶部加载并执行模型。

**$vi salarymodel.py (vi — cmd 行编辑器)**

![](img/4044e2e1f06a81ddf462fe64fa9c585d.png)

现在，我们的最后一步是运行***$ python 3 salary model . py***文件。

![](img/814638e6766ee9c036b330912de09d03.png)

Github 链接-[https://github.com/NiketHub/salarypredicton_model.git](https://github.com/NiketHub/salarypredicton_model.git)

谢谢你..！！