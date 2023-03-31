# 解析 Python Heroku Starter 应用程序中的 HTTP 参数

> 原文：<https://medium.com/analytics-vidhya/parsing-http-params-in-python-heroku-starter-app-2b7810a32e4?source=collection_archive---------4----------------------->

![](img/b8243b0592fb01a9823c8cce419be690.png)

我在这里写的很多故事(如果不是全部的话)都是因为我花了太多时间去寻找一个简单问题的解决方案。这是其中的一次。

我正在做一个项目，在这个项目中，我要构建一个服务器来响应简单的 GET API 请求。Heroku 是一个推荐的解决方案，由于我能够启动并运行这个项目，我感觉很好。

接下来，是修改代码和接受参数的时候了，格式是

```
curl -s [https://url.com?var=value](https://url.com?var=value)
```

这是我溜进栈溢出兔子洞的地方。我在您的代码中看到了许多发送 GET 请求的方法(不是我想要的)和解析非 url 格式请求的方法(不幸的是，我被命令格式束缚住了手脚)。

然而，最终我通过仔细查看启动项目发现了这个问题，并且，带着一点点运气，我发现了一个非常简单的方法来修改[启动项目](https://devcenter.heroku.com/articles/getting-started-with-python#prepare-the-app)以接受一个参数。

默认情况下，您用`**heroku create**`生成的 URL 会显示一个基本的登陆页面。让我们删除它。在 hello/views.py 中，注释掉 `def index(request):`函数中的代码行`return render(request, “index.html”)`。然后，如果您取消对`return HttpResponse(‘Hello from Python!’)`的注释并将您的更改推送到 heroku main，当您转到 URL 时，您将会看到“Hello from Python”字符串。

我们知道如何发送 HttpResponse，但是现在让我们解析 http 请求。我开始这个项目是为了一个智能井字游戏服务器，所以我想做的是接受一个游戏棋盘状态的 X、O 和+(空格)的字符串表示。所以从现在开始，我将通过添加一个参数来测试我的 url，例如 https://<myserver>.herokuapp.com/？board = xoxox ++ x .注意，如果你只是带着一个参数去 url，它不会影响返回的响应。</myserver>

现在来解析 url 参数。您可以使用以下命令获取 Http 请求的参数:

```
board = request.GET.get('board')
```

然后将可变板作为字符串处理:

```
from django.http import HttpResponse, HttpResponseBadRequest...if board is None: return HttpResponseBadRequest("no board parameter received")if len(board)!=9: return HttpResponseBadRequest("incorrect number of spaces")#TO-DO: game logicreturn HttpResponse(board)
```

现在，如果您不带参数地访问链接或`curl -s <URL>` ，将会显示一条 HttpResponseBadRequest 消息(无论您使用什么作为 HttpResponseBadRequest()的输入)。同样，如果 board 参数的字符数不正确，则会发送错误的请求。否则，“游戏”继续进行，并返回一个棋盘。

编码快乐！
科琳