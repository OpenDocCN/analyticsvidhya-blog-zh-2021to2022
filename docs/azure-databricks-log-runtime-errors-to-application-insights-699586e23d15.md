# Azure 数据块将运行时错误记录到应用洞察

> 原文：<https://medium.com/analytics-vidhya/azure-databricks-log-runtime-errors-to-application-insights-699586e23d15?source=collection_archive---------4----------------------->

# 使用 open census 库将错误日志推送到 Azure monitor

# 先决条件

*   Azure 帐户
*   Azure 数据块
*   Azure 存储

# 步伐

*   创建数据块集群
*   安装库

```
opencensus-ext-azure
```

![](img/d6824a39b648b11cb9fc38328abbe7c8.png)![](img/7ef4af6b8d40d9f8533508378439fbe1.png)

*   创建笔记本

# 密码

*   选择 python 作为语言

```
import logging
from opencensus.ext.azure.log_exporter import AzureLogHandlerlogger = logging.getLogger(__name__)# TODO: replace the all-zero GUID with your instrumentation key.
logger.addHandler(AzureLogHandler(
    connection_string='InstrumentationKey=xxxxx-xxxxxx-xxxxxx-xxxxxxx')
)
```

*   现在记录一些示例日志

```
logger.warning("Sample from open census test 01")
logger.error("Sample from open census test 02")
```

*   现在让我们记录一个异常

```
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracerproperties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}# Use properties in exception logs
try:
    result = 1 / 0  # generate a ZeroDivisionError
except Exception:
    logger.exception('Captured an exception.', extra=properties)
```

*   登录应用洞察并转到日志

![](img/1d8abd4b9834d0a4951f29c2fe4854df.png)![](img/5211eaab39ed8d91e25b88d40de98c3e.png)

*   以上测试完成自—[https://docs . Microsoft . com/en-us/azure/azure-monitor/app/open census-python # configure-azure-monitor-exporters](https://docs.microsoft.com/en-us/azure/azure-monitor/app/opencensus-python#configure-azure-monitor-exporters)

*最初发表于*[T5【https://github.com】](https://github.com/balakreshnan/Samples2021/blob/main/adb/opencensusapplog.md)*。*