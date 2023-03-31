# 用 Docker 和 Factory Boy 创建一个超级容易测试的 Django 应用程序。

> 原文：<https://medium.com/analytics-vidhya/creating-a-super-easily-testable-django-app-with-docker-2f3477101186?source=collection_archive---------17----------------------->

![](img/fe67f06807fa1fba6cbe194a47ced3d1.png)

我见过很多建立 Django 项目的不同方法，但我相信 Django 的开发环境应该总是用 Docker 容器化。原因是，我知道不得不手动设置测试数据库、销毁它并在出错时重新创建它是多么令人沮丧，而 docker-contained 将为您节省大量时间和精力。作为奖励，它只需要一个 YAML 文件就可以部署在 Kubernetes 上了！(尽管这可能取决于您计划使用的云服务……)

# Docker 配置

首先，这里是 docker 文件。我知道使用 alpine 图像有一些缺点，但我相信您可以很容易地将其替换为更小的图像，如 slim，所需的更改将是最小的。

您可以使用下面的 YAML 文件来设置开发环境。

您可能想知道为什么在端口部分列出了两个不同的端口号。原因是我使用 VSCode 作为我的主要代码编辑器，为了调试带有断点的项目，您需要使用一个名为 ptvs 的 pip 包并公开一个端口来使它工作。下面是一个如何公开调试端口的示例。

创建一个 make 文件也会让你的生活轻松很多。

# 写作测试

为 Django 应用程序编写测试有许多不同的方法。您可以使用 fixtures 或者使用模拟包来模拟所有东西。我个人喜欢用下面的包。

[](https://github.com/pytest-dev/pytest-factoryboy) [## pytest-dev/pytest-factoryboy

### pytest-factoryboy 使测试设置的工厂方法与依赖注入相结合变得容易，这是……

github.com](https://github.com/pytest-dev/pytest-factoryboy) [](https://github.com/kiwicom/pytest-recording) [## kiwi com/py test-记录

### 一个 pytest 插件，它通过 VCR.py 记录测试中的网络交互。

github.com](https://github.com/kiwicom/pytest-recording) 

除非万不得已，否则您可能不应该浪费时间嘲笑 API 响应。很容易出现输入错误，如果您使用的 API 引入了规范修订，您可能需要手动进行大量调整。您可以用上面的记录包记录 API 响应，并让您的应用程序处理潜在的规范修订。在我创建并在本文底部链接的 Django 应用程序示例中，我没有使用 recording 包，因为我没有在其中使用任何外部 API。然而，我确实使用了 Factory Boy 来填充测试数据库，我将附上一些代码片段来帮助您理解它的用法。(我发誓它比固定装置更容易使用……)

Django 和 Ruby on Rails 是如此高级的框架，默认提供了许多现代特性，我真的不明白使用 NodeJS 和 TypeORM 这样的技术有什么好处。Golang 有 goroutines 和它独特的简单语法，但是 NodeJS 真正提供了什么？(说真的，NodeJS 为什么这么受欢迎？)

[](https://github.com/dakaii/fuzzy-parakeet/tree/medium) [## 达卡伊/绒毛鹦鹉

### 在 GitHub 上创建一个帐户，为 dakai/fuzzy-parakeet 的发展做出贡献。

github.com](https://github.com/dakaii/fuzzy-parakeet/tree/medium)