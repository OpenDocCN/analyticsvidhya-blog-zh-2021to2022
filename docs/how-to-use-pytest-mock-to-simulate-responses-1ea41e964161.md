# 如何使用 pytest-mock 模拟响应

> 原文：<https://medium.com/analytics-vidhya/how-to-use-pytest-mock-to-simulate-responses-1ea41e964161?source=collection_archive---------2----------------------->

![](img/c15cb5c25165b972d3b7922345582e62.png)

Alexandre Debiève 在 [Unsplash](https://unsplash.com/photos/FO7JIlwjOtU?utm_source=unsplash&utm_medium=referral&utm_content=view-photo-on-unsplash&utm_campaign=unsplash-ios) 上的照片

电气、计算机或设计工程师面临的最大挑战之一是弥合硬件和软件之间的差距。编写软件来运行硬件的任务一直被证明是一项具有挑战性的任务。这主要是因为专注于学习语言语法，而没有足够的时间花在学习调试和软件测试上。当被要求证明软件在运行硬件时没有带来问题时，差距进一步扩大。这通常会导致数小时的长时间全系统测试，以确保可重复性，当涉及硬件时，由于响应缓慢，这不是一个可扩展的解决方案。对于 Python 开发者来说，解决方案是使用 [pytest](https://pypi.org/project/pytest/) 和 [pytest-mock](https://pypi.org/project/pytest-mock/) 插件来编写测试代码的单元测试，以模拟硬件响应。

注意:虽然这篇文章是为硬件工程师写的，但是这个概念也可以扩展到任何外部进程或系统，比如数据库。

# 驱动程序代码

```
from time
import sleep
from fabric import Connectionclass Driver:
    def __init__(self, hostname):
        self.connection = Connection(hostname)def run(self, command):
        sleep(5)
        return self.connection.run(command, hide=True).stdout.strip()def disk_free(self):
        return self.run("df -h")@staticmethod
    def extract_percent(output):
        free_line = output.split("\n")[1]
        percent = free_line.split()[4] return percent
```

上面的驱动程序是一个简单的例子。它使用一个名为 [Fabric](https://pypi.org/project/fabric/) 的库来建立 SSH 连接。有两种方法:

1.  **run()** —允许在目标上发出任何通用命令，并返回原始输出。在输出返回之前，会插入一个 5 秒的延迟来模拟缓慢的响应。
2.  **disk_free()** —生成命令“df -h”，然后使用生成的命令调用 **run()** 方法
3.  **extract_percent()** —解析原始输出并返回磁盘可用百分比

# 以集成方式测试代码

```
import socket
from mock_tutorial.driver import Driverdef test_driver_integrated():
    d = Driver(socket.gethostname())
    result = d.disk_free()
    percent = d.extract_percent(result)
    assert percent == "75%"
```

上述测试代码通常以黑盒测试方式编写，用于测试该驱动程序。首先实例化驱动对象，调用 **disk_free()** 函数，解析输出，最后与预期结果进行比较。

```
$ pytest -s
======================== test session starts =======================
platform linux -- Python 3.9.4, pytest-6.2.3, py-1.10.0, pluggy-0.13.1 rootdir: /home/chinghwa/projects/mock_tutorial
plugins: mock-3.5.1
collected 1 item
tests/test_driver.py .                                        [100%]
========================= 1 passed in 5.51s ========================
```

除了执行慢了 5.51 秒，还有一个问题。“df -h”的输出很可能会随着时间而变化，不会停留在 75%。虽然这个例子是虚构的，但它说明了以另一种方式检查输出的必要性。

# 用 pytest-mock 测试代码以模拟响应

```
import socket
import pytest
from mock_tutorial.driver import Driverdef test_driver_unit(mocker):
    output_list = [
        "Filesystem Size Used Avail Use% Mounted on",
        "rootfs 472G 128G 344G 28% /",
        "none 472G 128G 344G 28% /dev", ]
    output = "\n".join(output_list)
    mock_run = mocker.patch( "mock_tutorial.driver.Driver.run", return_value=output )
    d = Driver(socket.gethostname())
    result = d.disk_free()
    percent = d.extract_percent(result)
    mock_run.assert_called_with("df -h") assert percent == "28%"
```

上面的代码被重写为使用一个模拟对象来修补(替换)run() 方法。首先，我们需要导入 pytest(第 2 行)并从 pytest-mock(第 5 行)调用 mocker fixture。

在第 12–14 行，**驱动程序**的 **run()** 方法被打了补丁，用预编程的响应来模拟实际的响应。这意味着任何对 **run()** 的调用都将返回**输出**的字符串值。

第 18 行将检查发送给 **run()** 方法的命令。当调用 **disk_free()** 方法时，这将生成一个“df -h”命令，并用这个命令调用 **run()** 。

第 19 行将检查从**输出**中提取百分比的解析函数。如果第 8 行中的 Use%被更改，这将失败，因为这是正在提取的值。

```
$ pytest -s
======================== test session starts =======================
platform linux -- Python 3.9.4, pytest-6.2.3, py-1.10.0, pluggy-0.13.1
rootdir: /home/chinghwa/projects/mock_tutorial
plugins: mock-3.5.1
collected 1 item
tests/test_driver.py .                                        [100%]
========================= 1 passed in 0.36s ========================
```

添加模拟对象后，测试时间减少到 0.36 秒。缓慢的 **run()** 方法被修补以更快地执行，并且还检查了解析模拟输出的代码。

# 摘要

通过用模拟对象修补缓慢的响应，这应该为用 Python 开发快速执行的单元测试提供了一个良好的起点。除了单元测试之外，还应该编写集成测试，尽管它们可以不那么频繁地执行。

这篇文章是我用 Python 写的第一篇关于这个主题的文章，我希望将来能深入研究其他 pytest-mock 方法。

# 资源

*   本文代码—[https://github.com/chinghwayu/mock_tutorial](https://github.com/chinghwayu/mock_tutorial)
*   pytest 夹具—[https://docs.pytest.org/en/stable/fixture.html](https://docs.pytest.org/en/stable/fixture.html)
*   pytest-mock 方法—[https://github.com/pytest-dev/pytest-mock/](https://github.com/pytest-dev/pytest-mock/)
*   pytest 标记和标记测试为慢—[https://docs.pytest.org/en/stable/example/markers.html](https://docs.pytest.org/en/stable/example/markers.html)

*原载于 2021 年 4 月 21 日 https://chinghwayu.com**[*。*](https://chinghwayu.com/2021/04/how-to-use-pytest-mock-to-simulate-responses/)*