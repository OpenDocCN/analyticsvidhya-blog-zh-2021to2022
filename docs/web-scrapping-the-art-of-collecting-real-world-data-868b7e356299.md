# 网络报废:收集真实世界数据的艺术

> 原文：<https://medium.com/analytics-vidhya/web-scrapping-the-art-of-collecting-real-world-data-868b7e356299?source=collection_archive---------24----------------------->

![](img/d9dc4c93682accb90d4c04fbafd6acd0.png)

[来源](https://www.google.com/search?q=scrapping&sxsrf=ALeKk03Iyf5CRIkhbBOk_O83uV3ScVX7tg:1616766095572&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjg0r6Gi87vAhXjwjgGHZecD0gQ_AUoAXoECAEQAw&biw=1366&bih=625#imgrc=FjYOG8MKEAfKIM)

> **什么是 Web 报废？**

在客户端和服务器(网站)之间建立连接以解析特定网站的数据的过程称为 web 报废。假设你是一名 NLP 工程师，为一家特定的公司工作，该公司希望了解客户对他们在市场上提供的产品的看法，为此他们没有给你 csv、excel 或 json 格式的数据，而是给你发布评论的网站的身份验证。在这种情况下，你必须使用 web scrapping 从 web 中提取数据，然后根据你的方便将所有这些数据保存到数据库或 csv 文件格式中，以便进一步操作。

换句话说，我们可以说 web 报废是一个从第三方来源提取信息的前数据科学过程。作为一名优秀的数据科学家，我们应该了解网络报废。

这个博客将帮助你从头开始建立你的自定义评论剪贴簿

**在这个博客中，我们将实施端到端的网络报废项目。让我们开始吧……**………

> **请求**

我们可以使用`import requests`导入请求库。request 的主要任务是建立客户端和服务器之间的连接。如果连接将被建立，那么它将在`200–299`范围内打印`status_code`输出。假设我想解析来自 flipkart 网站的信息，如果该网站允许解析相关信息，那么我们将在`200–220`之间获得状态代码。

```
import requests
r=requests.get("https://www.flipkart.com/")
print(r.text)  #print all the textual data present in website
print(r.status_code) #print result between 200-299 if website            
                     #allowed to parse the data
print(r.headers["content-type"]) #print text/html; charset=utf-8
print(r.encoding)      #print utf-8
```

> 美味的汤

Beautiful Soup 是一个 python 库，用于解析 html 页面中的文本数据。假设我们想要解析关于联想笔记本电脑的所有评论，那么这些事情可以使用 beautiful soup 来完成。它可以很容易地从`bs4`包装中导入

```
from bs4 import BeautifulSoup as bs
```

> **美人汤里的** `**find**` **和** `**find_all**` **有什么区别？**

假设我们想要删除一个有 10 个 `h5`标签的网页，如果我们应用`find(”h5")`，那么我们将只得到第一个出现的`h5`标签的数据。但是在`find_all(“h5”)`中，我们将获得网页中所有`h5`标签的数据。

> 在下面的任务中，我将使用两个库**请求**和 **urllib**

**urllib :-** 它访问 URL 地址并试图读取该地址，但是**请求**做两件事首先它读取 URL 地址，其次它对该 URL 进行编码

## 任务 1:-取消对 iphone 7 的评论

在这里，我们将为您想要从 flipkart 中删除的任何产品评论构建一站式解决方案。

**步骤 1:导入基本库**

```
import requests 
from bs4 import BeautifulSoup
from urllib.request import urlopen as uReq
import pandas as pd
import numpy as np
```

`requests`这个库将读取 URL 并对其中的数据进行编码。 `uReq`将要读取的 URL 地址。`Pandas`因为我们将把数据存储成 csv 格式，因此我们需要熊猫库。`bs4`用于解析 html 标签中的所有数据。

**步骤 2 :-建立一个自动链接，用于提取用户选择的评论**

```
import requests
from bs4 import BeautifulSoup
from urllib.request import urlopen as uReq
import pandas as pd
import numpy as np
product=input("Enter the product name \n") #enter the product name whose review you want to scrap
url="https://www.flipkart.com/search?q="+product.replace(" ","")
uClient=uReq(url)  #requesting the url
url=uClient.read() #reading the url
uClient.close()    #closing the connection
soup=BeautifulSoup(url,"html.parser") #parsing the html page
data_class=soup.find_all("div",{"class":"_1AtVbE col-12-12"})
data=data_class[2]
# here are the following things we are going to scrap
ratings = []
comment_headings = []
user_comments = []
user_names = []
number_of_likes = []
locations = []
for page in range(1,41):    #prininting the reviews of first 30 pages
    url = "https://www.flipkart.com" + data.div.div.div.a["href"].format(page)  # link
    req = requests.get(url)  # setting the connection with given website
    soup_updated = BeautifulSoup(req.text, "html.parser")  # parse the data
    comments = soup_updated.find_all("div", {"class": "col _2wzgFH"})
    # upper code will print all the data present in comment class
    for comment in comments:
        rating = comment.find("div", class_="_3LWZlK _1BLPMq")
        if rating is not None:
            ratings.append(rating.text)
        else:
            ratings.append(np.nan)
        comment_heading = comment.find("p", class_="_2-N8zT")
        if rating is not None:
            comment_headings.append(comment_heading.text)
        else:
            comment_headings.append(np.nan)
        user_comment = comment.find("div", class_="t-ZTKy")
        if user_comment is not None:
            user_comments.append(user_comment.text)
        else:
            user_comments.append(np.nan)
        user_name = comment.find("p", class_="_2sc7ZR _2V5EHH")
        if user_name is not None:
            user_names.append(user_name.text)
        else:
            user_names.append(np.nan)
        location = comment.find("p", class_="_2mcZGG")
        if location is not None:
            locations.append(location.text)
        else:
            locations.append(np.nan)
        likes_count = comment.find("span", class_="_3c3Px5")
        if likes_count is not None:
            number_of_likes.append(likes_count.text)
        else:
            number_of_likes.append(np.nan)
#taking all these scrapped information into dictionary format
dict1={"users":user_names,"ratings":ratings,    "heading":comment_headings,"reviews":user_comments, "location":locations,"likes count":number_of_likes}
#converting all the scrapped data into dataframe
df_scrapped=pd.DataFrame(dict1)
print(df_scrapped.head())
df_scrapped.to_csv("scrapped_df.csv")
```

> **为什么我们在将值追加到列表**时使用了 `**if else**` **条件**

如果我们不使用 if else 条件，那么我们将得到**属性错误:- None 类型对象不是文本。为了克服这一点，我们应用了`if else`条件，让我们更准确地理解它。**

```
if rating is not None:
    ratings.append(rating)
```

如果 html 标签的 rating 类中不存在任何空值，那么我们将把这些值追加到我们已经获取的`ratings`列表中。

```
else:
    ratings.append(np.nan)
```

如果在这种情况下我们没有任何评级(html 类标签不包含任何值)，我们将使用`Nan`填充空值。

> **为什么我们要用** `**Nan**` **填充** `**None**` **值为什么我们要额外添加** `**else**` **块**

正如我们所知，在构建数据框时，每一列都应该具有相同的大小，否则我们将无法创建数据框，因此我们采用了额外的`else` 块，每当我们获得`None`值作为结果时，该块将返回`Nan`值。

N 注:——取`if else`条件总是明智的，它不会在废弃任何一种产品评论时给出任何属性错误。

**第三步:生成 csv 文件**

点击此 [**链接**](https://github.com/Akasonal/Review-Scrapper/blob/main/scrapped_df.csv) 查看报废数据框。

**结论:-**

这些都是我的观点，如果你对博客的进一步改进有任何建议，请在下面评论。别走开，我们将一起学习更多的话题。**不断学习不断探索……..**