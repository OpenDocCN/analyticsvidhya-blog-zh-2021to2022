# 使用 Microsoft Graph API 创建个人资料页面

> 原文：<https://medium.com/analytics-vidhya/create-profile-page-using-microsoft-graph-api-280f57149f09?source=collection_archive---------28----------------------->

> Microsoft Graph 是一个 RESTful web API，使您能够访问 Microsoft 云服务资源。注册应用程序并获得用户或服务的身份验证令牌后，您可以向 Microsoft Graph API 发出请求。

**先决条件**:

【azure 主动订阅

熟悉将微软认证库或`msal`集成到应用程序中(可选)

[向微软身份平台注册的应用程序](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)

从[这里](https://github.com/VMoose/Msal_Active-Directory)下载或克隆启动代码。

**第一步:**开始，打开***config . p****y*文件，在那里输入应用客户端 ID、客户端密码和 redirect_path。 [**这个**](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app) 应该可以帮你入门。

![](img/43e6bc845cfa85e9c15ce88b74eea842.png)

**步骤 2:** 在本地运行应用程序。如果一切顺利，您应该能够看到:

![](img/b1c4dccd7f46b0a27bc0c7ba2fca183f.png)

在尝试登录 Microsoft 时，您应该看到:

![](img/f518982116ee36b99723fad51bb31bb8.png)

**步骤 3** :来自 Azure active directory 的认证令牌保存在缓存中，可以通过在 **views.py** 中添加以下函数从缓存中提取

![](img/301757102748faf70a28c8dfed417c4a.png)

**第四步:**在 **config.py** 中添加变量，从 Microsoft Graph API 中获取用户信息。

![](img/0083d7d4f645cac76f5c9c1d9b24a7b9.png)

**第五步**:将此函数粘贴到 **views.py** 中，从 API 中获取数据

![](img/754ffaff9748e98f2e0715d681cd0ae1.png)

**步骤 6** :使用下面的函数将图形 API 中的图像保存到本地

![](img/e6f2c95f584da5c786566758ce700921.png)

**第 7 步**:将数据传递给**索引模板**，登录后显示。

![](img/ccf81bab7fd4200df76fb1ef3c82dd34.png)

**第八步**:显示**指标模板**中的数据

![](img/407ea596bf7b26c2356851407695887c.png)

最终的输出应该是这样的

![](img/c0be9877e29ace1189bdf56a3ff1bdc5.png)

**有用链接:**

1.  [https://developer.microsoft.com/en-us/graph/graph-explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)—了解更多关于图形 API 的信息
2.  从[这里](https://github.com/VMoose/Msal_Active-Directory)下载或克隆完整的代码。