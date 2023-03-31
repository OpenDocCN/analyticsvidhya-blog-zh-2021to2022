# 在虚拟机(VMware)上安装 Red Hat Enterprise Linux

> 原文：<https://medium.com/analytics-vidhya/installation-of-red-hat-enterprise-linux-on-virtual-machine-vmware-372339fab02d?source=collection_archive---------14----------------------->

## 简介:

Red Hat Enterprise Linux (RHEL)是为商业市场开发的 Linux 操作系统的发行版。RHEL 以前被称为红帽 Linux 高级服务器。

RHEL 操作系统(OS)支持物理、[虚拟化](https://searchservervirtualization.techtarget.com/definition/virtualization)和[云](https://searchcloudcomputing.techtarget.com/definition/cloud-computing)环境中的各种工作负载。RHEL 版本适用于服务器、大型机、SAP 应用、台式机和 OpenStack。

## **让我们开始安装:**

我们必须做三件基本事情:

1.  创建 Linux 虚拟机。
2.  设备和 DNS 配置。
3.  YUM 配置。

*   **首先安装 VMware workstation，然后单击创建新虚拟机选项。**

![](img/d63584fa522f0f8ccb29195f16686272.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **然后选择典型选项，然后点击下一步。**

![](img/d901443664ba829648f30c9ed510b45b.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在选择第二个选项“已安装的光盘镜像文件”,然后浏览 linux ISO 文件，然后单击下一步。**

![](img/46cd99e7a5385973fec27900a1a262e0.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **提供您将在用户和 root 帐户中使用的用户名和密码。**

![](img/1bca064e0afe4bb72df3484032c3cd79.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在输入虚拟机名称，它将自动使用虚拟机的默认路径。**

![](img/03d7f3f88920bc1bfe82d87a546a9943.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **设置磁盘空间，选择第一个选项将虚拟磁盘存储在单个文件上，然后单击下一个。**

![](img/d7f3cac3449d1fc1ce0b878fae5a3274.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在点击完成**

![](img/422c7391771d1464119a4d8b6afa84d3.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   现在您的虚拟机正在安装。

![](img/41fa966e90c414fd8923de673ad9110b.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

![](img/6f1d03ec5bcdf56a1d3cb73fdb7aea3d.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

![](img/77c8b9c2073a9b32a1aa4360ffb7cc4c.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在选择‘其他’。**

![](img/ff41e2b81dfcdd2843441e9ea1a95f8d.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **输入 root 用户名，选择登录，然后输入您之前在安装时输入的密码**

![](img/2ef808f4e1f6cf744fe9b6e52ac472c6.png)

图片来源:【https://medium.com/@ankitgupta_974 

*   **现在右击虚拟机窗口并选择终端，现在终端窗口打开。使用 setup 命令并按 enter 键。**

![](img/7f91647c5bf2605f2bc4e1d1c66308f2.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在，选择防火墙配置回车。**

![](img/484942b7e424d6cf991504f34925d148.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **进入启用选项，按空格键，然后选择确定。**

![](img/da99ff32b7702bc842f700fe2e156f96.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **点击是按钮。**

![](img/b81cb610b64ed426cbe86b6fd3d9bfbf.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **选择网络配置并选择运行工具。**

![](img/03a728053420c1d7668f2460b73d1d55.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **选择设备配置，按保存并退出按钮。**

![](img/e23770acca9467e87f300bed29c9b97d.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **选择下图所示的第一个选项。**

![](img/66913ee62f8ed475e87e9597a5925822.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **写下主机名、主 DNS、辅助 DNS，然后点击确定按钮。重新启动虚拟机..**

![](img/a52ec0d6fbb68d6e12c42b1d9543e3ce.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **提供静态 IP，该 IP 就是您的虚拟机 IP。设置默认网关，然后按“确定”按钮。**

![](img/4006b961f01c0e22104fb6346bd61c43.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **进入虚拟机— >点击设置— >选择 CD/DVD(SDATA) — >选择“用作 ISO 镜像”选项，然后浏览 RHEL 的 ISO 镜像。在“设备状态”中，勾选“已连接”和“开机时已连接”复选框。**

![](img/1287ad3b0aabe0cf3d740b6953fbf3c5.png)

图片来源:【https://medium.com/@ankitgupta_974 

*   **现在一个 RHEL 文件夹将会打开。**

![](img/e7a0cd2ee81768ab998565f223174f62.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **打开终端，在终端上键入以下所有命令。**

```
**cd /var/**
```

通过这个命令，您可以到达 var 目录。

```
ls
```

命令列出所有目录。

```
mkdir
```

做一个目录。

```
**cd /var/ftp**
```

进入 ftp。

*   创建一个目录名**“pub”。**

![](img/27a7109344e6432d6a410ed20fb0d00d.png)

*   **现在“pub”是一个空目录，所以使用下面的命令复制 pub 目录中的“Packages”文件夹，并按回车键，然后在 pub 中复制“Package”文件夹。**

```
**“cp -R Packages /var/ftp/pub”** 
```

![](img/236c70b2d9833e0102dc876b1b389bfe.png)

图片来源:【https://medium.com/@ankitgupta_974 

*   **现在进入包文件夹，运行下面终端图像中显示的命令。**

![](img/70268db2b40a18a9245b424b9e66e96a.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在运行命令来配置 yum。使用命令如下图所示。**

![](img/e354a609d54aafcfea8eb71e1cb1d927.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **现在从使用命令返回到“pub”返回一步。**

```
**cd ..** 
```

*   **现在运行以下命令。**

```
**“createrepo -v Packages”**
```

![](img/496a53d82b6fbb79d28f520c98cb6618.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   **运行命令到达 yum.repos.d 位置。**

```
**cd /etc/yum.repos.d** 
```

![](img/b9a64b5c111af0e5d1e0c726bf1bd6a1.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

*   现在编辑路径并给出所有信息，如下所示。

![](img/a0ebb4741e2e890fbe5bc61f0c89e12b.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

```
**press Esc ->colon(:)->wq!(for save and exit)**
```

*   **再次键入命令进行检查。**

```
**vi testing.repo
Type Command for cleaning**
```

*   **键入清洗命令。**

```
**yum clean all**
```

*   **键入安装 java 的命令。**

```
**yum install java**
```

![](img/f8f055137d63106f50e86cc7128e1fc8.png)

图片来源:[https://medium.com/@ankitgupta_974](/@ankitgupta_974)

**安装完成。**

**作者领英:**

[https://www.linkedin.com/in/ankit-gupta2/](https://www.linkedin.com/in/ankit-gupta2/)

[](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350) [## 从 JSON 文件中提取有用数据用于机器学习

### 如何从一个 JSON 文件中提取数据用于 Python 中的机器学习模型

medium.com](/analytics-vidhya/extract-the-useful-data-from-jason-file-for-data-sceince-34ed5ae0b350) [](/analytics-vidhya/installation-of-opencv-in-simple-and-easy-way-15556edca7a4) [## 安装 OPENCV 的 5 个简单易行的步骤

### 在这个有趣的教程中，我们将学习在 Ubuntu 系统中设置 OpenCV-Python。以下步骤针对 Ubuntu 16.04 进行了测试…

medium.com](/analytics-vidhya/installation-of-opencv-in-simple-and-easy-way-15556edca7a4) 

**感谢您的阅读，如果您喜欢请点击拍手按钮。**

**关注我们，了解更多简单有趣的内容。**

**更多内容请看**[**AnalyticsVidhya**](https://medium.com/analytics-vidhya)**。**