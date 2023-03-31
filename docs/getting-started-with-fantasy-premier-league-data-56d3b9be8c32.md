# 使用 Python 访问梦幻超级联赛数据

> 原文：<https://medium.com/analytics-vidhya/getting-started-with-fantasy-premier-league-data-56d3b9be8c32?source=collection_archive---------0----------------------->

## 如何从 FPL API 访问数据

![](img/21f5bd4bd744b9885e9ebd557baa1b09.png)

# 关于作者

我第一次玩[梦幻英超](https://fantasy.premierleague.com/) (FPL)是在 2007 年。我当时在上高中，并没有真正关注英超。然而，我的大多数朋友都这样做了，我厌倦了不能加入他们在教室后面的团队选择讨论中偷懒。我记得我选择了我的第一支球队，它的中场由法布雷加斯、杰拉德、兰帕德和克里斯蒂亚诺·罗纳尔多组成，然后是一群来自朴茨茅斯和维根等垫底俱乐部的后卫，因为他们是我剩余预算所能负担的最好的球队。

随着时间的推移，FPL 提高了我对英超联赛的兴趣，而我从观看现场比赛中积累的知识也让我的 FPL 队受益匪浅。在接下来的几个赛季中，我发现自己一直在迷你联赛中名列前茅。在 2015/16 赛季，我距离前 10 公里只有一步之遥，但从全球总排名的角度来看，我很大程度上可以说我已经*好了*。

在大学期间，我学会了使用 Python 编程，现在是一名数据科学家。我最近一直在想，我是否可以利用我的一些技能，以一种更加数据驱动的方式做出我的 FPL 决策。我偶然发现了一个 [Reddit 帖子](https://www.reddit.com/r/FantasyPL/comments/f8t3bw/cheatsheet_of_all_current_fpl_endpoints/)，其中提到了一个 FPL 的 API(应用程序编程接口),这篇文章将提供一个初学者如何使用 Python 访问这个 API 的指南。

# 梦幻英超 API

好消息是这个 API 是完全免费使用的(前提是你不用它来做任何赚钱的事情)。这个 API 由许多端点组成，这些端点一起可以为用户提供 FPL 游戏的全貌。端点基本上只是一个 URL，在您向它发送 HTTP 请求后，它会用一些数据进行响应。

这些终点的总结如下:

# 引导-url

让我们从列表中的第一个端点开始；试着将它的端点([https://fantasy.premierleague.com/api/bootstrap-static/](https://fantasy.premierleague.com/api/bootstrap-static/))粘贴到你的浏览器地址栏中——你应该会得到一个看起来像这样的相当混乱的响应:

```
{"events":[{"id":1,"name":"Gameweek 1","deadline_time":"2020-09-12T10:00:00Z","average_entry_score":50,"finished":true,"data_checked":true,"highest_scoring_entry":4761681,"deadline_time_epoch":1599904800,"deadline_time_game_offset":0,"highest_score":142,"is_previous":false,"is_current":false,"is_next":false,"chip_plays":[{"chip_name":"bboost","num_played":112843},{"chip_name":"3xc","num_played":225426}],"most_selected":259,"most_transferred_in":12,"top_element":254,"top_element_info":{"id":254,"points":20},"transfers_made":0,"most_captained":4,"most_vice_captained":4},{"id":2,"name":"Gameweek 2","deadline_time":"2020-09-19T10:00:00Z","average_entry_score":59,"finished":true,"data_checked":true,"highest_scoring_entry":6234344,"deadline_time_epoch":1600509600,"deadline_time_game_offset":0,"highest_score":165,"is_previous":false,"is_current":false,"is_next":false,"chip_plays":[{"chip_name":"bboost","num_played":94615},{"chip_name":"freehit","num_played":111968},{"chip_name":"wildcard","num_played":494000},{"chip_name":"3xc","num_played":221133}],"most_selected":259,"most_transferred_in":302,"top_element":390,"top_element_info":{"id":390,"points":24},"transfers_made":14637421,"most_captained":4,"most_vice_captained":254} ...
```

## JSON 数据格式

从 API 返回的响应是一种称为 JSON 的格式。它的应用非常广泛，尤其是在 API 领域，所以知道如何处理这种格式的数据是一项非常有用的技能。

幸运的是，我们有一些 Python 库可以用来更容易地筛选这些数据，即[请求](https://docs.python-requests.org/en/master/)、 [json](https://docs.python.org/3/library/json.html) 和 [pandas](https://pandas.pydata.org/) 。

## 使用请求库

为了创建一个新的请求，我们使用了`requests.get()`函数。并对响应使用`.json()`方法来正确解析它。

如果您试图打印响应的值，那么您的笔记本编辑器在试图呈现完整的单元格输出时会变得非常慢。在这个阶段，我们只对理解模式感兴趣，不需要看到每一条数据都打印到我们的屏幕上。`pprint`模块为这个问题提供了一个简洁的解决方案，我们将使用它来显示 API 响应中的顶级列。

响应是 JSON 格式的，非常类似于嵌套的 Python 字典

现在我们可以看到这个端点包含一些嵌套字段。让我们仔细看看其中的一些字段。

# 玩家数据

**元素**字段包含 FPL 当前赛季每个英超球员的数据。我们可以访问这些数据，就像访问与字典中的一个键相关联的数据一样。响应是更多字典的列表——每个玩家一个。

让我们获取元素数据，然后显示列表中第一个玩家的信息:

每个嵌套字典都包含关于特定玩家的信息

## 更容易检查熊猫

在现阶段，如果这些数据是表格形式的，对我们会更有用。熊猫图书馆就是为此而建的。可以使用`json_normalize()`函数将 JSON 数据加载到数据帧中:

熊猫数据框对于在一个视图中显示多个玩家很有用

现在我们有了一个包含每个球员信息的表，但是我们不知道他们为哪个队效力，或者他们踢什么位置，因为我们只有这些列的数字 ID 值。

## 支持数据

我们可以通过将基本响应中的**团队**字段提取到一个数据帧中来获得团队的名称和实力评级:

每支球队都有主客场进攻和防守的实力评级——这对分析即将到来的友谊赛很有用？

同样，对于球员位置，我们将使用 **element_types** 字段:

让我们将这三个表结合起来，得到一个玩家的单一视图。我们将使用`merge()` pandas 函数来连接相关列上的表。玩家可以通过`players.team`和`teams.id`栏加入队伍:

玩家及其各自团队的组合视图

然后我们也可以加入玩家的位置:

球员、球队和位置的综合视图

> *注意上面* `*merge*` *功能的两种不同用法。既可以作为静态函数调用(如* `*pd.merge(left_df, right_df, on= ...)*` *)，也可以作为现有数据帧上的方法调用(如* `*left_df.merge(right_df, on=*` *)。*

## 玩家游戏周历史

现在我们有了一些球员、球队和位置的基本信息。让我们来看看当前赛季的 gameweek 积分。

我们可以通过两种方式做到这一点:

1.  对于每个游戏周的 GID，从**https://fantasy.premierleague.com/api/event/{GID}/**获取所有玩家数据
2.  对于每个玩家 PID，从**https://fantasy . premier league . com/API/element-summary/{ PID }/**获取游戏周历史

因为我们已经在一个数据框架中包含了所有的玩家，所以让我们使用选项 2，以每个玩家为基础获取数据。

**元素摘要**端点在顶层包含三个字段:

1.  **夹具**包含即将到来的夹具信息
2.  **历史**包含历届 gameweek 玩家的分数
3.  **history_past** 提供了前几个赛季的总成绩

我们可以定义一个名为`get_gameweek_history()`的函数，它接受一个参数`player_id`，并返回他们之前所有比赛周的分数数据:

上面是一个调用函数来获取 ID=4 的球员皮埃尔-埃默里克·奥巴姆扬的历史记录的例子。我们可以看到他在赛季开始的前两场比赛中有一个进球和一次助攻，在此之前的三场比赛中没有任何进球贡献。

## 球员赛季历史

我们可以编写一个类似的函数来获得一个球员上个赛季的数据:

上面我们可以看到，梅苏特厄齐尔最好的赛季是 2015/16 赛季，他拿到了 200 分。可悲的是，对于阿森纳球迷来说，他无法在以后的赛季复制这种形式。

# 将这一切结合在一起

我们将通过创建一个`points`表来结束，该表包含本赛季游戏中所有玩家的所有 gameweek 积分。

首先，创建一个包含球员姓名、球队和位置的单一数据框架

创建一个包含位置和球队信息的球员数据框架

接下来，使用 pandas 的`progress_apply()` dataframe 方法将`get_gameweek_history()`函数应用到我们的`players` dataframe 中的每一行。

使用 progress_apply()获取所有玩家的游戏周分数

让我们使用新的数据框架来找出本赛季得分最高的 5 名球员。

# 接下来呢？

希望这能为您自己的分析提供一个良好的起点。我将发表这篇文章的后续文章，完成后会在这里添加一个链接。