# 使用 JS+Flask+Postgres+Firebase+Heroku 构建全栈 webApp

> 原文：<https://medium.com/analytics-vidhya/building-full-stack-webapp-using-js-flask-postgres-firebase-heroku-7b3ec622a345?source=collection_archive---------0----------------------->

使用 vanilla JS 中的 frontend，Flask framework 中的 backend，用于后端数据库连接的 Postgres，用于存储的 firebase，用于发送电子邮件的 sendgrid，最后使用 Heroku 云服务器和 Github 工作流，部署一个完整的 web app。

![](img/0ec3cda07d4f18b0d19d70af69af65b7.png)

我们将使用由以下生命周期组成的非常模块化的软件工程方法来解决这个问题:

> [需求分析(根据涉众描述需求)](#8a70)
> 
> [规划(团队规模估算、成本估算)](#34a0)
> 
> [设计(UI/UX 流程，定义架构)](#646d)
> 
> [构建(设计数据库模型，开发前端、后端代码库)](#2cf6)
> 
> [测试(单元测试、github 静态代码分析、冒烟测试)](#7387)
> 
> [部署(使用 Heroku 服务器分两个阶段部署，即准备阶段和生产阶段)](#3910)

![](img/582780c4a72d4756c89ecdf458c60e8c.png)

软件开发生命周期

> 如果你想直接跳到构建/开发过程，你可以这样做，但我真的希望你在开发之前至少过一遍设计部分，以获得我们将要构建的感觉。

所以，不要等待和浪费你的时间，让我们直接进入第一步:

> ➡️*需求收集*

这一阶段需要进行市场研究，并通过各种过程(如问卷调查、讨论、集思广益会、研讨会等)从最终用户或选定的客户以及每个利益相关者那里收集各种可能的反馈和要求。

显然，我们不会做这项研究，您可以放心地认为我已经为您做了这项研究:)

> 假设我们要建立一个系统，人们只需点击一下就可以轻松地在板/卡上创建一个任务，并保持它的安全，我们还可以跟踪用户从任务创建到完成的旅程，并鼓励用户尽快完成任务，同时不要让用户从其他工作中分心，特别是要给用户一个很好的 UI/UX 感觉。

这是问题陈述的高级概述。现在，作为一名软件开发人员，我们必须在这里问很多问题，但请稍等片刻，您的所有问题很快就会得到解答。

让我们为我们的系统写功能性和非功能性需求—

一般我们用 [*SRS*](https://www.geeksforgeeks.org/software-requirement-specification-srs-format/) 来写这些需求。

功能性—

*   用户能够通过我们的注册页面/登录页面无缝注册。
*   用户能够非常容易地创建一个任务，没有任何障碍
*   用户可以通过以下方式应用推送通知进行提醒:
    - >电子邮件
    - >电话
*   用户可以更改任务的状态/标记→默认情况下，我们可以显示“todo”类型的标记
*   Schedular 将向所有为这些任务设置了提醒的用户发送提醒
*   用户能够编辑任务描述，删除任务。

非功能性—

*   用户密码应该得到安全保护
*   注销用户后，任何人都不能使用浏览器中的后退按钮重新登录其他用户的帐户。
*   无缝的用户界面/UX 体验
*   出色的性能，两次操作之间无延迟
*   系统始终为用户服务，并在用户群增长时处理负载。

> 🆕我们还将命名我们的应用程序:“**烧瓶-任务-管理器-应用程序**”或任何你想拥有的东西。

🌟现在，可用于需求收集和分析的强大工具有:

(为了更好的建模和可视化，我们通常使用 [UML 图](https://creately.com/blog/diagrams/uml-diagram-types-examples/)

![](img/6704c4ad57074961d65abb8911ad03ce.png)

更好的需求分析和建模工具

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

> *➡️* 策划

![](img/78822339987449e1a1e29c1f74a22cb0.png)

[规划图](https://www.geeksforgeeks.org/software-engineering-project-planning/)

该阶段主要涉及以下四项活动:

项目成本，开发项目的成本和持续时间，完成项目所涉及的努力和风险。

你可能会花费的唯一成本是互联网连接，一台笔记本电脑，最有可能是几个小时的时间表，其他一切都将由各种令人敬畏的服务提供商免费托管。在发展的每个阶段，我们可以分析和选择对我们来说最好的选择。

我们现在真的不能有这些估计，所以，我们可能会跳过这个阶段，但知道这些是很好的。

🌟现在，有一些很棒的规划工具:

![](img/fd23e48d9f824bbad34e2031c9cfe847.png)

更好规划的工具

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

> *➡️* 设计

所以，现在我们来到设计部分，在这里我们将描述系统的[架构](https://en.wikipedia.org/wiki/Systems_architecture#:~:text=A%20system%20architecture%20is%20the,more%20views%20of%20a%20system.&text=A%20system%20architecture%20can%20consist,to%20implement%20the%20overall%20system.)看起来如何，以及系统的 [UI/UX](https://en.wikipedia.org/wiki/User_interface_design) 看起来如何。

首先，记住我们所有的需求，我们在第一阶段已经定义了所有的需求，让我们开始设计我们的应用程序，根据业务逻辑将它分解成组件，因为最终，系统的每个组件都必须服务于一些与之相关的业务逻辑。

![](img/3187dbccedc3b28d2d9d3d7685652f70.png)

现在，让我们一个一个地考虑一下。

![](img/5a522475e0b46cf2856d33472eb7173f.png)

当一个用户想要转到他的仪表板页面开始创建那些任务时，哪种类型的组件可以用于这种场景？首先，用户必须登录系统，对吗？或者，如果用户是新用户，那么它可以使用注册流程创建其凭据，对吗？

是的，你得到了它，它必须是授权模块，可以帮助用户做到这一点以上的情况。

![](img/a77140f7cf612430b6387ada9e2c88f8.png)

什么类型的组件可以用于用户希望看到其个性化配置文件的场景，我的意思是，想想看，每个用户都应该能够获得其定制的页面，对不对？

因此，这里我们可以有某种类型的用户相关模块，它可以为我们提供所需的用户信息，以便我们可以向登录的用户显示特定信息。

![](img/85c24f93ca65ba0148afc6bd55e0f293.png)

什么类型的组件应该能够显示用户已经创建的所有任务，我们需要显示用户已经创建的所有任务，对吗？我们还需要显示用户对创建的任务的任何控制，比如它是否可以编辑、删除或订阅该任务的通知？任务的状态如何？每个任务都可能不同。

因此，这里我们可以有某种任务相关模块(假设我们将其命名为产品模块)，它为我们提供该用户的所有任务相关信息。

![](img/28809203cccf3211230e5a11bd6eea6b.png)

什么类型的组件应该能够让用户能够通过订阅特定任务的通知来设置该任务的通知，并且还可以获取用户的信息来发出通知，如电子邮件、电话等。

我们必须允许用户设置通知或取消订阅，对吗？

因此，我们需要一个通知组件来帮助用户订阅/取消订阅特定任务的通知。

![](img/c20dc3560aefdba9f163635ab8a9b66f.png)

现在，最后，我们可以创建一些 schedular 组件，它可以自动发送任何自动邮件来提醒用户完成他们的任务，直到用户完成任务或取消订阅通知。
让我们称之为 CRON 组件(基本上是基于 Unix 的系统，或者一般来说，今天的每个框架都有某种类型的调度服务，它在某种程度上与 CRON 作业或调度作业相关，这是它的名称)。

👏如果你没有跳过，那就做得很好！

现在，您已经在很高的层次上了解了您的系统需求。

让我们将它们命名为服务，因为所有这些都服务于某种业务逻辑并提供特定的服务——一项工作/任务，它应该做而不知道其他组件做什么。

![](img/3cd6ae8632d0a89d90076deea89f729a.png)

系统的高级组件

从上图可以清楚地看到，我们需要创建五个组件。

现在，问题是这些组件之间有什么共同点吗？这些成分之间有什么联系吗？

合乎逻辑的假设是，可以创建这些服务并使其独立工作，而不会干扰任何其他服务。

当然，有时您会发现，我们可以利用一个服务与另一个服务相互通信并获取数据，以满足后者的需求，但在这种情况下，为了保持简单和灵活，而不需要引入任何服务对其他服务的依赖性，我们可以让所有服务独立。

这就把我们带到了[微服务](https://en.wikipedia.org/wiki/Microservices)的世界，其中每个服务都可以独立行动，并独自服务于目的，但它是松散耦合的，因此如果我们想要扩展我们的系统，我们无法承受完全解耦的系统，所以我们需要一些耦合—松散耦合。

因此，假设我们希望我们的产品服务知道用户服务可以提供什么，或者它向外部世界公开了什么接口，以便交流和利用用户服务可以提供的信息，从而完成其自身的需求，如果产品服务引入了任何更改，那么它可以很容易地完成，而不会对其他服务产生很小或没有影响。

我们将允许所有服务使用 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) (s)协议在内部或从我们的前端 webapp 进行通信。

REST 对我们很有用，因为它实现简单，可以灵活地独立扩展和部署我们的服务，并通过简单的接口实现通信，客户可以根据与每个服务相关的业务目标来理解这些接口。

我们要选择的技术堆栈如上所述—

Python (Flask)用于更快更简单的开发，纯 vanillaJS 用于前端，因为不涉及太多复杂性，但是的，我们肯定会看到一些改进，并了解为什么将我们的前端应用程序分解为小组件是有用的——每个新框架/库利用的可扩展想法付诸行动，Firebase 用于安全存储资产，如用户配置文件图片，Heroku 用于云上的服务器部署，最后 Postgres 用于数据库，它可以轻松地与 Flask 集成，Heroku 支持与它的直接连接，这可以使它在生产上非常快。

👉这里是我们的用户界面/UX 部分，我们将看到我们的前端应该是什么样子。

一般来说，有一个独立的 UI/UX 设计团队负责这些组件，有一个设计系统，通常遵循每个页面上的所有按钮看起来几乎相同，这适用于每个组件，它应该在 webapp 的页面上保持一致。

当你构建一个 webapp 时，有一些通用的组件，比如 header、footer，我们只需编写一次就可以在任何地方重复使用。因此，我们应该在编写 css 时寻找并牢记这一点。

我将分享我选择的设计组件的链接，可能你也会喜欢:)

[点击](https://www.figma.com/file/Kpl19BFDGrELUma5bfBF3j/Taskly-frontend?node-id=0%3A1)查看描述流程的完整设计。

我会放一些 [figma](https://www.figma.com/) 的截图给你，让你感受一下我们正在建造的东西。

![](img/d431456131f9a8f45b6f3244878e68d5.png)

通用组件

![](img/f8423420b85570df1976917e014b6a99.png)

taskly 登录主页

![](img/bf5dcebefcb345145a9a01c0e970cde1.png)

taskly 仪表板页面

![](img/d72731e7a030b084823dd14cff847375.png)

任务密码更改页面

🌟现在，有一些很棒的工具可以用来设计:

![](img/844c51fa11b84ac80a5f05513299da13.png)

更好设计的工具

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

> *➡️* 大厦

现在，让我们开始开发，记住我们为系统所做的所有设计。

让我们在开始之前做一些决定。我们应该如何构建我们的目录以实现更快更容易的开发？

我们应该如何构建我们所有的 HTML、CSS、JS 文件和其他资产？

我们先来看看前端部分:

![](img/2be0661e0fef9c4584bc35d9b7a288ad.png)

任务前端目录结构

此外，我们应该如何保持我们的 python 运行时环境相互独立，以便在版本方面没有包/模块冲突，或者只是分离我们的环境变量？

现在让我们来看看后端部分:

![](img/24b3ac707803bf93eb7199bef1399a16.png)

任务-后端-目录-结构

![](img/478d1a6559d18b091d76e60e5df173b5.png)

放心吧！我们将浏览每个文件，为什么它是这个 webapp 所需要的，以及我们如何使用以下不同的前端和后端组件来满足系统的要求。

让我们开始构建我们系统的前端，只关注主要组件，因为其他组件很难实现。

1.  主页——我们需要构建登录表单并注册模块。

2.仪表板页面-我们需要建立任务创建部分，为已经创建的任务显示任务桶，订阅/取消订阅模式，上传个人资料 pic 模式，任务状态模式的改变和标题上的下拉菜单。

3.更改密码页面—我们需要分两部分构建更改密码表单。
首先，我们需要验证，如果用户首先在我们的记录中，那么只有我们继续给用户更改密码的选项。

我们将构建一个获取用户数据并在页面上设置数据的示例函数:

有一种更好的方式来管理每项微服务的所有主机:

创建一个单独的配置文件，并将所有主机放在那里。

将您的代码分成单独的组件，因为有些组件是可重用的，通过编写一次代码并尽可能多地重用它们来相应地加载它们。

让我们设计我们的数据模型。

让我们问一个问题——应该为这个 webapp 存储什么数据来满足业务需求？

首先，我们希望存储用户的登录凭证信息和他们的名字、姓氏(在注册时)。

然后，我们可能会在他们的手机、电子邮件上发送通知，这样也必须在某个时间点存储通知。

我们还需要使用某个 id—“user _ id”来唯一地标识用户

很好，我们确定了用户信息的所有要求。

数据需要什么格式？—

数据可以以表格的形式进行逻辑结构化，并且对于该系统，我们更重视一致性而不是可用性(阅读关于 [CAP](https://en.wikipedia.org/wiki/CAP_theorem) )。

类似地，我们可以为用户创建的任务构建一个关系，每个任务可以与一个惟一的 user_id 相关联。

数据存储之间有任何关系吗？

因此，我们可以在两个数据存储中看到 user_id 的外键类型的关系(从现在起我们在这里称它们为表，因为我们现在确信用户 [RDMS](https://en.wikipedia.org/wiki/Relational_database) 。

让我们说服自己，下面的表格将用于我们的系统。

![](img/d5bbac47dfd2f1ea759af009b1caf89f.png)

用户表

![](img/cc8506a8ea30ddb9da1b9b1f8cc4033d.png)

任务列表表

user_id，list_id 应该是[主键](https://en.wikipedia.org/wiki/Primary_key)，因为我们需要唯一地标识 users 表中的用户和 task_list 表中的任务。

user_id 是 task_list 中的[外键](https://en.wikipedia.org/wiki/Foreign_key)，它引用 users 表中的 user_id。

如果需要，可以通过执行复杂的[连接](https://en.wikipedia.org/wiki/Join_(SQL))来获取数据。
此外，如果您注意到我们在 task_list 表中有一些默认值，因为每当我们创建新任务时，它都必须标记为“Todo”。

类似地，我们可以知道“is_email_pushed”、“is_phone_pushed”的值可以是 0，这表示用户是否已经通过任务的相应方式订阅了通知。

> N **注**:我们应该总是尽量不在数据库中存储任何机密信息，并且只在用户尝试使用该功能并明确授予他们权限时才尝试从用户那里获取，这就是为什么我们在注册时也不存储任何额外信息，这可能会成为天真用户的障碍。
> 用户隐私是必须的，我们必须尊重用户隐私以维护用户的信任。

现在让我们开始我们的系统后端。

*   用 Flask Blueprint 构建完整的 API

在开始之前，让我们创建如下所示的目录结构:

![](img/33c2e3cc5bd2e23f93a3cb15c540bd53.png)

此外，让我们使用这个令人敬畏的包管理器" **pipenv** "，它可以帮助跟踪 requirements.txt 文件(该文件包含运行我们的 flask 应用程序所需版本的所有依赖项)。

```
pipenv install flask
```

它将在[packages]下创建一个包含所有依赖项的 Pipfile。

```
[packages]flask = "*"
```

现在，让我们理解基本 API 和协议方法的请求和响应。

我们将使用基于 JSON 的消息来发送请求和响应，因为我们只获取一些信息，所以我们将使用 **GET** 方法。

我们应该从逻辑上定义我们的端点，这样任何开发人员都无需阅读太多文档就能理解。

在构建精确的 API 之前，我们通常会编写一个 API 契约，其中定义了以下内容:

```
endpoint : /user/fetch-infoMethod : GET  
request : user_id=1
response : {"user_id": "1", "firstname": "sourav", "lastname":  "kumar"}
```

现在，在决定了契约之后，我们编写下面的代码(让我们暂时不要深入数据库连接，使用内存中的程序) :

基本烧瓶-应用程序示例

```
export FLASK_DEBUG=1
export FLASK_APP=app.pyflask run -p 8080
```

这将在端口 8080 上运行我们的应用程序，并在 developement server 运行时使用默认的本地主机(启用调试器)，我们的应用程序将在

```
http://127.0.0.1:8081/
```

由于我们将路由定义为/user/fetch-info，这也需要一些参数，所以我们需要从浏览器或终端发出请求，或者更好地使用 Postman 进行测试:

```
http://127.0.0.1:8081/user/fetch-info?user_id=1
```

将获取以下响应:

```
response body: { "firstname": "sourav", "lastname": "kumar"}response status : 200 
```

这里有什么东西不见了吗？不，不包括响应，但是如果我们在终端上记录完整的请求参数和完整的响应对象以获得更好的信息，不是更好吗？

我们可以使用以下四个[装饰者](https://www.geeksforgeeks.org/decorators-in-python/)来做到这一点，你可以在这里阅读:

```
@app.before_request
@app.after_request
```

![](img/728d4863290333aa7fbdc550c820a338.png)

终端日志

如果我们继续构建更多带有端点的 api，比如 product/fetch-product-info、user/authorize-user 等，你会发现这个流程有问题吗

如果我们在 app.py 中构建所有东西，那么这将导致在一个文件中管理所有东西的非常大的问题，并且当你搜索某些东西时会迷失，并且很难在一个文件中记录所有东西。

合乎逻辑的方法是将目录划分为服务，然后为每个服务创建单独的 app.py 文件 flask Blueprint 提供了现成的东西来管理大型项目和扩展开发流程。

现在让我们回到前面显示的目录结构(但是现在没有其他不相关的文件) :

![](img/c2a4c8c73c078a18aafefc9206ca6980.png)

`[**Blueprint**](https://flask.palletsprojects.com/en/2.0.x/api/#flask.Blueprint)`是一种组织一组相关视图和其他代码的方式。它们不是直接向应用程序注册视图和其他代码，而是向蓝图注册。

现在，让我们使用 blueprint 构建上面的 API。

**user.py**

**app.py**

看到了吗？现在我们可以很容易地添加更多的视图和蓝图，它们都是单独定义的，并且可以很容易地扩展开发过程。

*   Postgres 与 flask 应用程序的连接

如果不将用户的数据安全地存储在数据库中，并通过将数据库连接到 flask 应用程序来为用户提供实时体验，那还有什么乐趣可言

那我们开始吧。

首先，让我们从这里的[设置你的数据库](https://www.youtube.com/watch?v=Q8iYj2ypWss)并获取[的 pgadmin](https://www.pgadmin.org/download/) 。

```
pipenv install psycopg2-binary 
```

按照设计部分所示创建表格，或者根据需要进行修改。

保持我们的 **app.py** 和以前一样，将 **user.py** 改为

这还将记录 SQL 查询、受影响的行数、选定的数据和其他对调试更有用的信息。

如果你想设置 MYSQL 而不想设置 Postgres，请不要担心—

你只需要从[这里](https://www.youtube.com/watch?v=IWXQeXHDerg)设置 Mysql，得到[工作台](https://dev.mysql.com/downloads/workbench/)。

同样，您只需要更改 **user.py —**

```
pipenv install flask-mysql
```

然后，

```
from flaskext.mysql import MySQLmysql = MySQL()app.config['MYSQL_DATABASE_USER'] = "root"
app.config['MYSQL_DATABASE_PASSWORD'] = "password"
app.config['MYSQL_DATABASE_DB'] = "developement"
app.config['MYSQL_DATABASE_HOST'] = "localhost"mysql.init_app(app)
```

这里有个小缺口

![](img/d7f858bcbf535de55f33157b19190d51.png)

*   环境变量的重要性

你在上面的代码中发现了一些安全问题吗？我们在源代码中公开我们所有的秘密凭证…..根本不应该这样做，这会危及数据库的安全。

那么，有什么解决办法呢？环境变量是我们的救星。

我们应该从终端本身设置我们的环境变量，并且通常避免在源代码的任何部分使用任何秘密凭证，或者创建一个**。env** 文件，并通过包含在**中来忽略 git 要跟踪的这些文件。gitignore** 。

```
export DATABASE_URL = "postgres://hhkjrxymeo:89hskhfks@kjhff/kjfskh"
```

代码的变化就像

```
DATABASE_URL = os.getenv('DATABASE_URL')
```

因此，从现在开始，我们将继续使用这些 env 变量。

*   火基集成

现在，我们已经完成了基本设置，可以向用户显示基本信息了——他们从应用程序登录后的个人详细信息，以及他们在任务桶卡中已经创建的任务。

如果用户想改变/修改我们提供的默认个人资料图片怎么办？

我们需要为这些对象提供一些安全的存储——fire base[来拯救我们，这是一种更快、更安全的方式来实现我们的目标。](https://firebase.google.com/)

让我们继续将 firebase 集成到我们的 flask 应用程序中。

从[这里](https://firebase.google.com/docs/database/rest/start)设置你的 firebase 账户，然后我们去 [Pyrebase4](https://github.com/nhorvath/Pyrebase4) 库。

我们需要对我们的应用程序代码做很小的改变，无论你在哪里建立你的 API 来上传图片。

确保您拥有以下凭据(您可以从 firebase 存储控制台获得这些凭据)—

```
firebaseConfig = {"apiKey": "sdjhfdkjskdh43535hk","authDomain": "flask-app.firebaseapp.com","projectId": "flask-app","storageBucket": "flask-app.appspot.com","messagingSenderId": "98437593998","appId": "1:98437593998:web:983759kjh98759","measurementId": "H-43578678BHGH","databaseURL": ""}import pyrebasefirebase = pyrebase.initialize_app(firebaseConfig)
storage = firebase.storage()profile_pic = request.files['file']user_id = request.form["user_id"]APP_UPLOAD_FIREBASE_PATH = f"images/{user_id}.png"storage.child(APP_UPLOAD_FIREBASE_PATH).put(profile_pic)
```

您将看到您的个人资料图片上传到 firebase 存储控制台的 images 目录下，并带有唯一的 user_id 标识符。

*   使用 Jinja2 为邮件发送者和生成邮件模板集成 Sendgrid。

比方说，我们现在希望通过构建我们自己的自动化邮件系统来提高用户参与度，Sendgrid 是我们通过电子邮件联系用户的一种有效、安全的方式。

这将有助于两件事—

1.  当用户订阅和取消订阅任务桶时发送用户通知。
2.  创建 CRON 调度程序作业。

我们先来看看如何集成 sendgrid。从受信任的电子邮件中打开您已验证的 sendgrid 帐户，并在此从[设置 sendgrid。](https://github.com/sendgrid/sendgrid-python)

保持你的 HTML 邮件在新创建的模板目录下，其他目录结构保持不变。
顺便说一句，[这里的](https://stripo.email/)是一个在网上创建精彩邮件的伟大工具。

```
pipenv install sendgrid
```

记住要保证 sendgrid API 密匙的安全。包封/包围（动词 envelop 的简写）

Jinja 是一个用于 Python 编程语言的 web 模板引擎，我们将在这里使用它将 HTML 转换成模板，flask 可以理解，我们的 web 服务器可以轻松地发送邮件。它有特殊的占位符，可以使用我们传递给它的变量动态生成模板。

```
from sendgrid import SendGridAPIClientfrom sendgrid.helpers.mail import Mailimport jinja2
```

现在，您知道了如何通过 sendgrid 发送您的定制邮件。

让我们看看如何在 flask 中使用 CRON scheduler 使用[高级 Python 调度器](https://apscheduler.readthedocs.io/en/stable/)来调度您的邮件或任何类型的作业。

使用这个 flask 扩展创建 CRON 作业是非常简单有效的方法，你可以阅读更多关于它的内容，为繁重的后台任务创建自己的调度程序，这不会干扰你的主要可执行应用程序或服务。

![](img/f0a4aeef7562f1a883fd6227f2daed61.png)

最后，我们(几乎)完成了我们在设计中决定构建的所有内容。

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

> *➡️* 测试

现在是时候打破我们的代码了！

针对系统的所有限制进行测试。想想边缘案例。

![](img/021734b38919ca6a8192f6c8573df183.png)

老实说，你应该在开发的每个阶段彻底测试你的代码，然后由你团队中的同事通过代码审查过程，我们还使用静态代码分析器来发现代码中的任何一般性错误和安全问题。我们可以在 Github 上使用 [codeql](https://github.com/github/codeql) 作为我们的用例。
阅读不同类型的[测试](https://www.geeksforgeeks.org/types-software-testing/)。

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

> *➡️* 部署

现在是最终部署系统的时候了——我们都在等待让我们的用户最终使用我们制造的系统的时候了。

我们将看到将我们的前端系统部署到 [Github pages](https://pages.github.com/) 并将后端系统部署到 [Heroku](https://www.heroku.com/) 的所有流程，以及如何通过整合一切并保持服务于用户请求来管理所有服务，我们将通过创建一个支持更快部署的工作流管道来实现自动部署。

让我们使用 [this](https://docs.github.com/en/github/importing-your-projects-to-github/importing-source-code-to-github/adding-an-existing-project-to-github-using-the-command-line) 将我们的代码推送到 Github，并保持其公共存储库，然后使用 [Github pages](https://pages.github.com/) 部署您的代码。

其他一切都将由 github 管理，你的网页应该在 Github 分配的域名生活。

在将我们的后端服务部署到 [Heroku](https://dashboard.heroku.com/) 之前，我们必须了解 [Heroku 基础设施和 dynos](https://devcenter.heroku.com/articles/dynos)——Heroku 的主干，还必须了解[管道](https://devcenter.heroku.com/articles/pipelines)。

现在，让我们在部署之前准备一些文件，这有助于 Heroku 更好地了解您的应用程序。

**过程文件**

```
web: gunicorn wsgi:app
```

**runtime.txt** (如果您的机器上有其他版本，请更改您的版本)

```
python-3.8.10
```

**wsgi.py**

```
from app import appif __name__ == '__main__': port = int(os.environ.get('PORT')) app.run(port=port)
```

注意:不要为 Heroku 设置端口变量。

现在，首先，我们必须将我们的代码从[这里](https://docs.github.com/en/github/importing-your-projects-to-github/importing-source-code-to-github/adding-an-existing-project-to-github-using-the-command-line)上传到 [Github](https://github.com/) ，以便更好地与 Heroku 集成(如果不想向外界公开，您可以将您的存储库[设为私有](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/setting-repository-visibility)，以获得更好的安全性)。

我们现在将在 Heroku 上部署我们的后端。

首次注册后，您将看到以下屏幕—

![](img/7cf750cdf909b14ae8b2221c0b9fefa2.png)

让我们首先通过单击页面右上角的“new”下拉按钮创建一个新的管道，如图所示。

![](img/f83670a5cfdc93d11eed06465dbdafce.png)

现在，填写所有细节并连接到您的 github 帐户，以便更好地集成部署。

![](img/bf3d415e347d761a08c62c2b33a67926.png)

在与 Github 集成并创建 pipeline 之后，您可以看到我们有各种选项来创建我们的工作流—

主要是，我们将使用暂存和生产。

因此，我们的想法是，我们将从 Github 上的一个分支自动部署到 Heroku 的 staging 应用程序，我们将对其进行测试，如果一切正常，那么我们可以将其推广到生产应用程序，您可以与外界/用户共享您的应用程序。

现在，你在下面的屏幕。点击**为试运行和生产添加应用**，并为您的应用命名。

![](img/ae8cd13bdc0428b0bcc1f4c258ef632d.png)

现在，关注下面所示的区域:

![](img/11bd30a7d3f1ce1b03bba298a621025f.png)

现在，单击暂存应用程序并打开该暂存应用程序的仪表板。

然后打开页面的**部署**页签，选中该选项启用自动部署，并选择自动部署的分支。

![](img/0d8fae0896a9b60b6d92576ad77177e3.png)

如果您已经将代码推送到 Github，那么第一次您应该进行手动部署(之后，每次推送到这个分支的代码都会自动部署到 Heroku)。

![](img/825b68ae14bc9d47768a77d4c41e55d1.png)

注意:包括您的**管道文件**，因为它将帮助 Heroku 在构建过程中安装所有的依赖项。

您可以在 app 页面的**活动**选项卡中看到构建过程日志。

以下是应用仪表板页面中最重要的选项卡。

![](img/dbe619c26cc7e7ec42e34f36898be597.png)

现在，最重要的是使用它直接支持的 Heroku Postgres，所以我们需要获得数据库的凭证。

现在，转到页面上的**资源**选项卡。

从这个标签安装 Heroku 插件——Heroku Postgres。

![](img/ca6341e942af862a1bad57cce7dcf580.png)

然后，通过点击 postgres 插件打开 datastore。

![](img/c4fd5754a67bc1eb1bab2aa3a9978b2b.png)

在这里，您可以看到关于您的数据库和实时连接的一切。

点击设置并获取您的凭证。

复制这个 **URI** 值并把它保存在某个安全的地方，因为你必须在你的环境变量“DATABASE_URL”中设置它，就像我们在代码中使用它一样。

现在，使用应用程序页面仪表板上的**设置**选项卡中的配置变量设置您的环境变量(用于数据库或任何其他使用的密钥)。
这里也设置**build packs**——选择为 *heroku/python* 。

![](img/90f2370e902545c7876464a92b9bf987.png)

最后，我们可以从右上角**打开的应用中看到我们的应用。**

![](img/ac5b7f625d20fe20f55445bfe26b877e.png)

如果您没有为默认路线设置任何查看功能，那么点击此按钮并打开您的网站将会显示“403 错误-未找到”。

![](img/9e4ddbabc16a65311b449edb865d40ee.png)

你现在可以使用 [postman](https://www.postman.com/) 测试你的应用程序，点击定义请求的 API 的 URL，看看你的响应是否是 200，主体是否与我们在上面定义的 API 相同。

此外，您可以设置您的默认路由“/”，如下所示，这样您就不会得到任何错误，也可以测试您的服务。

```
@user.route('/', methods=['GET'])
def test_user_service():
    return ("<h1> User service is up and running !</h1>")
```

这将确保您不会在默认路由上出错。

现在，继续检查您的应用程序是否在以下路线上可用:

```
https://flask-task-manager-app.herokuapp.com/user/fetch-info
```

带有必需的请求。

此外，对于调度程序，您必须如下所示更改您的 *Procfile* ，并从[这里](https://devcenter.heroku.com/articles/scheduler)读取您想要使用的调度程序:

```
worker: python app.py
```

注意:Heroku 上的 web 进程将在大约 30 分钟不活动后休眠！

![](img/b820bfe663ba22120c6228775d031c31.png)

不要担心，这并不意味着你的应用程序会消亡，一旦它收到请求，它会再次产生动态，并会保持活跃，直到再次 30 分钟不活跃期。

这可以确保你不会被不必要的收费，也许会给第一个点击请求的用户带来不好的体验(因为目前你的用户群很小)，但这不是问题，因为之后任何其他请求都不会有问题，而且它通常会在睡眠后 30 秒内再次产生。

测试你的后端 API，如果一切正常，就从*试运行*到*生产*。

![](img/a0b9b34954032bed6300e9c786b0cae8.png)

最后，打开前端系统的配置文件，并将用于获取 API 的主机设置为生产的 Heroku 域主机(您可以通过打开生产应用控制面板页面并单击“打开应用”进行检查)。

你的前端现在应该与后端应用程序集成，你的应用程序应该为最终用户能够使用他们的需求。

![](img/ecad60fbf0566deb275f028d899d7fba.png)![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

如果你能走到这里，你应该得到无数的掌声👏👏👏👏

你真的应该得到更多——让我们把它完成吧！

如果你能在你的终端上看到你所有的日志，那该有多好？(你不能用你的免费账号在 Heroku 上这么做)——这是可能的，而且在调试你的应用时非常有用。

让我们从非常基本有用的命令开始，让你的应用程序连接到终端，并查看你的实时日志和服务器状态。

首先，您必须登录 Heroku，然后您可以开始使用进一步的命令(作为管理员用户登录到您的终端)。

```
heroku login
```

这将带你到新的网页(如果不是，然后手动打开链接，这是显示在您的终端)，点击登录。

回到你的终端。

检查你的所有应用程序—

```
heroku apps
```

检查您的服务器状态和动态类型—

```
heroku ps -a flask-task-manager-app
```

检查您的实时服务器日志—

```
heroku logs --tail -a flask-task-manager-app
```

你甚至可以管理你所有的数据库连接和设置，并使用[这里](https://devcenter.heroku.com/articles/heroku-postgresql)的更多这样的命令来查看你的实时表格。

![](img/20647cb77802f9e366db6e6eedb5f3aa.png)

为进一步阅读和继续你的旅程提供了很好的参考

*   [https://flask . pallets projects . com/en/2.0 . x/patterns/index . html](https://flask.palletsprojects.com/en/2.0.x/patterns/index.html)
*   [https://stack overflow . blog/2020/03/02/best-practices-for-rest-API-design/](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
*   https://devcenter.heroku.com/categories/app-performance

✔️:很快，我将发布所有的代码开源，并在完成后在这里更新，因为在发布之前有一些问题需要解决。

更新:代码发布并开源。

![](img/b4dc7b501cdf03cdf91263f72973b1f3.png)

如果你想看我的最终版本，你可以点击这里:

![](img/8450116519785202caafccdcae181cbe.png) [## 任务性|家庭

### 最佳任务管理器-简单高效，适合你

souravs17031999.github.io](https://souravs17031999.github.io/taskly-home/) 

如果你想保持联系，请关注这个帐户，并在每个周末关注新的应用程序或新项目。

下周末看这个空间！