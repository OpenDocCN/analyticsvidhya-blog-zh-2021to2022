# 玩一些独特而有趣的 API

> 原文：<https://medium.com/analytics-vidhya/play-with-some-unique-and-fun-apis-f1369da8d3d1?source=collection_archive---------11----------------------->

![](img/ca0baec6814ed7d0f040998c9a881856.png)

# 介绍

术语 API 是一个首字母缩写词，它代表“**应用程序编程接口**”。

> *把 API 想象成餐馆里的菜单。菜单提供了您可以点的菜肴列表，以及每道菜的描述。当你指定你想要的菜单项目时，餐馆的厨房会做这项工作，并为你提供一些成品菜肴。你不知道餐馆是如何准备食物的，你也不需要知道。*

今天就来探索一些很酷很独特很好玩的 API 吧。

# 移除 API

所以，我们要探索的第一个 API 是来自 [**removal.ai**](https://removal.ai/) 。Removal.ai 使我们可以轻松地从图像中移除背景，最终得到的图像更干净、更好。使用它的网站很简单，我们只需拖放我们的图像，它就会提供我们的合成图像。

现在，这个网站也给了我们 API 访问权限。我们只需要一个 **API 键**，我们可以使用下面的 cURL 命令来移除背景，而不用去它的网站。

```
$ curl -L -X POST "https://api.removal.ai/3.0/remove"
-H "Rm-Token: YOUR_API_KEY"
-F "image_file=@path/to/image.jpg"
-F "get_file=1" -o transparent_image.png
```

你可以在这里从[获取 API 密匙，在这里](https://removal.ai/my-account)阅读完整的 API 文档[。](https://removal.ai/api-documentation/)

# 随机笑话 API

[**icanhazdadjoke.com**](https://icanhazdadjoke.com/)是互联网上最大的爸爸笑话精选。现在支持许多不同的集成，以确保您可以在任何地方访问您需要的爸爸笑话。可以加到 Slack 甚至 Alexa。它为我们提供了 API 来获取一个随机的笑话，一个特定的笑话，或者搜索各种格式的笑话。最好的事情是我们不需要为使用 API 而认证我们自己。

我们可以用不同的方法获取数据。获取 JSON 格式笑话的基本 cURL 命令是:

```
$ curl -H "Accept: application/json" [https://icanhazdadjoke.com/j/R7UfaahVfFd](https://icanhazdadjoke.com/j/R7UfaahVfFd)
```

你可以在这里了解更多。

# 漫威 API

我们要讨论的下一个 API 来自 [**漫威**](https://developer.marvel.com/) 。漫威的粉丝现在一定很喜欢这个帖子。它允许各地的开发者访问漫威庞大的漫画图书馆的信息——从即将到来的到 70 年前。这个工具将帮助各地的开发者使用漫威漫画时代 70 多年的数据创建惊人的、不可思议的和难以置信的网站和应用程序。

只需[注册](https://developer.marvel.com/signup)一个账户，在这里获得一个 API [。我们获得了一个巨大的 API 访问范围，从人物到系列和漫画等等。只需阅读 API 文档](https://developer.marvel.com/account)[这里](https://developer.marvel.com/documentation/getting_started)并开始尽可能多地探索这个 API(*当然，漫威粉丝必须做*)。

# 免费餐 API

这个 API 来自[**TheMealDB.com**](https://www.themealdb.com/)，在这里我们可以使用名称、地区、类别和配料来搜索食物。API 和站点将在访问点始终保持免费。您可以在应用开发期间或出于教育目的使用**测试 API 键“1”**。但是，如果公开发布，您必须通过电子邮件申请一个生产 API 密钥。如果你是一个美食家，你必须在这里探索这个 API..

# 趣味翻译 API

[Funtranslations API](https://api.funtranslations.com/) 提供了 funtranslations.com 提供的全套翻译，这样你就可以将它们集成到你的工作流程或应用程序中。瓦特小时

基本端点是:

```
[https://api.funtranslations.com/translate/](https://api.funtranslations.com/translate/)
```

对于公共调用，你不需要传递任何 API 键。只需调用端点(参见示例[这里的](https://funtranslations.com/api/))。然而，对于付费订阅，您需要在标题中以*X-fun translations-API-Secret*的形式传递 API 密钥。

# 美国宇航局 API

人类对外层空间一直很感兴趣。这个 API 的目标是使 NASA 的数据，包括图像，对应用程序开发人员来说是显而易见的。[**api.nasa.gov**](https://api.nasa.gov/)星表越来越大。你不需要验证就可以浏览 NASA 的数据。然而，如果你将大量使用 API 来支持一个移动应用，那么你应该注册一个 [NASA 开发者密钥](https://api.nasa.gov/#signUp)。你可以在这里了解更多关于 API[的信息。](https://api.nasa.gov/#browseAPI)

# 通知单 API

[Advice Slip JSON API](https://api.adviceslip.com/) 来自 AdviceSlip.com[的](https://adviceslip.com/)，在这里我们可以随机获取建议，通过 id 获取建议或者搜索建议。它是完全免费的，如果你想获得建议，它会很有帮助。点击了解更多关于这个[的信息。](https://api.adviceslip.com/)

# 结论

我希望你喜欢上面有趣的 API，并且将来有一天一定会去看看。您也可以查看这个 [github 资源库](https://github.com/public-apis/public-apis)以获得公共 API 的详细列表。
本帖最初发表在我的[博客](https://iread.ga) [这里](https://iread.ga/posts/17/play-with-some-unique-and-fun-apis)。