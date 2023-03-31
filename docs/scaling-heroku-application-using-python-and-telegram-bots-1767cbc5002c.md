# 使用 Python 和 Telegram 机器人扩展 Heroku 应用程序

> 原文：<https://medium.com/analytics-vidhya/scaling-heroku-application-using-python-and-telegram-bots-1767cbc5002c?source=collection_archive---------5----------------------->

![](img/88bc8117ab79366e1c64636f6987f54c.png)

Heroku 是一个基于容器的云平台即服务(PaaS)。开发人员使用 Heroku 来部署、管理和扩展现代应用。他们的平台优雅、灵活且易于使用，为开发人员将应用推向市场提供了最简单的途径。Heroku 提供免费和付费两种等级。它始于 2007 年，现在只支持 Ruby 语言；它支持许多语言，如 Ruby，Python，Java，Scala，PHP 等。根据我的个人经验，当我用它来部署基于 Python 的 Flask Web 应用程序时，Heroku 的学习曲线很陡。这也需要一些 Git 知识。但是，当您部署了第一个应用程序后，一切都变得轻松了。

## **Dynos**

> 在 Heroku 使用的容器被称为“dynos”

Heroku 是一个可伸缩的平台，dynos 提供可伸缩性。Dynos 是隔离的虚拟化 Linux 容器，旨在根据用户指定的命令执行代码。Dynos 是任何 Heroku 应用程序的基础，从简单到复杂。

简而言之，dynos 运行应用程序；如果运行的 dynos 数量为 0，则应用程序处于非活动状态。否则，如果 dynos 的数量大于 0，则应用程序是活动的。

在本指南结束时，我打算为我们如何使用 Python 来控制 Heroku 应用程序(使用 Heroku API)和在 Telegram 上转发更新打下基础。使用这些基本技能，人们可以构建一个定制的电报机器人，它可以转发更新并接受用户的命令。

**生成 Heroku API**

导航到“**账户设置**，点击“**显示**找到你的 API 密匙。把它拷贝到安全的地方。

![](img/28748238c01660b4417fac17f410ffcc.png)![](img/769f8816f7c79efd0974ffadb8f169f1.png)

# 使用电报机器人通知

使用电报机器人“**机器人令牌**”和“**聊天 id** ”发送消息是必要的。按照指南为您的帐户生成一个。

[](/@ManHay_Hong/how-to-create-a-telegram-bot-and-send-messages-with-python-4cf314d9fa3e) [## 如何创建电报机器人并用 Python 发送消息

### Telegram 拥有令人惊叹的功能，机器人功能就是其中之一

medium.com](/@ManHay_Hong/how-to-create-a-telegram-bot-and-send-messages-with-python-4cf314d9fa3e) 

# **Python 代码**

## Python 类的初始化

首先，我们导入所有必要的库，比如“ **requests** ”来进行 API 调用，“ **base64** ”来编码 Heroku API，等等。

然后我们用变量初始化一个 python 类，比如 **Heroku app name** ， **Heroku API key** ， **Telegram bot Token** 和 **Telegram chat id** 。

函数" **generate_basekey** "将对 API 进行编码并创建一个头。该标题将用于在接收端验证我们的请求。

函数“ **Telegram_Notify** ”是一个帮助函数。其他功能将使用此功能通过电报机器人发送更新。

两个函数“**get _ current _ dyno _ quantity**”和“**get _ current _ dyno _ quantity _ notify**”在本质上是相似的，只有一点不同，即它们都返回当前的 dyno 数，但是“**get _ current _ dyno _ quantity _ notify**会向 Telegram chat 发送消息，而另一个则不会。

“ **scale** ”功能执行实际的缩放工作，即增加和减少应用的数字数量。

如果请求成功，它将使用电报消息通知。

“**重启**”功能是一个方便的功能。当对应用程序进行一些更新或我们面临问题时，这是最有用的。

这就把我们带到了指南的结尾，希望你喜欢。如果你是 heroku 的正式用户，你也可以看看 Heroku 的 [***库。***](https://github.com/martyzz1/heroku3.py)

请在我的[***Github***](https://github.com/rvt123/Medium_Articles/tree/main/Heroku_With_Python)找到本指南的完整代码。在[***LinkedIn***](https://www.linkedin.com/in/raghuvansh-tahlan/)上联系我。

[](https://www.linkedin.com/in/raghuvansh-tahlan/) [## raghuvansh tah LAN-Guru Gobind Singh Indraprastha 大学-新德里，德里，印度| LinkedIn

### 我已经从 Guru Gobind Singh Indraprastha 大学获得了 B. Tech(计算机科学工程)学位，我的目标是成为一名数据科学家。

www.linkedin.com](https://www.linkedin.com/in/raghuvansh-tahlan/) [](https://github.com/rvt123/Medium_Articles/tree/main/Heroku_With_Python) [## Medium _ Articles/Heroku _ With _ Python at main rvt 123/Medium _ Articles

### 向 rvt123/Medium_Articles 投稿并启动存储库。

github.com](https://github.com/rvt123/Medium_Articles/tree/main/Heroku_With_Python)