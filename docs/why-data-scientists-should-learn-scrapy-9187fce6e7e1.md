# 为什么数据科学家应该学习 Scrapy

> 原文：<https://medium.com/analytics-vidhya/why-data-scientists-should-learn-scrapy-9187fce6e7e1?source=collection_archive---------29----------------------->

## 高级刮擦解决方案

没有什么比刮网更让我热血沸腾了。想想有多少数据在外面到处流动——没有相关的 API，没有官方的下载链接。难道你不想自己得到那些数据吗，就像你的狩猎采集祖先自己得到野兽一样？

![](img/773763d715cf3729114c72fc3ec45a68.png)

你的祖先在吃某种犰狳。

如果你是一个对网络抓取感兴趣的充满活力的数据科学家，你应该学习 [Scrapy](https://scrapy.org/) 。Scrapy 是 Python 唯一最强大的抓取工具——它对于请求库就像熊猫对于 Python 字典一样。它快速、灵活、功能丰富，并且有据可查。毫无疑问，Scrapy 是 Python 的首选抓取包。

![](img/76126e76916e0aceda4d3f8a5a06fe83.png)

# 什么是 Scrapy？

Scrapy 是一个用于开发蜘蛛的**高级**应用框架。蜘蛛是一种程序，它战略性地导航一个网站(或多个网站)并提取结构化数据。想象一下，如果你可以跳过开发网络抓取应用程序的底层管道，只关注你的抓取任务的细节——这就是 Scrapy 提供的。当 Scrapy 做得更好时，为什么要开发自己的蜘蛛框架、请求调度器、复制过滤器、下载器和导出机制？

## 有什么好的？

Scrapy 构建在 [Twisted](https://twistedmatrix.com/trac/) 异步网络框架之上，是 Python 可用的最快的抓取工具。它使用[并发](https://en.wikipedia.org/wiki/Concurrency_(computer_science))向服务器发送大量请求，并以极快的速度下载网页。听起来很刺耳？我建议你像个男人一样。我没有时间去尊重服务器，你也一样。

![](img/1a598cb5bf8070105a60a589ca930ede.png)

愤怒的服务器。

开玩笑的。你应该总是小心翼翼，这通常意味着配置下载延迟和并发设置。您可以将以下常量添加到`settings.py`:

```
# seconds of delay
DOWNLOAD_DELAY = 0.25# max simultaneous requests per IP
CONCURRENT_REQUESTS_PER_IP = 8
```

或者，您可以启用 [AutoThrottle](https://docs.scrapy.org/en/latest/topics/autothrottle.html) 扩展，它可以根据服务器延迟和您的目标并发设置动态调整下载延迟。配置节流将防止您被禁止，也防止您惹恼运行您的目标网站的人。

![](img/800e7afa5f008de696b4409ce0843420.png)

谢谢你的服务。

正如我已经提到的，Scrapy 非常灵活，并且装载了许多有用的特性。它附带了各种用于不同常见用例的 spider 类，以及用于下载图像和其他文件的基础设施。你可以使用你最喜欢的 HTML 数据提取工具，比如 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 和 [lxml](https://lxml.de/) 。您还可以创建自己的数据处理管道来帮助清理数据。您甚至可以将蜘蛛部署到云中，并设置它们定期运行。想象一下，你可以利用云端蜘蛛多年的数据做些什么。

我想强调的最后一点是，Scrapy 是有据可查的。你将能够学习如何做任何你需要做的事情。文档不仅仅是一个 API 参考，还包括了 Scrapy 设计中每个概念的详细指南。我建议从[教程](https://docs.scrapy.org/en/latest/intro/tutorial.html)开始，然后边学边做。

## 你什么时候不会用 Scrapy？

Scrapy 最适合需要在多个页面上爬行的重型抓取任务，可能是定期的。如果你想从一个表，一个页面，一次检索数据，你不需要 Scrapy。可以用[请求](https://docs.python-requests.org/en/master/)，甚至熊猫的惊人强大 [pd.read_html](https://pandas.pydata.org/docs/reference/api/pandas.read_html.html) 。

或者，如果您需要抓取大量使用动态加载内容的网站，而这些内容无法从其 HTML 源访问，您可能希望使用 [Selenium](https://selenium-python.readthedocs.io/index.html) 。Selenium 是为测试 web 应用程序而设计的跨平台浏览器自动化工具。然而，这是一台笨重的机器，(也就是说，速度慢，有很大的依赖性)，除非万不得已，否则我不推荐使用它。Selenium 是一个底层的刮擦解决方案，它没有太多刮擦基础设施。另外，事实证明，没有 Selenium 也可以访问动态加载的内容。参见[本页](https://docs.scrapy.org/en/latest/topics/dynamic-content.html)关于处理动态加载内容的零碎文档。

# 推荐功能

虽然[教程](https://docs.scrapy.org/en/latest/intro/tutorial.html)超出了本文的范围，但我还是想强调几个我最喜欢的 spider 初学者特性。

## (被低估的)站点地图蜘蛛

当开发一个蜘蛛时，你要么继承基本的`Spider` 类，要么继承 Scrapy 的一个专门的子类。Scrapy 提供了在不同情况下有用的各种`Spider`子类。我最喜欢的子类是`SitemapSpider`，尽管开发人员忽略了在 Scrapy CLI 中包含它的模板。

然而，当我想抓取一个网站时，我做的第一件事就是检查它是否有一个[站点地图](https://www.sitemaps.org/index.html)。网站地图本质上只是一个包含链接和元数据的 XML 文件，旨在帮助蜘蛛导航网站。如果我想从一个有成千上万个产品页面的电子商务商店中抓取产品，从网站地图中跟踪链接几乎总是比从页面中提取链接更可取。你从所有你需要的链接开始，这个列表是相对干净的。另外，网站地图很常见。为什么不利用它们，在轻松模式下爬行呢？

为了了解在 Scrapy 中创建一个蜘蛛有多简单，请看下面来自[文档](https://docs.scrapy.org/en/latest/topics/spiders.html)的例子。

```
**from** **scrapy.spiders** **import** SitemapSpider

**class** **MySpider**(SitemapSpider):
    sitemap_urls = ['http://www.example.com/sitemap.xml']
    sitemap_rules = [
        ('/product/', 'parse_product'),
        ('/category/', 'parse_category'),
    ]

    **def** parse_product(self, response):
        **pass** *# your HTML parsing code here*

    **def** parse_category(self, response):
        **pass** *# your HTML parsing code here*
```

在`MySpider` 体中定义的类变量有特殊的意义。蜘蛛将从从`sitemap_urls` 获取网站地图并解析它们以提取网站页面的 URL 开始。它发送来自匹配`'/product/'`到`parse_product` *、*的 URL 的响应以及来自匹配`'/category/'`到`parse_category`的 URL 的响应。您只需为每种页面类型编写 HTML 数据提取代码。这几乎和使用 BeautifulSoup 请求一样简单。

## HTTP 缓存

当开发一个新的蜘蛛时，您会想要反复测试它，以确保它行为正确并提取所需的数据。这对于目标网站来说可能非常烦人。将以下常量添加到您的`settings.py`将启用无过期缓存，因此在测试您的蜘蛛时，您将永远不必发送一个特定的请求超过一次。

```
HTTPCACHE_ENABLED = True
HTTPCACHE_EXPIRATION_SECS = 0
HTTPCACHE_DIR = 'httpcache'
```

这种方法的一个警告是，你可能会在硬盘上留下一个非常大的文件夹。非常大的文件夹需要很长时间才能删除，这很烦人。为了缓解这个问题，我启用了 gzip 压缩。

```
HTTPCACHE_GZIP = True
```

## 图像管道

如果你想抓取图像，你需要使用内置的[图像管道](https://docs.scrapy.org/en/latest/topics/media-pipeline.html)。您可以通过向`settings.py`添加以下内容来启用它:

```
ITEM_PIPELINES = {'scrapy.pipelines.images.ImagesPipeline': 1}
```

由于您刚刚开始，您会希望将图像存储在您的硬盘上(而不是远程服务器上)。为此，添加以下内容:

```
**import** os
IMAGES_STORE = os.path.join('valid', 'directory')
```

现在当你从一个网站上抓取物品时，你可以在`image_urls` 字段中放一列图片网址，Scrapy 会自动下载并存档。

# 结论

没有任何其他 Python 包可以与 Scrapy 竞争作为一个重型刮擦解决方案。所谓的 Scrapy 替代品，Selenium 和 Requests，其实并不在一个档次上。Requests 是一个用于发出 HTTP 请求的库，Selenium 是一个用于浏览器自动化的工具。两者都是底层的抓取工具，需要你自己搭建抓取基础设施。这就像在开始一个分析项目之前试图改造熊猫一样——没有一个头脑正常的人会去尝试。

如果你对 web 抓取感兴趣，你应该学习 Python 最强大的抓取包。你会对它的效率印象深刻。

# 加入

Scrapy 是免费的，[开源](https://github.com/scrapy/scrapy)，由 [Zyte](https://www.zyte.com/) 维护。我与 Zyte 或任何其他赞助公司都没有关系。我写这篇文章是发自内心的，基于我的电子商务网络抓取经验。我花了一段时间才自己发现 Scrapy，我浪费了无数的时间在低级别的替代品上。