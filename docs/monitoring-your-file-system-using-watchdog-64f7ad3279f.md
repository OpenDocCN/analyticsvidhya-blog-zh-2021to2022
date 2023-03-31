# 使用看门狗监控您的文件系统

> 原文：<https://medium.com/analytics-vidhya/monitoring-your-file-system-using-watchdog-64f7ad3279f?source=collection_archive---------0----------------------->

## 了解如何监视本地或远程机器上文件系统的任何更改，并使用 watchdog 采取一些措施。

您将使用 watchdog 连续监视文件夹，检查是否有新文件，或者现有文件何时被修改或删除。当文件夹的大小超过一定的限制，然后采取一些行动。

![](img/1b4809d285666d26d3649f0222f53723.png)

照片由 [Iwona Castiello d'Antonio](https://unsplash.com/@aquadrata?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/fies-and-folder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄