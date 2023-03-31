# 创建基本 Reddit 机器人的综合指南[第 3 部分]

> 原文：<https://medium.com/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-3-351817cffae2?source=collection_archive---------12----------------------->

## Reddit 机器人开发 101

[](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-1-15fb0e4cebcb) [## 创建基本 Reddit 机器人的综合指南[第 1 部分]

### 创建简单的交互式 Reddit 机器人指南的第 1 部分。

medium.com](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-1-15fb0e4cebcb) [](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-2-85de2fd300cd) [## 创建基本 Reddit 机器人的综合指南[第 2 部分]

### 在本系列的第一部分中，我向您展示了如何启动和运行我们的机器人。我们知道机器人是如何访问 Reddit 的…

medium.com](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-2-85de2fd300cd) 

在本系列的第 1 部分和第 2 部分中，我们创建了一个工作机器人，它分析文章标题，搜索关键词，并将地区名称与当前的新冠肺炎统计数据进行匹配。然而，我们没有解决最后两个问题:

1.  我们的机器人不会跟踪它回复了哪些帖子。所以如果我们循环运行我们的程序，它会多次回复相同的帖子。
2.  该机器人不会一直保持活动状态。它只有在我们运行的时候才会回复。

![](img/198ab0f2403c316eaee236c6a128ea25.png)

安迪·凯利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 确保每个帖子只有一个回复

确保一个帖子只被回复一次的一个简单方法是让我们的程序创建一个文本文件，并将我们回复的每个帖子的提交 ID 写入其中。通过这种方式，id 被存储起来，并可以在下次执行时被访问。我们可以如下实现这个方法:

现在，在通过帖子寻找关键字之前，我们添加以下条件来检查当前帖子的 ID 是否是列表*ID*的一部分:

+ '\n '是因为我们在写入文件时将每个 ID 添加到下一行。

最后，如果当前的帖子满足所有必要的条件，一旦我们对它执行了操作，我们就将它的 ID 写入文本文件。

# 让我们的机器人一直保持活跃

对此我们有三种主要的方法。

*   我们可以监视已经部署了我们的 bot 的子 reddit 的活动。因此，我们的机器人只会运行时，一个新的职位了。
*   我们可以使用任务调度器定期执行我们的程序。
*   我们可以让我们的程序永远运行，按照我们的要求在程序中使用*睡眠*功能。

我选择了最后一个选项。我在我的程序的主体之前添加了一个真正的条件，并在程序的最后添加了一个睡眠函数，在 while 函数中。

# 处理 covibot 中的问题

下面是 [covibot](https://github.com/ashishkulkarnii/covibot) (来自[第二部分](/analytics-vidhya/a-comprehensive-guide-to-creating-a-basic-reddit-bot-part-2-85de2fd300cd))在解决了上述两个问题后的样子:

上面的 bot 可以部署到服务器上并永久激活，或者如果您有办法，也可以在本地运行它。Heroku 提供了大量免费的 dyno 小时供你使用，尤其是如果这个项目是纯实验性的。

如果你阅读了我的 Reddit 机器人开发指南的所有三个部分，谢谢你！

[](https://www.linkedin.com/in/ashishkulkarnii/) [## Ashish Kulkarni | LinkedIn

PES 大学的 CSE 新生](https://www.linkedin.com/in/ashishkulkarnii/)