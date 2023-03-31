# 烧瓶罩下-SQLAlchemy

> 原文：<https://medium.com/analytics-vidhya/under-the-hood-of-flask-sqlalchemy-793f7b3f11c3?source=collection_archive---------2----------------------->

学习如何用几行代码自己重新创建这个流行的 Flask 扩展

你有没有想过 Flask-SQLAlchemy 扩展是如何工作的？自己建立 SQLAlchemy 会有多难？今天，我们将深入探讨如何设置 SQLAlchemy ORM 以用于 Flask 应用程序。最后，您将有自己的 Flask-SQLAlchemy 版本可供使用！

SQLAlchemy 的目的是为您的 Flask 应用程序提供一种与数据库通信的方法。虽然 Flask 和 SQLAlchemy 文档确实让我晕头转向了几天，但希望我能够将它分解成小块，以便您轻松地消化。为了解释 SQLAlchemy 的各个部分是如何工作的，我将使用一个 Flask 应用程序在电话上与它的好朋友数据库交谈的类比…

![](img/c6b779526785c840b15e7f8872f31cab.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*   [sessionmaker](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/session_api.html%23sqlalchemy.orm.session.sessionmaker&sa=D&ust=1611325253268000&usg=AOvVaw2pNTrdsOq6xYWLM1CC1xKR)

SQLAlchemy 的 sessionmaker 对象是 Flask 用来调用数据库的电话——如果你愿意的话，它是“调用工厂”。当你初始化 sessionmaker 时，你把你希望它进行调用的配置传递给它，这样每次 sessionmaker 进行调用时，它的外观和行为都是一样的。例如，您可以设置创建会话时使用的“绑定”。bind 本质上是告诉 sessionmaker，当 Flask 告诉它“开始呼叫”时，应该呼叫哪个电话联系人，并且它将总是只呼叫那个联系人。

*   [发动机](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/core/connections.html?highlight%3Dengine%23sqlalchemy.engine.Engine&sa=D&ust=1611325310752000&usg=AOvVaw38xcWYMwvq6cBYOtK2sw3Q)

在你能告诉你的会话制造者“电话”打电话给哪个联系人之前，你必须在所说的电话中创建一个联系人。设置 SQLAlchemy 引擎就像创建这个新的联系人。当您运行`sqlalchemy.create_engine(‘database_location’)`时，您传递您希望您的应用程序与之对话的数据库的位置字符串。

一旦创建了引擎，就可以将 sessionmaker bind 设置为等于您的引擎，它将始终调用该引擎。

*   [时段](https://docs.sqlalchemy.org/en/13/orm/session_api.html#sqlalchemy.orm.session.Session)

在我们的电话呼叫示例中，SQLAlchemy 会话对象是呼叫本身。Flask 使用 sessionmaker 对象进行调用/会话，然后使用它来查询数据库、添加新行、更新行、删除行等。

*   [作用域 _ 会话](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/contextual.html%23sqlalchemy.orm.scoping.scoped_session&sa=D&ust=1611325376667000&usg=AOvVaw1zqyHl2o_gk_LykGHkIXIh)

想象你住在一个有几部固定电话的房子里。你正在厨房里打电话，突然你妈妈决定去卧室接电话。她的电话和你的电话连接在同一个电话上。一旦所有人都挂断电话，通话就结束了，必须有人开始新的通话。座机是 SQLAlchemy 的 scoped_session 对象。scoped_session 对象确保家中的每部电话都可以访问同一个呼叫，并且当每个人都按下“结束呼叫”时，呼叫被断开。当手机再次尝试拨打电话时，它会创建一个新的呼叫，而不是继续之前的呼叫。

在固定电话的例子中，通话的持续时间取决于房子里的人何时按下“开始通话”和“结束通话”在 web 应用程序的世界中，这类似于 web 服务器向您的 web 应用程序发出请求。当请求开始时，scoped_session 对象应该使用 sessionmaker 对象创建一个可由 Flask web 应用程序中的所有模块访问的会话。Flask 应用程序使用该会话对数据库进行任何必要的查询或更新。然后请求结束，scoped_session 对象移除当前会话。当一个新的请求到来时，它会创建一个新的会话来处理这个请求。

希望这个类比能让您对使用 SQLAlchemy 创建数据库连接时的工作原理有一个好的理解。现在让我们把它写成代码。

1.首先，让我们创建一个名为“SQLAlchemy”的空类通过将 SQLAlchemy 连接设置为一个类，可以更容易地将各个部分导入到 Flask 应用程序的不同部分。

2.让我们创建 __init__ 函数。这里我们唯一需要做的事情是告诉 SQLAlchemy 对象我们将如何定义数据库中的表。我们将使用声明性扩展，它允许我们一次性定义表和模型，类似于使用普通 Flask-SQLAlchemy 扩展时的做法。我们会将此任务分配给 self。在建立数据库时传递到我们的表中。这里有更多关于声明性扩展的信息。

3.现在让我们创建一个神奇的函数 init _ app 函数。这是我们将运行的连接 flask 应用程序的函数。

我试图在代码中添加注释来解释它在做什么，但是现在让我一行一行地深入解释。

*   第 4 行:我们使用 SQLAlchemy 的 create_engine 函数初始化引擎，并传入数据库的位置(在应用程序的配置中的变量“database”下设置)。
*   第 6 行:我们创建了 sessionmaker，并设置了一些缺省设置，以便在生成会话时使用(比如在第 9 行设置 bind=self.engine，以便所有会话都能访问正确的数据库)。
*   第 13 行:我们通过传入 self.sessionmaker 来初始化 scoped_session 注册表，以便注册表知道如何创建会话。最初注册表是空的，但是当我们的应用程序收到请求并调用 self.session 时，scoped_session 注册表将创建并存储一个与请求相关联的会话。
*   第 15 行:我们将 scoped_session 的 scopefunc 设置为 flask 函数，该函数标识当前应用程序的调用堆栈。该应用程序在每个请求中设置和拆除调用堆栈，因此它是一个合适的对象来绑定我们的会话生命周期。[了解更多关于 scopefuncs](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/contextual.html%23sqlalchemy.orm.scoping.scoped_session&sa=D&ust=1611277354468000&usg=AOvVaw3fDGcaJ12IW-hlB5ZInrdC) 的信息。
*   第 18 行:我们设定自我。Base.query 到 session.query_property()。如果你以前使用过 Flask-SQLAlchemy，你知道你可以直接对一个表运行查询，而不是对 db.session. IE，如果你有一个名为 user 的表，你可以运行 User.query…而不是 db.session.query(User)…
*   第 20 行:这一行创建任何已定义的模型，如果它们还不存在，就在数据库中创建它们。稍后将详细介绍。

4.确保 scoped_session 对象在每次请求后删除当前会话。为此，我们将编写一个调用 self.session.remove()的函数“remove_session”，并将其添加到我们的应用程序的[拆卸请求函数列表](https://www.google.com/url?q=https://flask.palletsprojects.com/en/1.1.x/reqcontext/&sa=D&ust=1611269233322000&usg=AOvVaw1dCVxvTbH91maP3NaHYwW5)(在每个请求结束时运行的函数)。

注意，我们在`init_app()`函数的末尾添加了`app.teardown_request(self.remove_session)`。

10.现在让我们使用 self 来定义我们的数据库表。基本声明性扩展…下面是如何创建表的简短示例:

注意我们是如何从 your_application 导入 db 的。这是假设您在应用程序的 __init__ 中设置了 db = SQLAlchemy()。py 文件。一定要通过 db。基到每个表类中，以便 SQLAlchemy 能够理解它们。您还需要从 SQLAlchemy 导入各种表结构(比如列、整数、字符串等)。

11.设置您的 flask 应用程序工厂并使用 SQLAlchemy ORM！下面是一个简单的 __init__ 的例子。我使用我们在本教程中创建的 FLask_SQLAlchemy 对象创建的 py 文件:

注意，首先我定义了 db = SQLAlchemy()，其次我从定义它们的模块中导入了我的表类，然后我运行了 db.init_app(app)。您必须按照这个顺序做这些事情，否则您会发现您的数据库表没有正确创建。db=SQLAlchemy()创建 db。我们需要定义表的基础。如果在导入表类之前执行 db . init _ app(app ), SQLAlchemy 不会知道它们，也不会在数据库中创建它们。

就是这么做的！[这里有一个关于如何使用 SQLAlchemy session 对象运行查询和向数据库添加内容的简要概述](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/session_basics.html%23basics-of-using-a-session&sa=D&ust=1611277262431000&usg=AOvVaw3u6I_MCMjtmpcueeFjhQwF)。

我从来不喜欢在没有测试之前就创建一些东西，以确保它能正常工作…让我们创建一些测试来确保我们的 Flask-SQLAlchemy 对象能正常工作。SQLAlchemy 文档指出，在 web 应用程序的上下文中，有两件事情需要实现:

1.  当 web 应用程序第一次启动时，创建一个 scoped_session 注册表，并确保该注册表可以跨应用程序访问。
2.  确保在每个请求结束后调用 scoped_session.remove()。

让我们确保我们的模块遵循这两条规则。如果你有兴趣跟随，为了测试，我创建了一个简单的 Flask 应用程序，它创建了一个将用户“注册”到数据库的应用程序。你可以在 github 上找到完整的应用和测试[。](https://github.com/klfoulk16/recreate-flask-sqlalchemy-tutorial)

为了便于测试，我将 init_app 函数中的从上面的示例`self.session = sqlalchemy.orm.scoped_session(self.sessionmaker, scopefunc=flask._app_ctx_stack.__ident_func__)`更改为`self.session = init_scoped_session()`，init_scoped_session 是一个返回原始代码的函数(创建一个 scoped_session 对象)。这样，我可以将一个装饰器绑定到函数上，记录该函数被调用的次数。

1a。当 web 应用程序首次启动时，创建一个单一的 scoped_session 注册表

1b。确保这个 scoped_session 注册表可以跨应用程序访问

2.确保在每个请求结束后调用 scoped_session.remove()。

两次测试都以优异的成绩通过了！

如果你像我一样，你可能仍然对一切是如何工作的感到有点困惑。在这里，我将回顾一下我脑海中的一些问题和我找到的答案。

1.首先，为什么我们为每个请求创建一个新的会话，并在请求端删除它？为什么我们不在应用程序的生命周期内创建一个单独的会话呢？

对于 web 应用程序，将会话的生命周期与请求的生命周期联系起来是意料之中的事情。一个请求，意思是当你的应用程序的一个路径被调用时(例如一个对你的应用程序的“GET”或“POST”请求)。[这个页面](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/contextual.html%23using-thread-local-scope-with-web-applications&sa=D&ust=1611322882873000&usg=AOvVaw1HxbLaDBASTsaOJBWph1rm)有一个很好的图表，展示了遵循这个模式时 SQLAlchemy 绑定的流程应该如何工作。

2.SQLAlchemy 文档指出，当调用 scoped_session 对象时，它将始终返回相同的会话，直到作用域结束并调用 scoped_session.remove。这是如何工作的？

让我展示两个我运行来证明这种行为的测试:

请注意，您必须显式调用 scoped_session 对象(db.session)才能实现这一点。通过调用 db.session()，您将会话对象本身传递给会话 1 和会话 2。如果您只是运行 session1 = db.session，那么您将会传递 scoped_session 对象本身，而第二个测试(断言 session1 和 session2 在不同的请求中是不同的)将会失败，因为 scoped_session 对象本身在不同的请求之间不会改变。

这里有一个例子:

3.用 session = scoped _ Session(Session maker)，调用 Session()和 Session 是一回事吗？例如，在您的代码示例中，为什么您可以调用 db.session，而不是像 SQLAlchemy 文档中的这些示例那样必须调用 db.session = db.session()。

这是一个可爱的东西，叫做隐式方法访问。我推荐在这里阅读[。我在上面的问题中也解释了一下。](https://www.google.com/url?q=https://docs.sqlalchemy.org/en/13/orm/contextual.html%23implicit-method-access&sa=D&ust=1611322882875000&usg=AOvVaw1xijPA4VNXAH3IzELJhkNa)

4.我需要在 flask 的 g 对象中存储什么吗？

不。如果你和我一样，你可能会先试着写一些像这样的代码:这是不必要的。你想多了。scoped_session 对象处理所有这些。

5.你在 PyPi 上发表了你的项目吗？

是的，事实上我做到了。可以通过调用`pip install flask-sqlalchemy-bind`用 pip 安装。点击了解更多信息[。我可能会发表一篇文章，讲述我是如何将它上传到 PyPi 的，以及我为此所经历的过程。但也许不是因为我在这篇文章后有点博客了。](https://pypi.org/project/flask-sqlalchemy-bind/1.0.0/)

希望这以半简洁的方式回答了你所有的问题。如果你还有其他问题(或者如果你在我的思维模式中发现了一些错误),请随时与我在 klf16@my.fsu.edu 联系。