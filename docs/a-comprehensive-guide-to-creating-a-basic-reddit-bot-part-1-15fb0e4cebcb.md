# 创建基本 Reddit 机器人的综合指南[第 1 部分]

> 原文：<https://medium.com/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-1-15fb0e4cebcb?source=collection_archive---------5----------------------->

## Reddit 机器人开发 101

![](img/4d1b340776912936c5d5bb9254d3a118.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我最近参加的一个编码比赛中，我被要求创造一个有益于社会的产品或概念，同时保持创新性和易用性。经过一番考虑，我最终有了创建一个 Reddit 机器人的想法，它可以告诉人们原始海报打算前往的特定地区的新冠肺炎统计数据。问题是，我不希望机器人被有意识地调用，而是希望机器人自动检测用户的目的地，检索维护良好的数据，并以不引人注目的方式将其作为评论发布。

当我在研究如何根据我的需求创建一个机器人时，我没有从单一来源找到我需要的信息。所以在这篇文章中，我希望通过解释我在创建我的机器人时采取的一步一步的过程来填补这个空白，希望这也能帮助你！

# 你需要的软件

![](img/a83535eaacd687c4e3ce671944a35668.png)

照片由[蒂莫西·戴克斯](https://unsplash.com/@timothycdykes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在这篇文章中，我将看看如何使用 Python 3.6 或更高版本的[制作一个机器人，所以确保你的电脑上已经安装了。](https://www.python.org/)

下一件你需要的东西是 PRAW——Python Reddit API 包装器。PRAW 是一个 Python 包，允许你以最简单的方式访问 Reddit 的 API。它被设计成遵循 Reddit 的所有 API 准则，所以违反规则不是问题。安装 PRAW 也极其容易；只需在命令提示符终端输入 *pip install praw* 。pip 是 Python 包安装程序。如果您收到一个错误消息，说您没有安装 pip，那么在这里学习如何安装它[。](https://pip.pypa.io/en/stable/installing/)

# 我们的机器人如何看到 Reddit 页面

现在我们已经准备好了所有需要的软件，让我们快速地看一下 Reddit API 是如何工作的。老实说，这很简单。本质上，Reddit 在。json 格式，可以像解析字典一样用 Python 来解析。

我创建了 subreddit r/SpaceJam2021 来测试我的机器人；这是你对它的看法，这是我们的 T2 机器人对它的看法。

# 在 Reddit 上注册我们的机器人

创建机器人的下一步是在 Reddit 上注册它。我们通过创建一个 Reddit 应用程序来做到这一点。登录你想用来操作机器人的账户(如果你想让机器人与你的个人账户分开，就创建一个新账户)，然后进入[这一页](https://www.reddit.com/prefs/apps/)。点击页面底部的*创建应用*。这里会提示您填写一个*名称*和一个*重定向 uri。*选择项目符号*脚本*，并将其他文本框留空(它们是可选的)。*重定向 uri* 与我们的 Reddit 机器人无关，所以使用类似[example.com](http://example.com)的虚拟 URL。

它带你去的页面给了我们用户 ID(在文本*个人使用脚本*下)和密钥。这两条信息以后会有用的。

# 用我们的机器人的细节更新 praw.ini 文件

这部分可能有点复杂，尤其是你的电脑比较杂乱的情况下。你必须找到你的 *Python* 文件夹，导航到 *Lib* 文件夹，然后是 *site-packages* 文件夹，找到 *praw* 文件夹。

在 *praw* 文件夹中，你会看到一个名为 *praw.ini.* 的文件，我们必须将机器人的详细信息添加到这个文件中，但我建议避免更改原始的 *praw.ini* 文件，以防我们搞砸了什么。所以创建一个 *praw.ini* 的副本，然后打开原来的。

用记事本打开 *praw.ini* ，在底部添加以下几行:

机器人 1 只是为了将我们的机器人与你们现有的/未来的机器人区分开来。在各自的“=”符号后添加客户端 ID 和客户端密码(在单引号或双引号中)。*用户名*和*密码*对应的是你的 bot 要操作的账户的用户名和密码，而 *user_agent* 只是你可以给你的项目取的一个随机的名字。你在这里填写的所有细节都是敏感的，这就是为什么我们把它添加到 *praw.ini* 而不是代码本身。

顺便说一下，如果你找不到 praw 文件夹，并且由于文件资源管理器的搜索很乏味，我推荐使用 Windows 的 [Wox](http://www.wox.one/) 。

# 测试我们的机器人阅读帖子的能力

PRAW 套装有很多属性，你可以在这里找到。但是为了测试机器人的阅读能力，我们需要下面几行代码:

让我们一条一条地过一遍:

*   *import praw* 将 praw 包导入到我们的代码中。
*   T4。Reddit('bot1') 本质上是让 *bot1* 访问 Reddit。
*   我们使用*r . subreddit(" space jam 2021 ")*来表示我们的机器人将在哪个 sub Reddit 中运行。您可以用自己选择的子编辑替换 *SpaceJam2021* 。
*   limit = 5 意味着我们只能看到页面的前五篇文章。我们可以使用*热、顶、新、*或*上升*对帖子进行排序。

您可以尝试一下，并在评论中留下任何发现或建议。

本文到此为止。在下一篇文章中，我将解释机器人的交互部分是如何工作的。让我知道这是否有帮助！

 [## 创建基本 Reddit 机器人的综合指南[第 2 部分]

### 在本系列的第一部分中，我向您展示了如何启动和运行我们的机器人。我们知道机器人是如何访问 Reddit 的…

ashishkulkarnii.medium.com](https://ashishkulkarnii.medium.com/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-2-85de2fd300cd)