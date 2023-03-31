# 创建基本 Reddit 机器人的综合指南[第 2 部分]

> 原文：<https://medium.com/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-2-85de2fd300cd?source=collection_archive---------13----------------------->

## Reddit 机器人开发 101

![](img/1bf73332975a0b9394ec94ac1fb8b52b.png)

[亚历山大·奈特](https://unsplash.com/@agk42?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在本系列的第一部分中，我向您展示了如何启动和运行我们的机器人。我们知道机器人如何访问 Reddit，以及它如何看到帖子。然而，我们从来没有谈到机器人的交互部分，这无疑是它最重要的特性。所以在这篇文章中，这就是我们将要讨论的内容。

如果您还没有阅读第 1 部分，我强烈建议您阅读:

 [## 创建基本 Reddit 机器人的综合指南[第 1 部分]

### 创建简单的交互式 Reddit 机器人指南的第 1 部分。

ashishkulkarnii.medium.com](https://ashishkulkarnii.medium.com/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-1-15fb0e4cebcb) 

# 用我们的机器人发表评论

老实说，这是轻而易举的事，因为 PRAW 给了我们所有需要的工具。我们所要做的就是导入 PRAW，调用我们的机器人，将其添加到一个 subreddit(我们选择的)，寻找一个或多个提交(按*热门、热门、新、*或*上升*排序)，并发布回复。我们可以这样做:

我再次使用 r/SpaceJam2021 作为测试的子编辑。不要在你的机器人测试中散布流行的子目录，这会让其他用户感到厌烦。

*submission . reply(" "*)用传递的参数作为评论进行回复，它下面的一行只是给了我们一个我们的 bot 在终端回复的帖子的确认。

# 评论满足条件的帖子

让我们更进一步，为我们的机器人应该回复的帖子添加一些条件。在大多数情况下，让一个机器人评论每一篇帖子是不切实际的。

所以在下面的程序中，我指示我们的机器人回答“我是机器人！”只对标题中包含字符串“你是机器人”的帖子，忽略大小写。通过更改*限制*，您可以更改每次执行时您想要查看的帖子数量。

# 访问和解析数据集以获得所需的统计数据

我在第 1 部分中提到过，我的项目的目标是创建一个 Reddit 机器人，提供特定地理区域的新冠肺炎案例的统计数据。我决定创建一个机器人，而不是像应用程序或网页这样更通用的东西，是因为我想通知那些不知道的人。我想告诉那些甚至没有寻找我要给他们的信息的人。我认为没有其他实现可以有效地完成这项任务。

所以我的机器人的主要部分，不一定与你的机器人相关，是在免费在线 API 的帮助下访问数据集。

我采取的第一步是找到一个免费的 API 服务，它愿意访问可靠的、维护良好的数据集，这些数据集存储了新冠肺炎的统计数据(活跃病例、死亡人数等)。).我发现了一个免费可信的网络抓取解决方案，它访问各种政府/官方网站获取新冠肺炎统计数据，并将这些数据转化为每五分钟更新一次的公共 API。

例如，Apify 访问印度新冠肺炎州的[卫生和家庭福利部](https://mohfw.gov.in/)，访问美国的[疾病控制和预防中心](https://www.cdc.gov/)。

Apify 的数据集是 JSON 文件，所以很容易用 Python 解析。这个是世界数据集的样子，这个[是印度数据集的样子。](https://api.apify.com/v2/key-value-stores/toDWvRj1JpTXiM8FF/records/LATEST?disableRedirect=true)

因为我总是想要最新最好的数据，所以把数据存储在本地是没有意义的。因此，我们将不得不在运行时访问在线数据集。下面是我用来做这件事的代码:

没有必要赘述，但基本上 *world_covid_stats* 现在是一个字典，包含了 Apify 当时在世界新冠肺炎统计上的所有数据。

因为我来自印度，所以我决定提供一些关于印度国内统计的更深入的信息。所以我添加了这段代码来获取状态信息:

我使用 India _ covid _ stats[" region data "]的原因是，JSON 文件的开头是关于整个印度的数据，我想在运行时跳过对它的解析。

# 创造我们的最终机器人

现在我们已经有了所有需要的数据，我们可以开始完成我们的代码了。我希望机器人在没有被明确调用的情况下提供信息，这意味着我必须考虑用户的各种造句。我肯定更高级的编码人员会决定在机器人中实现 NLP 模型，但我决定走一条稍微容易一点的路线。

该机器人在 Reddit 帖子的标题中查找关键词，如*去*、*访*、中的*等。如果它碰巧找到其中任何一个，它会在两个数据集中查找一个印度州的名称或一个国家的名称。如果找到一个，它就检索相应的数据并把它放在注释中。*

例如，如果一个帖子说*我正在俄罗斯访问一段时间*，机器人会回复*俄罗斯有 348160 个当前病例，小心！*

这是我的机器人的代码:

如果您对运行 covibot 感兴趣，请前往 [it's repo](https://github.com/ashishkulkarnii/covibot) ，并阅读 README.md 文件中的说明。

我们终于有了一个可以与其他平台用户智能交互的 Reddit 机器人！你可能认为我们已经完成了，但还没有。这里有一些我们尚未解决的缺陷:

1.  我们的机器人必须手动调用。它不会自动回复我们 subreddit 上的帖子，相反，我们必须在每次需要时运行它。
2.  我们的机器人还不知道哪些帖子已经回复，哪些没有回复。因此，如果你运行它两次，并且在这两次之间没有新的帖子，你一定会看到我们的机器人的多个评论。

这是本系列的第 3 部分，我们将解决最后两个问题:

[](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-3-351817cffae2) [## 创建基本 Reddit 机器人的综合指南[第 3 部分]

### Reddit 机器人开发 101

ashishkulkarnii.medium.com](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-3-351817cffae2)