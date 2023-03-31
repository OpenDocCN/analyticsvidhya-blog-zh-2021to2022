# 导入 CSV 样本(系外行星！)数据导入 PostgreSQL

> 原文：<https://medium.com/analytics-vidhya/import-csv-sample-exoplanets-data-into-postgresql-d57aac30518d?source=collection_archive---------17----------------------->

![](img/ddba8ceca8ce170c9c974ea339383773.png)

[PostgreSQL](https://aiven.io/postgresql) 现在是而且仍然是我们最受欢迎且不断增长的存储平台之一；其他存储技术来来去去，但现代 Postgres 是许多应用程序的可靠选择。当您启动您的第一个 Aiven PostgreSQL 时，您会希望花一些时间来体验这些特性……但是有一个问题。你新的闪亮数据库是空的。

寻找和使用一些开放的数据集是填补这一空白的一个很好的方法，一个选择是去尝试 [Kaggle](https://www.kaggle.com/) 平台。它是一个寻找开放数据、关于数据科学的建议以及一些可以参加以磨练技能的比赛的地方。有相当多的数据集可供选择，但今天我们将使用来自开普勒任务的[系外行星数据](https://www.kaggle.com/nasa/kepler-exoplanet-search-results)。你需要一个(免费)帐户登录并下载数据。继续解压缩 zip 文件，我们将在本文中使用`cumulative.csv`作为例子。

# 开始使用 Aiven

如果你还不是一个 Aiven 用户，你可以[注册一个 Aiven 账户](https://console.aiven.io/signup)来遵循这篇文章中的步骤——我们会等你的！

我们还将使用[虚拟命令行界面](https://github.com/aiven/aiven-client)。该工具需要 Python 3.6 或更高版本，可以从 PyPI 安装:

```
pip install aiven-client
```

您还需要使用 CLI 工具验证您的 Aiven 帐户。在下面的命令中替换您自己的详细信息:

```
avn user login <email@example.com>
```

您已经拥有了在云中创建一个虚拟数据库所需的一切。

# 创建 PostgreSQL 服务

新项目的第一步是创建一个项目来保存服务。它只需要一个名字:

```
avn project create exoplanets
```

Aiven 在创建服务时提供了许多选项，但为了让我们快速进行，我们将使用最新的 postgres 和最小的软件包，称为 hobbyist。不过最有趣的事情之一是能够选择任何你喜欢的云平台，所以花点时间检查列表并复制你最喜欢的`CLOUD_NAME`字段:

```
avn cloud list
```

我选择了`google-europe-west1`作为我的例子，但是你可以用你选择的云来代替它。下面是创建 postgres 数据库要运行的命令:

```
avn service create -t pg -p hobbyist --cloud google-europe-west1 pg-exoplanets
```

节点准备就绪需要几分钟时间，但是 Aiven CLI 有一个方便的“wait”命令，直到服务准备好与我们对话时才返回。当我们像这里一样手动运行命令时，这不太重要，但是当您的 CI 系统自己启动数据平台时，这非常有用！

```
avn service wait
```

当命令返回时，我们的 PostgreSQL 集群就可以使用了。让我们创建一个数据库来保存样本数据；下面的命令创建一个名为“系外行星”的行星:

```
avn service database-create --dbname exoplanets pg-exoplanets
```

现在我们有了自己的 sad 和空数据库，让我们看看示例数据并将其导入。

# 将 CSV 数据添加到 PostgreSQL

PostgreSQL 内置了将 CSV 数据导入现有表的支持，但是我们没有表结构，只有一个 CSV。幸运的是，有一个工具可以解决这个问题— [ddlgenerator](https://github.com/catherinedevlin/ddl-generator) 是另一个 Python 命令行工具。

下面是如何安装`ddlgenerator`工具，然后从我们之前下载的 CSV 文件中生成`CREATE TABLE`语句:

```
pip install ddlgenerator ddlgenerator postgres cumulative.csv > create.sql
```

看一下文件内部，您会发现我们有了向 PostgreSQL 解释如何保存数据所需的结构。`avn service cli`命令将在新数据库上给我们一个`psql`提示:

```
avn service cli pg-exoplanets
```

从`psql`内部，我们可以连接到我们创建的数据库，然后运行 SQL 文件来创建表结构:

```
\c exoplanets \i create.sql
```

添加最后一块拼图，仍然从`psql`提示符开始，下一个命令带来 CSV 数据:

```
\copy cumulative from data/cumulative.csv csv header
```

干得好！`cumulative`表现在应该有一些数据供您使用了！

# 梦见系外行星

现在你有了一个开普勒太空望远镜拍摄的外行星测量数据的数据库。如果你还不熟悉这个项目，NASA 的任务页面[值得一读。当其中一个控制失灵时，任务进入了第二阶段，这提醒我们，我们可以看到和触摸的工程系统，或者至少 ssh 进入，比在太空中操作要容易得多！](https://www.nasa.gov/mission_pages/kepler/overview/index.html)

您可以探索该数据集，它描述了观测结果，并将每个系外行星的开普勒评估与其在现有文献中的官方地位进行了比较。例如，试试看开普勒发现的假阳性:

```
select kepler_name, koi_pdisposition from cumulative where koi_disposition = 'CONFIRMED' and koi_pdisposition = 'FALSE POSITIVE';
```

您还可以将此数据连接到其他工具，以进一步使用数据集。要么从 web 控制台获取连接细节，要么使用带有`avn`的 [jq](https://stedolan.github.io/jq/) 作为一行程序:

```
avn service get pg-exoplanets --json | jq ".service_uri"
```

# 包扎

良好的云实验实践表明，如果你已经完成了系外行星数据库，你可以删除它:

```
avn service terminate pg-exoplanets
```

要获得更多乐趣和知识，以下资源怎么样:

*   [Kaggle 开放数据集](https://www.kaggle.com/datasets)——如果你不喜欢系外行星，这里有一些很好的选择
*   在我们的文档中，您可以找到将您现有的 PostgreSQL 迁移到 Aiven 的[说明](https://help.aiven.io/en/articles/4358591-postgresql-migration-to-aiven)
*   更多关于 [Aiven CLI，](https://github.com/aiven/aiven-client#aiven-client-)
*   [针对 Postgres13 的 Aiven 现已推出](https://aiven.io/blog/aiven-for-postgresql-13-is-now-available)

还没有使用 Aiven 服务？在 https://console.aiven.io/signup[获得免费试用](https://console.aiven.io/signup)

*最初发布于*[*https://aiven . io*](https://aiven.io/blog/discover-exoplanets-with-postgresql)*。*