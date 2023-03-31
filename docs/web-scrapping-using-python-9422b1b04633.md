# 使用 Python 进行 Web 抓取

> 原文：<https://medium.com/analytics-vidhya/web-scrapping-using-python-9422b1b04633?source=collection_archive---------13----------------------->

我们从网站上提取信息，方法是将信息复制并粘贴到我们的文件中。然而，如果我们想尽快从网站上获取大量信息，这种手动提取数据的过程可能会很麻烦。在这种情况下，网络抓取会有所帮助，因为它是一种使用代码或 API 从网站下载大量数据的方法。

Python 是最流行的网络抓取语言，因为它有 Scrapy、Beautiful Soup 和 Selenium 等库，这些库使得抓取网站变得轻而易举。Web 抓取包括检查网页，找到与我们需要的信息相关的合适的 HTML 标记，使用熊猫库(如 Beautiful Soup 或 Selenium)来抓取 HTML 页面。在这一步之后，我们使用 pandas 库以我们需要的形式操作抓取的数据。

这个任务我们需要的包是请求，熊猫，和漂亮的汤。

*   请求包允许我们连接到我们选择的站点。在本例中，我们希望连接到 IMDB top 1000 movies 网页。
*   Beautifulsoup4 允许我们解析站点的 HTML 并将其转换为一个漂亮的 soup 对象，将 HTML 表示为一个嵌套的数据结构。
*   Pandas 包允许数据集操作。

下面的代码可以帮助理解这个概念。

```
import requests
from bs4 import BeautifulSoup
import pandas as pd
url = "[https://www.imdb.com/search/title/?count=100&groups=top_1000&sort=user_rating](https://www.imdb.com/search/title/?count=100&groups=top_1000&sort=user_rating)"
# makes a request to the web page and gets its HTML
r = requests.get(url)
# stores the HTML page in 'soup', a BeautifulSoup object
soup = BeautifulSoup(r.content,'lxml')
```

我们希望从 IMDB 网页中提取电影标题、电影发行年份、IMDB 评级、运行时间和类型。要检查包含我们需要的信息的所需标签，右键单击浏览器并选择 **Inspect** 。

使用下面的代码，我们可以下载所需的信息。

```
#Extracting text: Movie titles and release year
Titleandreleaseyear= soup.findAll('h3', {"class":'lister-item-header'})
titles = [movie.find('a').text for movie in Titleandreleaseyear]
release = [movie.find('span', class_='lister-item-year text-muted unbold').text for movie in Titleandreleaseyear]#Extracting audience rating
Rating = soup.findAll('div', {"class":'inline-block ratings-imdb-rating'})
imdbrating = [i.find('strong').text for i in Rating]# Extracting Runtime
Runtime = soup.findAll('span', {"class":'runtime'})
a = len(Runtime)
Movieruntime = []
for i in range(a):
    Movieruntime.append((Runtime[i]).text)# Extracting Genre
Genre = soup.findAll('span', {"class":'genre'})
a = len(Genre)
Genres = []
for i in range(a):
    Genres.append(((Genre[i]).text).replace('\n', '').strip())
```

从 HTML 中提取的数据是非结构化格式的；因此，需要使用下面的代码将其转换为结构化形式。

```
#pandas dataframe        
moviesdata = pd.DataFrame({
'movie': titles,
'year': release,
'Runtime': Runtime,
'imdb': imdbrating,
'genre': Genres})
#Create csv file named 'movies.csv'
moviesdata.to_csv('movies.csv')
```

在本文中，我们探讨了 web 抓取的基础知识，并且对这个概念有了更高层次的理解。点击💚如果你喜欢这篇文章。有问题可以写在下面的评论区，我会尽力解答。