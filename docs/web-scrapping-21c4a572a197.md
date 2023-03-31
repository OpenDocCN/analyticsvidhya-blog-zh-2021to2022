# Web 报废

> 原文：<https://medium.com/analytics-vidhya/web-scrapping-21c4a572a197?source=collection_archive---------26----------------------->

## 介绍

Web 报废是用于从网站提取数据的数据报废。这是一个从不同站点或单个站点提取数据的自动化过程。Web 报废包括两个主要方面:

1)抓取:

它下载一个页面，并在需要时从中提取数据。这就像当我们使用浏览器时，它会下载网页。

2)提取:

提取就像从网页中获取数据一样。

Web 报废用于:

1)从网站上删除数据

2)网络和数据挖掘等等

## IMDB 数据报废

让我们把手弄脏吧…

首先，让我们导入我们需要的库

```
import re
import requests
from bs4 import  BeautifulSoup
from collections import Counter
```

re:用于正则表达式
请求:用于发送 HTTP 请求
计数器:用于计数
BeautifulSoup:用于提取数据的主库。

现在，让我们瞄准我们的目标网站。并从网站获取数据。

```
url= "https://www.imdb.com/india/top-rated-indian-                   movies/?sort=ir,desc&mode=simple&page=1"
response = requests.get(url)
soup = BeautifulSoup(response.text,features="html.parser")
```

URL:存储 IMDB 站点的链接
响应:从站点
获取页面 soup:包含该页面的所有内容。以后可以用它来提取数据。

现在，让我们提取数据。

```
crew = [a.attrs.get("title")for a in soup.select('td.titleColumn            a')]
title=[ b.attrs.get('alt')for b in soup.select('td.posterColumn              img') ]
ratings=([float(b.attrs.get('data-value')) for b in                  soup.select('td.posterColumn span[name=ir]')])
rel_year=[]
year = [ soup.find_all('span',class_='secondaryInfo')]
for i in year[0]:
rel_year.append(str(i.text).strip('()'))
```

crew:这里，我们使用综合列表来迭代带有类名 titleColumn 的

| 属性，以提取 crew。 |

Title:在这里，我们使用综合列表来迭代类名为 posterColumn 的

| 属性，以提取标题。 |

Ratings:这里，我们使用 comprehensive list 迭代

| 属性来提取评级。 |

Year:存储中的 year 属性，类名为 secondaryInfo。

Rel_year:只是从 year 中删除'()'

```
movie_detail = {}
cnt=0
movie_list=[]
for cast,titles,rel,rate in zip(crew,title,rel_year,ratings):
cnt=cnt+1
movie_detail={
"Rank":cnt,
"Title":titles,
"ReleaseDate":rel,
"Ratings":rate,
"Cast":cast
}
movie_list.append(movie_detail)
```

这个代码块，压缩我们所有的变量，将它们绑定到一个字典中，然后将它追加到一个列表中。这使得我们的列表充满了我们的数据。

```
def successfulYear():
year_count = []
for i in movie_list:
year_count.append(i["ReleaseDate"])
year_count = dict(Counter(year_count))
print("-----------------Most Successful Year of Indian Cinema        According To IMBD--------------------",sep="\n")
print()
totalShare=0.0
totalCount=0
for i in year_count.keys():
percent = (year_count[i]/len(movie_list))*100
print('\t\t\t\t\t\tRelease Year\t Count\t Share(%)')
if year_count[i]>9:
print('\t\t\t\t\t\t',i,"\t\t\t",year_count[i],"\t\t","{:.2f}".form   at(percent))
totalCount = totalCount + year_count[i]
totalShare = totalShare + percent
else:
print('\t\t\t\t\t\t',i, "\t\t\t", year_count[i],         "\t\t\t", "{:.2f}".format(percent))
totalCount = totalCount + year_count[i]
totalShare = totalShare + percent
print("Total number of movies \t\t ",totalCount," Total Number of share ","{:.0f}".format(totalShare))
```

此功能有助于确定哪一年的电影发行数量最多，以便确定哪一年的电影发行数量最多并显示出来。

```
def favDirector():
director=[]
actor=[]
for i in movie_list:
cast_crew=[str(i["Cast"]).split(',')]
for i in cast_crew:
director.append(str(i[0]).strip(' (dir.)'))
actor.append(str(i[1]).lstrip(' '))
actor.append(str(i[2]).lstrip(' '))
print("-------------------List of Top Director in Descending Order---- ----------")
print(Counter(director))
print("-------------------List of Top Actors/Actress in Descending      Order--------------")
print(Counter(actor))
```

这个代码块有助于查看哪个导演发行的电影数量最多或者更成功。

```
successfulYear()
print('----------------------------------------')
favDirector()
```

## 结论

所以这都是关于网络废弃和网络废弃的一个小项目。

[点击此处](https://github.com/ParthNipunDave/WebScrapping)获取该项目的 GitHub 链接