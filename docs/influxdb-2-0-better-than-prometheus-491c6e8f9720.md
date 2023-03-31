# InfluxDB 2.0:比普罗米修斯好？

> 原文：<https://medium.com/analytics-vidhya/influxdb-2-0-better-than-prometheus-491c6e8f9720?source=collection_archive---------1----------------------->

![](img/6d9162858acb7e025d25d4ba903aca35.png)

上周末，我终于把我的 InfluxDB 数据库更新到了 2.0 版本。不幸的是，我丢失了所有的数据，但我对提供的新功能感到惊喜。我们先来看看它的主要新特性，并和监控之王:普罗米修斯进行对比。

# 流动的语言

InfluxDB 的两个主要版本之间最显著的区别是查询语言。在第一个版本中，通过一些 SQL 概念，InfluxQL 看起来非常简单。然而，在几次使用之后，很容易看出从一种用于关系数据库的语言到一个时间序列数据库的简单转换的局限性。intrusion 在 2019 年通过创建他们的过程化查询语言来应对这个问题，这种语言现在是版本 2: Flux 中的默认语言。

> 我不想生活在一个人类能想到的最好的数据处理语言是在 70 年代发明的世界里
> 
> [*为什么我们要建立 Flux，一种新的数据脚本和查询语言*](https://www.influxdata.com/blog/why-were-building-flux-a-new-data-scripting-and-query-language/)

最初的几个小时很难翻译我的旧查询，但最终，学习进展得相当顺利。文档中有完整的示例，语法也变得非常自然。以转换简单查询为例:

![](img/893ac33cd48bf9858080ff3554782919.png)

InfluxQL vs Flux。来源:[https://www . sqlpac . com/fr/documents/influx db-v2-prise-en-main-installation-preparation-migration-version-1.7 . html](https://www.sqlpac.com/fr/documents/influxdb-v2-prise-en-main-installation-preparation-migration-version-1.7.html)

正如我们在上面看到的，Flux 更加冗长，但是它的语法受 JavaScript 的启发，更容易理解数据是如何处理的。

# 流入界面中的 Chronograf

我第一次登录到 Influx 提供的新网络界面时，我惊讶地看到了一个“仪表盘”和“提醒”菜单。我抓住机会测试这些以前在 Chronograf 中的功能。创建仪表板与 Grafana 不同，但它包含了主要的功能。有几种类型的单元格可供选择:

*   图表
*   图表+单一统计
*   热图
*   柱状图
*   单一统计
*   测量
*   桌子
*   分散

存在一个集成的查询构建器，但是一旦需求变得更加关键(使用变量、几个字段的查询等)，它很快就显示出它的局限性。).

您还可以定义警报(基于异常值或当服务不再返回数据时),将它们发送到 Pagerduty、Slack 或 HTTP 服务器。甚至有一个基于批评的路由系统，很像 Alertmanager。

# InfluxDB 能打败普罗米修斯吗？

从一开始，这两个工具对于相同的用途就有完全不同的看法。普罗米修斯将 HTTP 中的数据抓取到它的导出器中。Influx 从 Telegraf 或客户图书馆接收多种语言的数据。第一种方法更适合微服务环境，但是设置起来更复杂。

但是一个简单的事实是，influence 固有地集成了 Grafana 特性和 Alertmanager 来重新洗牌。单个工具相对于三个工具的安装、配置和管理允许较小的基础架构有一个集中的监控、查看和警报解决方案。

就查询语言而言，Flux 比 PromQL 提供了更多的可能性来获得同样高水平的性能。与其竞争对手的简单性相比，它的冗长可能令人害怕，但是一旦掌握了语法，编写查询将是一种真正的乐趣！我认为 Flux 的强大功能使 InfluxDB 有可能用在监管以外的环境中，在这些环境中，时间序列数据库是相关的(监视股票市场价格、气象调查等)。)

我将在以后的文章中向您展示使用 Telegraf + InfluxDB 2.0 夫妇的完整监控和日志收集解决方案，敬请关注！