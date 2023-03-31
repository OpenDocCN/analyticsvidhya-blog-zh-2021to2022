# 在 Django 中模仿外部 API

> 原文：<https://medium.com/analytics-vidhya/mocking-external-apis-in-django-4a2b1c9e3025?source=collection_archive---------5----------------------->

![](img/d550e5580745b70202a919fac7a22178.png)

照片由[凯尔·格伦](https://unsplash.com/@kylejglenn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/does-not-exist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*您阅读的大多数文章和帖子都建议在运行您的测试时，使用* `*unitest*` *库中的* `*Mock*` *和* `*patch*` *特性来模拟 API 调用。但是，如果您试图在本地机器或任何其他受控环境中手动测试流，该怎么办呢？*

在为 web 应用程序构建后端服务时，经常会遇到必须与外部服务集成并使用其 API 的情况。

# 设置场景

考虑一个案例，我们正在构建一个电子商务应用程序。我们的应用程序旨在处理市场，我们必须与联邦快递等第三方服务集成，以处理运输和物流。

假设联邦快递服务为我们提供了 2 个 API

1.  创建发运 API:创建新的发运订单。
2.  发运状态 API:检查发运的状态。

出于本文的考虑，我将假设我们使用适配器模式来构建与第三方服务的接口。因此，我们最终得到了一个服务类，它有两个方法，如下所示，这两个方法调用了联邦快递服务的实际 API。

```
class FedexService:
    """Service class for interfacing with FedEx Service.""" def create_shipment(self, shipment_details: dict):
        """Create the shipment."""
        # Call the API to create shipment

    def check_status(self, shipment_id: str):
        """Check the status of the shipment by the id."""
        # Call the API to check the status
```

然而，出于测试的目的，我们希望避免调用 FedEx 服务 API，而是调用另一个实现相同方法的服务，但会返回一个预先确定的/受控的响应。

我们可以实现如下所示的模拟服务

```
class MockShippingService:
    """Mock service class for shipping.""" def create_shipment(self, shipment_details: dict):
        """Create the shipment."""
        return {
            'shipping_id': str(uuid.uuid4()),
            'created_at': datetime.datetime.isoformat(),
            'charges': '100.10'
        }

    def check_status(self, shipment_id: str):
        """Check the status of the shipment by the id."""
        return {
            'status': 'delivered',
            'updated_at': datetime.now().isoformat()
        }
```

# 无缝切换的诀窍

一旦我们实现了实际的和模拟的服务类，剩下的唯一事情就是在需要时在这些类之间进行切换。其诀窍在于实现一个多路复用器:

```
@apiplexer
class ShippingService:

    api_settings = 'SHIPPING_SERVICE'
```

这个类充当接口，应用程序的其余部分使用它来调用运输服务。例如:

```
def create_shipment(self, order_details: dict):
    """Create the shipment for the order."""
    # Do something to create the shipment_details
    shipment = ShippingService().create_shipment(shipment_details)
    # Do something to handle the shipment
```

`apiplexer`装饰器是在服务类之间切换的关键。装饰器可以实现为:

```
from django.conf import settings
from django.utils.module_loading import import_stringdef apiplexer(cls):
    """Load the classes based on their names."""
    return import_string(getattr(settings, cls.api_settings))
```

那么实际的切换是如何工作的呢？为此，我们在设置文件中指定要使用的类。

在`settings/local.py`我们插入

```
SHIPPING_SERVICE = 'shipping.service.MockShippingService'
```

在`settings/production.py`中，我们添加了

```
SHIPPING_SERVICE = 'shipping.service.FedexService'
```

当加载 Django 应用程序时，将根据该环境设置中提供的`SHIPPING_SERVICE`值导入要使用的适当运输服务类。

# 随你便吧

根据传递给它的方法的值，`MockShippingService`可以进一步扩展以处理许多不同的情况。它可以返回带前缀的值；或者从数据库中获取和返回值，或者甚至调用一个模拟 API 服务器。

使用这种方法的目的是有一种与第三方服务交互的方式，而不必实际与它通信。这使得在本地或分段环境中进行手动测试变得更加容易，只需要最少的外部依赖和资源。

开心嘲讽！

*感谢您的阅读。请在下面留下您的评论，告诉我们您是如何发现这篇文章有用的，或者您已经/将要如何以不同的方式或更好的方式实现这篇文章。干杯！*