# AWS:将 Python 库添加到 Lambda 层

> 原文：<https://medium.com/analytics-vidhya/aws-adding-python-libraries-to-lambda-layers-8c4eaf8fed80?source=collection_archive---------0----------------------->

![](img/3cb5ea2d25f1382f8428f052f4cb18b5.png)

克里斯里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**AWS Lambda** 是制作*短工作流*或*调度简单 CRON 作业*的方法，这些作业可能最多运行 **15 分钟**。最棒的是，人们不需要担心设置计算环境，因为它都是无服务器的。您甚至可以将它用作*调度器*来触发您的 ECS 或 AWS 批处理任务。可能还有很多其他的东西，但是我还没有探索它们。

Lambda 中的 Python 运行时自带一套[标准库](https://gist.github.com/gene1wood/4a052f39490fae00e0c3)。已经安装了很多库，比如 boto3、requests、e.t.c，你可以直接导入并创建你的 lambda 函数。但是，当你想使用不可用的库时，问题就出现了(*甚至熊猫也不可用*😐).

因此，现在要解决这个问题，可以采用以下任何一种方法:

1.  [**AWS 部署包**](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html) :将代码及其所有依赖项打包成一个 ZIP 文件，上传到 AWS。不要觉得这有用，因为你总是需要上传包，即使你每次都使用相同的库。当然，有点反直觉。
2.  **AWS Lambda Layers** :把依赖项打包成 ZIP 文件，上传到 AWS 就可以了。此外，一旦定义了一个层，任何数量的 Lambda 函数都可以使用它的所有底层依赖项。

我觉得后一种方法是最好的，因为它节省了大量的时间和空间，并提高了可重复性。我将从现在开始讨论这种方法。

> **请注意** : Lambda 使用亚马逊 linux 环境作为其 OS。因此，您在 Windows 笔记本电脑/PC 上安装的所有 python 包可能都无法在其上运行。安装在基于 Linux 的发行版(如 Ubuntu)上的软件包可以工作。Mac 应该也能工作但不确定。在我的 windows 机器上使用 [Docker](https://www.docker.com/products/docker-desktop) ，我将把安装在 Ubuntu 上的包上传到我的 lambda 层。所以，如果你还没有安装 Docker，我也建议你安装它。

现在，这就是我们开始将所需的包添加到 Lambda 层的地方。请原谅我离题了，但我认为给出一个概述总是更好的:)

只需按顺序执行以下步骤。突出显示的命令将在您的 CLI 中运行。

1.  `docker run ubuntu`:如果图像不存在，提取图像并运行容器。
2.  `apt update`:更新高级包工具。
3.  `apt install python3.8`:安装 Python(小心版本。我正在安装 3.8)。
4.  `apt install python3-pip`:安装 pip。
5.  `mkdir -p lambda_layer/python/lib/python3.8/site-packages`:创建一个安装软件包的目录(确保目录路径相同。第一个文件夹名可以由你选择，在我的例子中是“lambda_layer”。只是要谨慎 python 版本)。
6.  `pip3 install <package-name> -t lambda_layer/python/lib/python3.8/site-packages/`:将需要的 python 包安装到 made 目录下。对于多个包裹，按空间分开包裹。
7.  `apt-get install zip`:安装 zip(如果还没有的话)。
8.  `cd lambda_layer`:将光盘放入文件夹中制作。
9.  `zip -r python.zip *`:将所有安装好的包保存到一个 ZIP 文件中。
10.  在 pc 上并排打开一个新的命令提示符会话。
11.  `docker ps -a`:列出所有正在运行的容器。
12.  复制 ubuntu 映像的**容器 ID** 。它是一个字母数字，如 **79e37f5ff0db** 。
13.  `docker cp 79e37f5ff0db:/lambda_layer/python.zip C:\Users\anshul.sharma\aws`:使用以下约定将 zip 文件从 docker 容器复制到您的 PC—docker CP<CONTAINER _ ID>:<容器中 zip 的路径> <源目录>
14.  如果 **python.zip** 文件小于 **10MB** 直接上传到 lambda 层，否则先上传到 s3，定义 lambda 层时输入路径。

起初，这些可能看起来像很多步骤。但是，正如我所说的，这是一次性的活动，一旦上传，任何用户都可以使用这些库，只需将相应的层添加到他们的 lambda 函数中。

我希望这篇文章对你有所帮助。我花了很多时间试图找出一种方法来将库添加到 lambda 层，并考虑共享它。如有任何疑问，请随时联系我:)

## [LinkedIn](https://www.linkedin.com/in/anshuls235/)T21[Gmail](http://anshuls235@gmail.com)