# Python 中的重试装饰器

> 原文：<https://medium.com/analytics-vidhya/retry-decorator-in-python-55d0729755c7?source=collection_archive---------1----------------------->

![](img/0d8c9440e902b6ec5f6dc1adee7107e2.png)

在本帖中，我们将讨论重试装饰器以及如何在 python 中使用它。

一般来说，我们可能遇到过很多这样的情况，我们希望执行一段代码，直到得到预期的结果，或者我们可能需要等待某个操作完成后再继续。

在直接进入实现之前，我将解释一个简单的用例，其中我必须使用重试装饰器。

使用案例是，有一个 API 具有以下端点:-

```
1.api/getData
2.api/status
3.api/data
```

api/getData，返回“ **id** ”，而不是直接为每个请求返回数据(有一些输入参数)。

api/status，取 api/getData 返回的参数“id”，检查那个 **id** 的状态是否为**成功**(返回**真/假**)。

在 api/status 返回 true 之前，我们无法请求 api/data，因为数据尚未解析。因此，api/status 指示我们是否可以向 api/data 发出请求，以获取接受“ **id** ”作为参数的数据。

**因此，在这种情况下，我们向 api/status 端点发出连续请求，以检查状态。**

让我们看看如何使用和不使用 retry decorator 来实现这一点。

**无重试装饰器**

```
def check_status(id):
     status = requests.get(status_url)
     if status == True:
          return True
     else:
         return Falsedef get_data(id):

     status = None
     while status is None or status == False:
           time.sleep(5)
           status = check_status(id) response = requests.get(url)
     return response;
```

在上面的伪代码中，我在 while 循环中调用“check_status”方法，对于每个请求，我都将其延迟 5 秒钟，它必须等到 status 为“True”，只有这样我们才能使用“API/data”
端点获取数据。

因此，这是一种重试模式，我们通过延迟 5 秒钟来重复检查 id 的状态。

同样可以使用“重试装饰器”来实现。

**带重试装饰器**

要安装重试装饰器，请使用以下命令。

```
pip install retry
```

若要在代码中使用它，请在代码中包含以下语句。

```
from retry import retry
```

要使任何函数成为重试逻辑，只需在函数定义上方添加@retry()语句，如下所示。

```
@retry()
def check_status:
```

重试装饰器接受一些参数，

```
**exceptions** - you can specify your custom exception class , which indicates that you are expecting the function to retry when specified exception occurs.**tries** - specifies the maximum number of times function can be retried. Default value is "-1".**delay** - specifies the amount of time the function waits before next retry , similar to time.sleep() we have used above. Default value is "0".
```

以上是 retry decorator 接受的一些参数，更多信息请参考[文档](https://pypi.org/project/retry/)。

**实施:**

我没有使用 time.sleep(5)，而是使用了上面声明的 retry decorator "**wait _ for _ success _ status**"函数，该函数带有 3 个参数**异常类型**，它指示在特定类型的异常上函数必须重试，**delay**5 秒，这将函数执行延迟 5 秒，**用值 6 尝试**，这指示函数最多可以重试 6 次。

我们也可以添加多个异常类型。

```
@retry((ValueError,TypeError,InvalidStatus), delay=5, tries=6)
```

在这种情况下，如果出现任何一种异常，将会重试该功能。