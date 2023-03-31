# 在 Centos 7 上安装 tidygraph、ggraph R 库

> 原文：<https://medium.com/analytics-vidhya/install-tidygraph-ggraph-r-libraries-on-centos-7-80b8da748426?source=collection_archive---------11----------------------->

在 Centos 7 上安装 tidygraph 和图形库需要遵循的步骤顺序

![](img/8255901c1a93e321b2aa7f732c6303ae.png)

艾萨克·史密斯在 [Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

步骤 1:为 enterprise Linux 安装额外的包。

```
sudo yum install epel-release
```

步骤 2:安装必要的 c/c++ (gcc)编译器

```
sudo yum group install "Development Tools
```