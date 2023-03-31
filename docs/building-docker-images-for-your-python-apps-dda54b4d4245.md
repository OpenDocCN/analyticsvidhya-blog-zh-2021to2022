# 为 Python 应用程序构建 Docker 图像

> 原文：<https://medium.com/analytics-vidhya/building-docker-images-for-your-python-apps-dda54b4d4245?source=collection_archive---------13----------------------->

最近，我一直在做一个 web scraper，它从用户提供的 URL 下载文件，处理它们，最后上传到 FTP 服务器。当我开始这个项目时，我在探索几种编程语言来构建这个项目，最终我选择了 **Python** 。

每个 python 项目都需要安装一组库，我的项目也不例外。这里的问题是，当您在主环境下安装额外的库或不同的库版本时，您可能会破坏其他项目的依赖关系。因为那个…