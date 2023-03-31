# 如何在 Heroku 上免费使用定制时钟进程调度 Python Scrapy Spiders

> 原文：<https://medium.com/analytics-vidhya/how-to-schedule-python-scrapy-spiders-on-heroku-using-custom-clock-process-for-free-7f3eb9310ba4?source=collection_archive---------3----------------------->

![](img/2b7b5defe8e3aa50ee2d85ca2fb8f858.png)

照片由[拉蒙·萨利内罗](https://unsplash.com/@donramxn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你有没有试过在亚马逊印度大减价期间，每隔几个小时就查看你渴望购买的 iPhone 的价格下降情况？

你的愿望清单中是否有高价产品，并希望得到降价通知？

当股票/密码跌破特定价格时，你希望得到通知吗？

然后，您可能想要构建、部署并定期安排一个刮刀，免费从目标网站上刮取数据。

在开发的早期阶段，我们很容易在本地机器上运行和调度 Scrapy spiders，但最终我们希望在云中部署和运行我们的 spider。

在这篇文章中，我将向你解释如何部署你的 Scrapy spiders，并使用 Heroku 上的一个定制时钟进程定期调度它们。

# **先决条件**

本教程希望你准备好你的 Scrapy 项目，以便能够将你的蜘蛛部署到 Heroku。

# 让我们开始吧

我们需要安装部署和运行 Scrapy spiders 所需的几个模块:

*   运行`pip install scrapyd`安装 scrapyd 守护进程。
*   运行`pip install git+https://github.com/scrapy/scrapyd-client.git`安装 scrapyd-client。
*   运行`pip install herokuify_scrapyd`来安装 herokuify_scrapyd python 模块，该模块简化了向 Heroku 部署 Scrapy 蜘蛛。

你需要通过 pip 在 Heroku 上指定你的 Scrapy 项目的 Python 包依赖关系，通过运行`pip freeze > requirements.txt`在你的项目根目录下创建一个 requirements.txt 文件。

# **Heroku 入门**

*   在 [Heroku](https://www.heroku.com/) 上创建账户。
*   安装 [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) ，让你直接从终端创建和管理 Heroku 应用。
*   `cd`到你的项目文件夹，运行`heroku login -i`登录你的 Heroku 账户。
*   运行`heroku create`创建一个 Heroku app。

```
$heroku createCreating app… done, ⬢ <HEROKU_APP_NAME>
Created http://<HEROKU_APP_NAME>.herokuapp.com/ | git@heroku.com:<HEROKU_APP_NAME>.git
```

*   运行`heroku git:remote -a <HEROKU_APP_NAME>`向您的 Heroku 应用添加遥控器。

创建 Heroku 应用程序后，您需要编辑 scrapy.cfg 文件，如下所示:

用于 Heroku 上部署的 scrapy 项目的 scrapy.cfg 文件

# **自定义时钟流程**

Heroku Scheduler 是一个免费的附加软件，可以在指定的时间每 10 分钟、每小时或每天安排简单的任务。但是，如果您希望每 5 秒钟运行一次蜘蛛，或者每天运行三次蜘蛛，或者在特定的时间运行蜘蛛，该怎么办呢？在这些独特而复杂的场景中，使用自定义时钟进程调度蜘蛛可以提供更好的控制，建议在生产中使用。

请记住，scheduler add-on 并不保证作业在预定时间执行，在极少数情况下，作业可能会被跳过或运行两次。

现在，让我们创建一个定制的时钟进程来定期调度 Heroku 上的 Scrapy spider。我们将使用来自 APScheduler Python 库的 TwistedScheduler，因为 Scrapy 是建立在 Twisted networking 框架之上的。APScheduler 是一个轻量级的进程内任务调度程序，它提供了一个干净、易用的调度 API。

让我们通过运行`pip install pytz`和`pip install apscheduler`开始安装调度所需的模块。重新运行`pip freeze > requirements.txt`来更新你的 requirements.txt 文件。

现在，在项目根目录下创建一个 scheduler.py 文件，如下所示。

一个日程安排程序触发了我们在 Heroku 的战斗蜘蛛

`send_request()`函数向`schedule.json` Scrapyd JSON API 发出 POST 请求。您的 Scrapy 项目名称和蜘蛛名称作为 post 请求的一部分发送。

有关 APScheduler cron trigger 提供的选项的更多详细信息，请参考本文档。

在项目根目录中创建一个 Procfile，显式声明启动应用程序时要执行的命令。您的 Scrapy 项目的 Procfile 将如下所示:

Procfile 用于 Scrapy 项目

在项目根目录中创建 runtime.txt 文件，指定 Python 运行时，如下所示:

`python-3.7.10`

# **向 Heroku 部署蜘蛛**

*   运行`git init`、`git add .`、`git commit -m <COMMIT_MESSAGE>`来初始化一个本地 Git 存储库，并将您的应用程序代码提交给它。
*   运行`git push heroku master`将您的应用部署到 Heroku。

在将您的 spider 部署到 Heroku 之后，运行`heroku ps:scale clock=1`将时钟进程扩展到单个 dyno，从而避免调度重复的作业。

瞧啊。你现在已经成功地将你的战斗蜘蛛部署到 Heroku。您可以通过运行`heroku logs --tail`来检查日志，您将能够找到通过 scheduler.py 文件发送的 POST 请求。

现在，如果您转到`http://<HEROKU_APP_NAME>.herokuapp.com`，您应该会看到 Scrapyd 欢迎页面，在这里您可以找到您的未决、正在运行和已完成的作业，并且您可以检查日志。

要完全停止你的调度程序，运行`heroku ps:scale clock=0`将你的时钟进程缩减到 0。

> **注意:** Heroku 免费和爱好计划适用于非商业应用，你可能需要升级到其他计划来安排你的蜘蛛生产。

恭喜你！你已经完成了教程的最后部分，我希望这对你有所帮助。

> **源代码:**可以参考我的 [**亚马逊价格追踪器**](https://github.com/yashashreesuresh/amazon_price_tracker) Scrapy spider 在 Heroku 上使用自定义时钟进程定时调度。它跟踪你想要购买的亚马逊产品的可用性和价格，并在价格下降时通过电子邮件通知你！