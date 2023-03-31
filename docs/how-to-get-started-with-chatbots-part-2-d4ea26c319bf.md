# 如何开始使用聊天机器人—第 2 部分

> 原文：<https://medium.com/analytics-vidhya/how-to-get-started-with-chatbots-part-2-d4ea26c319bf?source=collection_archive---------10----------------------->

从对话流到生产环境

![](img/89919b7a353cece51b6aaac3d8a359f0.png)

在 2021 年，让你的“头在云端”是可以的——照片由 Unsplash 上的 [CHUTTERSNAP](https://unsplash.com/@chuttersnap) 拍摄

所以你已经在 Dialogflow 上建立了你的第一个聊天机器人。请给自己一个鼓励(真的，有很多活动部件和行业工具)。

这就够了，我们要利用这个动量。Google 云平台已经做了很多繁重的工作，比如提供可访问的数据库、NLP，以及用自己的脚本创建云服务的能力(应用引擎即将讨论)。

一定要通读[这篇教程](https://cloud.google.com/architecture/building-chatbot-agent-dialogflow)，用我的 [part 1 文章](/analytics-vidhya/how-to-get-started-with-chatbots-part-1-29d619759b33)来阐明幕后发生的事情。

好了，现在你已经完成了第 1 部分，我们今天的讨论将遵循第二个文档([https://cloud . Google . com/architecture/securing-scaling-chatbots](https://cloud.google.com/architecture/securing-scaling-chatbots))。

# 你从这篇文章中得到了什么:

我不想对你撒谎。理解这一点需要一段时间，但我坚信这对于任何值得学习的东西都是正确的(这个主题当然值得努力)。

对读者来说，我的目标是:在这个过程中包含更多的效率、清晰和乐趣。

# 1.webhook:从临时本地脚本到永久服务的旅程

*   **动机:**从第 1 部分开始，webhook 将 Dialogflow 连接到 Datastore，允许访问通过我们的数据集定义的实体，并将其托管在端口 5000 的本地环境中。
*   **HTTP 隧道:**在本地脚本托管且可用的情况下，Dialogflow 仍然无法到达数据存储中的实体。这就是 NGROK 派上用场的地方，它通过万维网上的服务器托管 webhook，使它可以访问 Dialogflow。
*   **HTTP 基本认证:**通过文档中提供的一段代码(请尝试阅读这段*代码*)，我们可以向 Dialogflow 和这个脚本提供一个特定的用户名和密码，以进一步保护我们非常公开的代码。
*   **Python Decorator:** 快速注意，上面的认证是用返回函数的函数实现的(NIFTY！).如果你对计算理论感兴趣，那就看看维基百科对高阶函数的解释。
*   **App Engine:**Google Cloud Platform 提供这个 API，用你的 python 代码创建服务。GCP 将代码从 Datalab 转换成裸 python，放在一个目录中，运行`gcloud app deploy`，更重要的是，运行`gcloud app browse`，它提供了一个公共 URL 来访问这个服务。

# 2.Iframes 很好，但是自定义 ui 更好

*   **有用的云 shell 命令:**如文档所述，当运行云 shell 时，您需要访问您的第二个项目。这里有一个列出你的项目的方法，以防你忘记名字:`gcloud projects list`。下面是如何切换你的项目:`gcloud config set project <project-name>`。此外，一旦进入项目，您可能会发现没有 Datalab 很难导航，因此您可以使用`cat <file-name>`将文件输出到终端命令。或者，如果您更喜欢命令行，那么编写`vim <file-name>`来直接读取和编辑文件。
*   **API 键:**警告，谷歌提供的文档尚未针对 [V2](https://cloud.google.com/dialogflow/es/docs/quick/setup#auth) 进行更新。过去，您会将基于 Angular 的 UI 与 Dialogflow 提供的客户端访问令牌集成在一起。由于这被发现是不安全的，V2 现在使用 JSON，它利用服务帐户密钥。参见[这篇伟大的媒体文章](/google-cloud/how-to-create-a-chatbot-using-dialogflow-enterprise-edition-and-dialogflow-api-v2-923f4a965176)了解如何更安全地连接你的聊天机器人 UI 和 API V2。
*   **NPM:** 这个工具和其他类似的工具(例如 Yarn)提供了对现有的许多 Javascript 库的访问。使用 NPM 和一个完善的 package.json，您可以安装许多库，并轻松地将 package.json 发送给其他开发人员，在他们的机器上创建相同的环境。在这里，它是用来整合角。
*   **Angular:** Angular 是一个基于 Javascript 构建的前端框架，支持前端代码的非静态或动态实现。由于 UI 需要频繁调用 Dialogflow，这对于更现代的界面来说是一个很好的选择。我推荐你看一看这篇[中型文章](/voice-tech-podcast/building-engaging-conversational-uis-with-rich-responses-using-dialogflow-fd182b2547e3)，作为你实施的指南。

# 3.身份感知代理:谷歌云平台拥有一切

*   **定义及关键术语:**来自 [IAP 文档](https://cloud.google.com/iap/docs):“IAP 为 HTTPS 访问的应用建立了一个**中央授权层**，因此您可以采用应用级的访问控制模型，而不是使用网络级的**防火墙**。”换句话说，网络并不总是可信的，但是经过验证的用户和设备是可信的。
*   **代理:**代理是一种介于发出请求的客户端服务器和提供资源的服务器之间的服务器。代理服务器向客户机服务器发出请求，在某种程度上屏蔽了客户机，但也允许对请求进行更多的控制。关于代理服务器如何工作以及不同种类的代理的更多信息，请参见 Wiki 。
*   **防火墙:**这个系统监控网络中的传入和传出请求。由于这是一项应始终包括在内的安全预防措施，IAP 通过基于用户的身份验证改进了这一障碍。使用这种类型的身份验证，攻击者不能通过已经信任的网络发出请求，因此是零信任模型。
*   **中央认证服务:**由于我不想过多地重复这是一种基于用户的认证，我想补充一点，这是一种 web 协议，也用于单点登录之类的事情。

# 我希望这能激励你进一步挖掘

聊天机器人开发不只是一件事！在早期阶段，你必须是一个多面手才能参与进来。我发现几乎所有新兴技术都是如此，这没关系。软件工程不仅仅是寻找解决方案，而且要让它们随时可用。然后，你只需要迭代和改进，所以试着享受这样做的乐趣。

在第 3 部分，我想更多地谈谈机器学习技术。感谢您的阅读，感谢您与我一起踏上这段旅程。