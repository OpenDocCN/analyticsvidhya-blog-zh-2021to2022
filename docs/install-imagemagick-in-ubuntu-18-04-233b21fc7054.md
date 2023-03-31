# 在 Ubuntu 18.04 中安装 Imagemagick

> 原文：<https://medium.com/analytics-vidhya/install-imagemagick-in-ubuntu-18-04-233b21fc7054?source=collection_archive---------5----------------------->

本教程是关于在你的 ubuntu 上安装 imagemagick 的。好，我们开始吧

![](img/8e7409f2e104ef37e8128f66871fc9fc.png)

[叶夫根尼娅·利托夫琴科](https://unsplash.com/@grape_eve?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

互联网上有很多安装 imagemagick 的资源。但每次我挣扎着安装。有时它错过了依赖，有时错过了必要的委托，如 png，jpeg 等。

我们将从源代码安装它，这样我们就可以知道确切的版本。现在，按照以下步骤安装 Imagemagick `7.0.11-4`

换成自己喜欢的版本。

> `sudo apt-get update`

安装依赖项:

![](img/4853546eb430c84de707a2b0a1d777d7.png)

照片由[赛·基兰·阿纳加尼](https://unsplash.com/@_imkiran?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

> `sudo apt-get install build-essential checkinstall libx11-dev libxext-dev zlib1g-dev libpng12-dev libjpeg-dev libfreetype6-dev libxml2-dev`

现在，转到/opt 目录并下载最新版本的 imagemagick。

> `cd /opt`
> 
> `sudo wget [http://www.imagemagick.org/download/ImageMagick.tar.gz](http://www.imagemagick.org/download/ImageMagick.tar.gz)`

这将下载最新版本的 imagemagick。写这篇文章的时候是`7.0.11-4`

将其解压缩，并按照剩余步骤进行安装和配置。

> `sudo tar xvzf ImageMagick.tar.gz cd ImageMagick-7.0.11-4`
> 
> `sudo touch configure`
> 
> `sudo ./configure`
> 
> `sudo make sudo make install`
> 
> `sudo ldconfig /usr/local/lib`
> 
> `sudo make check`

您可以使用以下命令检查安装是否成功:

> `convert --version`

如果安装正确，您可以看到如下输出:

`Version: ImageMagick 7.0.11-4 Q16 x86_64 2018-06-24 https://www.imagemagick.org Copyright: © 1999-2018 ImageMagick Studio LLC License: https://www.imagemagick.org/script/license.php Features: Cipher DPC HDRI OpenMP Delegates **(**built-in**)**: bzlib djvu fontconfig freetype gvc jbig jng jpeg lcms lqr lzma openexr png tiff wmf x xml zlib`

万岁！您已经成功安装了 imagemagick。

![](img/4d6edeae05e86ee15d93d0644872fa52.png)

[粘土银行](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

感谢阅读，编码快乐。