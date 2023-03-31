# 姜戈的信号

> 原文：<https://medium.com/analytics-vidhya/signals-in-django-af99dabeb875?source=collection_archive---------1----------------------->

![](img/5eb6b6f6e1f9214d2352d886ef16049f.png)

在本文中，我们将讨论 Django 中有哪些信号，以及如何在我们的 Django 项目中使用这些信号。那我们开始吧。

在许多情况下，当在模型实例中有一些修改或者创建一个特定的模型实例时，我们可能需要执行一些动作。Django 提供了一种优雅的方式来处理这种情况。信号是让我们把事件和行动联系起来的工具。

首先让我们回顾一下信号的基本概念，

***信号:***

信号是与特定事件相对应的对象。

```
from django.dispatch import Signal make_order = Signal(providing_args=["toppings", "size", "type"])
```

信号可以在特定事件发生时发送信息。这是通过在信号实例上调用`send()`方法来实现的(传入一个`sender`参数，以及上面指定的参数):

```
class Burger:     
    def create_order(self, toppings, size, type):
       make_order.send(sender=self.__class__, toppings=toppings, size=size, type=type)
```

**接收者:**

接收器是连接到每个信号的 python 函数。当信号发送消息时，每个连接的接收器都被调用。接收器的功能特征应该与信号的`send()`方法相匹配。您使用`@receiver`装饰器将接收器连接到信号。

```
from django.dispatch import receiver 
from burger import signals @receiver(signals.pizza_done) 
def notify-when_order_created(sender, toppings, size, **kwargs):
     # Create an order
```

以上是信号调度员的基本知识。接收器连接到信号并接收由信号发送消息，并进一步执行

我将向您展示信号最常见的用例，即在创建用户实例时创建一个概要文件实例。

最常用的信号是:-

> ***pre _ save/post _ save****:—模型的* `[***save()***](https://docs.djangoproject.com/en/3.1/ref/models/instances/#django.db.models.Model.save)` *方法被调用之前或之后发送。*
> 
> ***pre _ delete/post _ delete****:—在调用模型的* `[***delete()***](https://docs.djangoproject.com/en/3.1/ref/models/instances/#django.db.models.Model.delete)` *方法或 queryset 的* `[***delete()***](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.delete)` *方法之前或之后发送。*

解释:

首先我们导入了 post_save 和 receiver，然后我们创建了一个接收函数 create_user_profile，它将接收 post_save 信号。我们使用 receiver decorator 在创建用户实例时调用 create_user_profile 函数。

因此，我们接受 create_user_profile 函数上的 sender、instance、created(这将是一个布尔值，表示用户实例是否已创建)参数，

并且根据创建的真值，我们为用户创建配置文件对象。

上述是实现信号的一种方式，你也可以在不同的文件中使用接收器功能的代码(注意:你必须在应用程序的 app.py ready 方法中正确导入该文件，否则它将无法工作。

信号非常方便，你可以看到信号是同步的，这意味着正常的程序执行流程在继续发送信号的代码之前依次运行每个接收器。你可以在 Django 文档中阅读更多关于信号的信息。

信号调度机制并不是 Django 所特有的，它实际上是一种众所周知的设计模式:[**观察者模式**](https://en.wikipedia.org/wiki/Observer_pattern) 。唯一的区别是在术语上:在观察者模式中，信号被称为“主体”，接收器被称为“观察者”。

如果你在实现信号时遇到任何困难，请在评论中告诉我。

编码快乐！！！