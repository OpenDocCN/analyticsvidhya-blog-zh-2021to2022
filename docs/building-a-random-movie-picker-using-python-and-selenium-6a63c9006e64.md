# 使用 Python 和 Selenium 构建随机电影选择器

> 原文：<https://medium.com/analytics-vidhya/building-a-random-movie-picker-using-python-and-selenium-6a63c9006e64?source=collection_archive---------13----------------------->

![](img/ee055a4f4a5910837c81ed7d18544d76.png)

我妻子(丹妮)每周都在评论电影，并把它们上传到她的 Youtube 频道。她最近问我，是否有可能搜集这些导演的名单，并随机返回一部他们执导的电影，以帮助她决定下一部要评论的电影。

听起来像是某个*好-ol-web-* [*硒*](https://selenium-python.readthedocs.io/) 的工作。

## 我选择如何解决这个问题。

> 抓取网站可能很挑剔。你是在内容创作者的心血来潮的降价决策。

[Elacervo](https://www.elacervo.com/directores) 的标记比较棘手。他们主任的页面是一致的，但是个别主任的帖子却不是。有些 director 页面的电影列表有单独的`p`标签，而有些页面的整个电影列表被格式化在一个`span`元素中。**这是个问题**。

我没有试图拼凑出一种方法来获得大多数导演的视频，而是选择获得导演的名单，并从可靠的来源收集他们的电影名单。

我选择了 IMDB，它的 API [IMDbPY](https://imdbpy.github.io/) 周围有一个方便的 python 包装器。

## 为什么我选择 Selenium 作为工具呢？

Dany 是一个初学 web 开发的人，对 Python 很好奇。Selenium 为开发人员提供了自动化浏览器交互的可视化确认。生成一个新的浏览器实例并点击站点确实会影响性能，但是我相信 Selenium 在视觉方面的优势超过了性能问题。

像 [Scrapy](https://scrapy.org/) 这样的框架可以更快地提供数据，但是我做这个的一个重要原因是帮助丹妮学习 Python。

## 设置 Selenium 以自动化我的浏览器

Selenium 需要进行一些设置，以便开始自动化您的浏览器。

使用 Python 3 的内置包管理器 [pip](https://pypi.org/project/pip/) ，用命令`pip install selenium`下载 Selenium。

> 我强烈推荐利用一个 [virtualenv](https://virtualenv.pypa.io/en/latest/) 并创建一个隔离的 Python 环境。

您还需要下载适当的网络驱动程序。Selenium 的[文档](https://selenium-python.readthedocs.io/installation.html#drivers)有最流行的浏览器驱动的链接。对于本教程，我将使用谷歌浏览器和 Chrome 驱动程序。

## 使用 Selenium 和 Python 收集控制器

下面是我决定使用的代码片段。我附加了 numbers 注释来描述代码片段中的重要选择。

```
import json
from selenium import webdriver

driver = webdriver.Chrome('chromedriver') // 1
driver.get('https://www.elacervo.com/directores') // 2

// 3
for i in range(4):
  time.sleep(5)
  driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

// 4
directors = driver.find_elements_by_css_selector(
  "a[href*='https://www.elacervo.com/post/']"
  )

// 5
unique_directors = []
for link in directors:
  if (link.get_attribute("href")) not in unique_directors:
    unique_directors.append(link.get_attribute("href"))

// 6
names = []
for link in unique_directors:
  slug = link.split('/')[-1]
  name = slug.replace('-', ' ').title()
  names.append({"name": name})

// 7
with open('directors.json', 'w') as outfile:
  json.dump(names, outfile)

// 8
driver.quit()
```

1.  这是我们告诉 Selenium 使用 spawn 一个新的 Google Chrome 实例的地方。我们传递给`.Chrome()`方法的值`chromedriver`是我们在上一步下载的 chromedriver 文件的位置。
2.  `driver.get('https://www.elacervo.com/directores')` -这里我们告诉我们现在制作的 Selenium 驱动程序导航到 URL `[https://www.elacervo.com/directores](https://www.elacervo.com/directores.)` [。](https://www.elacervo.com/directores.)
3.  我抓取的网站有一些惰性加载逻辑，只有一定数量的控制器被加载，直到页面滚动到底部。这是执行一些客户端 javascript 以滚动到页面底部，等待几秒钟以加载新的控制器，然后滚动到页面的新底部。
4.  在这里，我收集了所有包含带有标签的`a`和包含`https://www.elacervo.com/post/`的`href`的 html 事件。这是使用包含通配符`*`的逻辑`href*=`。
5.  这是提取位于`href`源中的 directors URL。然后它将 URL 放入一个`unique_directors`列表中。这个页面上的一些导演有他们的链接两次，所以我删除任何重复的网址。
6.  我正在清理 URL 链接，以便简单地从它们那里获取导演的名字。聚集的链接看起来像`https://www.elacervo.com/post/martin-scorsese`。这里的逻辑是取最后一个`/`字符之后的所有内容，用空格替换`-`，然后将名称中每个单词的第一个字母大写。
7.  然后，我使用`json.dump`将收集到的控制器名称写入到一个`json`文件中，以便以后更快地使用。从一个`json`文件中读取要比生成一个浏览器点击并提取数据快得多。
8.  `driver.quit()` -这将关闭一个 Selenium Chrome 实例。

## 使用 IMDbPY 查询 IMDBs 数据库

IMDbPY 是一个非常简洁的用于查询 IMDB 数据的包。只需几行 Python 代码，我们就可以看到一个目录的整个电影画面。

## 这个 Python 片段将返回每个导演的每部电影

```
import json
from imdb import IMDb

file = open('directors.json',)
directors = json.load(file)

movies = []
ia = IMDb()
for person in directors:
  try:
    director = ia.search_person(person['name'])[0]
    try:
      films = ia.get_person_filmography(director.personID)['data']['filmography']['director']
      for film in films:
        if film['kind'] == 'movie':
          try:
            if (film['year']):
              movies.append(film)
          except KeyError:
            continue
    except AttributeError:
      continue
  except IndexError:
    continue

  with open('movies.json', 'w') as outfile:
    json.dump([{"title": movie['title'], 'year': movie['year']} for movie in movies], outfile)
```

我们使用 Python 内置的 [open](https://docs.python.org/3/library/functions.html#open) 函数来打开我们在 Selenium 部分创建的`directors.json`文件。然后使用 Python 的 [JSON 解码器](https://docs.python.org/3/library/json.html)，我们可以将文件中的数据加载成可用的 JSON 格式。

初始化一个 IMDb 对象使我们能够访问软件包的功能，允许我们查询 IMDb 的数据库。

方法`.search_person(person['name'])`返回 IMDb 数据库中的人员列表。似乎返回列表中的第一个结果是最受欢迎的，也是`[0]`后面的推理。对于这个项目，我假设他是我想合作的导演。

## 区分电影和其他电影类型

IMDbPY `Movie`对象的属性可以在这里看到[。对于这个项目，我只对电影感兴趣，所以我应用了一个条件来检查，将接受的电影附加到电影列表中。](https://imdbpy.readthedocs.io/en/latest/usage/movie.html)

原来 IMDbPY 的 Movie 对象只有在电影已经上映的情况下才有属性`year`，否则就有属性`status`。我只想要现在可以看的电影，并相应地过滤掉数据。

```
movies = []
if film['kind'] == 'movie':
    try:
        if (film['year']):
            movies.append(film)
    except KeyError:
        continue
```

## 将电影数据写入可重用的 JSON 文件

与使用 Selenium 提取的数据一样，我决定通过将获得的数据写入可重用的 JSON 文件来减少对 IMDb 的 API 请求的数量。

我决定只取电影的标题和年份值，而不是提取 IMDb 电影对象的所有数据。将来，提取额外的数据来进行高级过滤可能会很酷。例如，想要仅观看 1970 年和 1980 年之间的分级大于 9.0 的电影。虽然 IMDb 电影对象说它有某些属性，但它最终有点挑剔，我决定暂时不要它。

```
with open('movies.json', 'w') as outfile:
    json.dump([{"title": movie['title'], 'year': movie['year']} for movie in movies], outfile)
```

## 从我们的列表中随机选择电影

现在我们在 json 文件中有了我们导演组的所有电影的列表，我可以使用 Python 的 [random.choice](https://docs.python.org/3/library/random.html#random.choice) 来随机选择一部电影。

```
import random
import json

file = open('movies.json')
data = json.load(file)
print(random.choice(data))
```

## 随机看电影很好玩！

**认真地**试一试。至少，随机选择一部电影，看它的预告片。这些电影中有许多我从未听说过，但它们很吸引人，富有创造性和艺术性。

我希望这篇文章是有帮助的！