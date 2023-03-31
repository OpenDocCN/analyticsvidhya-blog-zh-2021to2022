# 使用 Python 进行 Web 抓取

> 原文：<https://medium.com/analytics-vidhya/web-scraping-with-python-91d2e955a363?source=collection_archive---------7----------------------->

## 如何从网站上抓取数据并将其转储到 CSV 中

![](img/21e1a48f06cce1f6cec8c910e16dfc36.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[**网络搜集**](https://realpython.com/beautiful-soup-web-scraper-python/) 是从互联网上搜集信息的过程。

**注意:**如果你只是出于教育目的抓取万维网上的一个页面，那似乎没什么问题。但是，你仍然应该考虑检查他们的*服务条款*，因为很少有网站不喜欢自动抓取器收集他们的数据，而其他人并不介意。

让我给你一个简单的例子来说明它的用途。比如你想从一个网站上买一个受欢迎的产品，这个产品一上来就没货了。一种方法是访问网站，做一些过滤，每天检查你的产品的可用性。

为此，你必须每天花几分钟时间，以便下次它出现在网站上时你可以抓住它。另一种方法是，你可以用 python web scraping 来自动化它，而不是每天查找。

它非常有助于你在网上自动搜索，将你的搜索范围扩大到你想要的任何网页，等等。

## 抓取测试页面

[链接](https://codedamn-classrooms.github.io/webscraper-python-codedamn-classroom-website/)

**请求**模块允许您使用 Python 发送 HTTP 请求。

HTTP 请求返回一个包含所有响应数据(内容、编码、状态等)的响应对象。

```
import requests

res = requests.get('link to your web page')
```

现在我们将使用 Python 中一个名为 [**的库 BeautifulSoup**](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#:~:text=Beautiful%20Soup%20is%20a%20Python,hours%20or%20days%20of%20work.) 来做网页抓取。

> Beautiful Soup 是一个 Python 库，用于从 HTML 和 XML 文件中提取数据。它与您喜欢的解析器一起工作，提供导航、搜索和修改解析树的惯用方式。它通常为程序员节省数小时或数天的工作。

要了解更多关于它的特性，你可以查看:[链接](https://www.crummy.com/software/BeautifulSoup/)

如果你没有太多的时间去查阅它的文档和特性，你只需要弄清楚一件事: ***BeautifulSoup 可以解析你在网上给它的任何东西。***

从测试页面抓取标题的简单例子。

```
from bs4 import BeautifulSoup

page = requests.get("https://codedamn-classrooms.github.io/webscraper-python-codedamn-classroom-website/")soup = BeautifulSoup(page.content, 'html.parser')title = soup.title.text # gets you the text of the <title>(...)</title>
```

**没错，就是这么简单。**

现在可以试试刮头、刮身、刮标题等。在我们深入研究收集产品并将其存储到 CSV 中之前，您需要自己动手。

[**Github 链接到资源库**](https://github.com/rahulkapoor253/WebScraping) **。**

```
import requests
from bs4 import BeautifulSoup
import csv

res = requests.get('https://codedamn-classrooms.github.io/webscraper-python-codedamn-classroom-website/')

soup = BeautifulSoup(res.content, 'html.parser')
```

现在我们可以使用 soup 变量属性开始收集数据。

```
all_products = []

products = soup.select("div.thumbnail")for product in products:
    name = product.select("h4 > a")[0].text.strip()
    price = product.select("h4.price")[0].text.strip()
    description = product.select("p.description")[0].text.strip()
    reviews = product.select('div.ratings')[0].text.strip()
    image = product.select("img")[0].get("src")

    all_products.append({
        "product_name": name,
        "price": price,
        "description": description,
        "reviews": reviews,
        "image": image
    })

columns = all_products[0].keys()
with open("my_data.csv", "w", newline="") as file:
    dict_writer = csv.DictWriter(file, columns)
    dict_writer.writeheader()
    dict_writer.writerows(all_products)
```

我来解释一下上面刚刚发生的事情。

我们可以看到，所有产品的详细信息都显示在一个 div 中，类名为 thumbnail。下面提到的是代表每个产品的 HTML。

`<div class=”thumbnail”>
<img alt=”item” class=”img-responsive” src=”/webscraper-python-codedamn-classroom-website/cart2.png”/>
<div class=”caption”>
<h4 class=”pull-right price”>$1139.54</h4>
<h4>
<a class=”title” href=”/webscraper-python-codedamn-classroom-website/test-sites/e-commerce/allinone/product/593" title=”Asus AsusPro Advanced BU401LA-FA271G Dark Grey”>Asus AsusPro Adv…</a>
</h4>
<p class=”description”>
Asus AsusPro Advanced BU401LA-FA271G Dark Grey,
14", Core i5–4210U, 4GB, 128GB SSD, Win7 Pro 64bit,
ENG
</p>
</div>
<div class=”ratings”>
<p class=”pull-right”>7 reviews</p>
<p data-rating=”3">
<span class=”glyphicon glyphicon-star”></span>
<span class=”glyphicon glyphicon-star”></span>
<span class=”glyphicon glyphicon-star”></span>
</p>
</div>`

**名称**:在< h4 >后面跟一个<标签。所以我们使用了 product . select(" H4>a ")[0]. text . strip()

**价格**:内< h4 >同档次价格

**描述**:带类描述的< p >内

**评论**:内部<部门>有等级评定

**图像**:在属性为 src 的< img >内

然后有代码将追加到 **all_products** 列表中的数据写入 CSV。这是在 python 中的 [csv 模块](https://docs.python.org/3/library/csv.html)的帮助下完成的。

如果你在评论中遇到任何困难，请告诉我。