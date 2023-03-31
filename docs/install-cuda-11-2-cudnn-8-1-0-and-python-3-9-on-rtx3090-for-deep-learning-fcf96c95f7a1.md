# 在 RTX3090 上安装 CUDA 11.2、cuDNN 8.1.0、PyTorch v1.8.0(或 v1.9.0)、python 3.9 进行深度学习

> 原文：<https://medium.com/analytics-vidhya/install-cuda-11-2-cudnn-8-1-0-and-python-3-9-on-rtx3090-for-deep-learning-fcf96c95f7a1?source=collection_archive---------0----------------------->

本教程是用 RTX3090 在 Ubuntu 20.04 LTS 上测试的。推荐的安装指南来自:[https://medium . com/@ dun . chwong/the-ultimate-guide-Ubuntu-18-04-37 BAE 511 efb 0](https://laptrinhx.com/link/?l=https%3A%2F%2Fmedium.com%2F%40dun.chwong%2Fthe-ultimate-guide-ubuntu-18-04-37bae511efb0)

# **1。配置**

运行以下命令安装 Ubuntu。

```
sudo apt-get update
sudo apt-get upgrade -ysudo apt-get install -y build-essential cmake unzip pkg-config
sudo apt-get install -y…
```