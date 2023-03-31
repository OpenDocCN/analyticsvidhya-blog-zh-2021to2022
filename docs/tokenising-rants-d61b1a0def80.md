# 表征咆哮——数据

> 原文：<https://medium.com/analytics-vidhya/tokenising-rants-d61b1a0def80?source=collection_archive---------20----------------------->

![](img/e1c3fb15c5e5e33485028b7780604895.png)

图片来自[我的新奥尔良](https://www.myneworleans.com/10-ways-to-alleviate-stress/)

疫情和随之而来的隔离切断了我们与许多线下互动的联系，这些交流曾经是我们许多人的安全出口。随着经济下滑和死亡率上升，我们人类作为适应性种族，走上网络平台来摆脱我们的负面情绪。

无论是推特上的咆哮或趋势标签，还是媒体上令人心碎的故事，或者心理健康论坛上经常相关的帖子，都有大量数据可供我们分析，所以我开始了。

在本文中，我们将讨论分析所需的数据收集(通过网络搜集)。这是精神健康数据符号化和分析系列文章的第 1 部分。

# 第一次尝试:Tweepy 和社交网络

虽然很多网站不允许你从他们的页面上抓取数据，幸运的是，Twitter 有一个流行的 API 让你的生活更容易，python 有了 Tweepy 包让这变得更容易。然而，要使用 Tweepy，你需要在 Twitter 上创建一个[开发者账户](https://developer.twitter.com/en/apply-for-access)。

一旦你的账户设置好了，你将拥有自己独特的令牌来使用 Twitter API，并且你将会在使用 Tweepy 抓取 Tweepy 时需要它们。

让我们先获得授权。还记得您在上创建的所有唯一令牌创建了一个开发者帐户吗？我们在这里使用它们。

> 一个建议:不要和任何人分享这些代币，除非你想因为某人滥用你的账户而被起诉。出于某种原因，这些令牌是秘密且唯一的。

```
from tweepy import OAuthHandlerfrom tweepy.streaming import StreamListener
import tweepy
import json
import pandas as pd
import timeaccess_token = "your_access_token_here"
access_token_secret = "your_access_token_secret_here"
api_key = "your_api_key_here"
api_secret_key = "your_api_key_secret_here"auth = OAuthHandler(api_key, api_secret_key)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, wait_on_rate_limit = True, wait_on_rate_limit_notify= True)
```

最后是刮削，

```
tweets_list = []query_list = [
    "anxiety", "anxious", "depression", "depressed", "suicidal", 
    "suicide", "ptsd", "trauma", "mental ilness", "triggered", 
    "bipolar", "mental health", "mental illness", "therapist", "therapy"
]countries = [
     "USA", "UK", "Finland", "India", "Canada", "Switzerland"
]date_since = "2018-01-01"for query in query_list:
    for country in countries:
        places = api.geo_search(query=country, granularity="country") # Acquiring geocodes for selected countries to filter the twitter query results
        place_id = places[0].id
        query_string = query + f" place:{place_id}"
        try:
            tweets = tweepy.Cursor(api.search, q = query_string , lang="en", tweet_mode='extended', since=date_since).items(200)for tweet in tweets:
                tweets_list.append([tweet.full_text, tweet.user.location, query])
            print(f"Query: {query} for country: {country} done!")
#             time.sleep(15*60)
        except:
            print(f"Something went wrong with query: {query}")
            continue
```

然而，这个决定适得其反。在社交媒体上，用户倾向于在非常轻松的背景下使用抑郁和焦虑等沉重的词汇。这并不意味着推文不是用户可能遭受某种精神痛苦的迹象，但鉴于幽默已经成为当代人非常常见的应对机制，收集的数据很快变得非常复杂。

另一个问题是 Tweepy 的刮极限。对于一个免费开发者的帐户，Twitter 的 API 有一个非常低的限制，将导致你的刮刀需要相当长的时间，直到你可以获得一个很好的数据块进行分析。

然而，已经进行了研究来分析使用这些社交媒体平台的用户的精神状态。然而，这需要更多的深入研究，我将在以后的文章中深入探讨。现在，让我们专注于一个更加小众的平台。

# 第二次尝试:BeautifulSoup 和 BeyondBlue

专门的论坛总是为数据收集提供更合适的来源，这个项目也不例外。在谷歌上快速搜索会出现几个专门讨论精神健康的论坛，而 BeyondBlue 被证明是一个极好的起点。

BeautifulSoup 是 PYTHON 上非常流行的网络抓取库。如果你熟悉网页结构和 CSS 选择器的基础知识，从网上抓取数据(只要允许)是一件非常简单的事情！最上面的樱桃就是刮不限量。使用 BeautifulSoup，您可以抓取网页上静态可用的所有数据。

> 然而，BeautifulSoup 的这一特性是一把双刃剑——虽然它给了你快速收集数据的自由，但你在选择选择器时也必须非常小心，否则你的数据集将很快被大量不必要的元数据堵塞。

在这个项目中，我坚持三个常见的精神健康话题:

1.  焦虑
2.  抑郁
3.  悲伤和损失

幸运的是，在 BeyondBlue.org，上述每个话题都有专门的论坛主题。因此，我们所要做的就是在基本 URL 后面添加相应线程的扩展，然后让我们的 scraper 施展它的魔法。

在我们进入代码之前，这里有一个抓取过程的快速纲要:

1.  我们将从每个线程中只抓取原始张贴者的内容。在检查网页结构时，可以清楚地看到，在每个论坛的主页上，**每个线程都被包裹在一个锚标签中，标签的类名为“sfforumThreadTitle”**，所以这就是我们的 scraper 要搜索的目标。
2.  对每个线程的进一步检查显示**线程下的每个帖子都被包装在一个 div 标签中，标签的类为“postAndSig”**。所以我们遍历线程页面中的每个 div，找到第一个 post (OP 的 post ),然后跳出循环，进入下一个线程。
3.  要移动到论坛中的下一页，我们只需更改 URL 中的页码:**base _ URL+'/<MH _ topic>/page/'<page _ number>**

```
**## importing libraries**import requests
import csv
import time
from bs4 import BeautifulSoup as BSdata = []
base_url = '[https://healthyfamilies.beyondblue.org.au/seeking-support/helping-yourself-and-others/online-forums'](https://healthyfamilies.beyondblue.org.au/seeking-support/helping-yourself-and-others/online-forums')
headers = {'User-Agent': 'Mozilla/5.0'}**## Anxiety**anxiety_threads = []
**url = base_url + '/anxiety'**
for i in range(1,10):
    print("========================================")
    print(f"Scraping page#{i} ( url: {url} )........")
    page = requests.get(url, headers=headers)
    soup = BS(page.text, 'html.parser') ** # Step 1**
    links = soup.find_all('a', class_="sfforumThreadTitle") 
    for link in links:
        thread_url = base_url + '/anxiety/' +(link.attrs['href'].split("/"))[1]
        thread_page = requests.get(thread_url, headers=headers)
        thread_soup = BS(thread_page.text, 'html.parser') ** # Step 2**
        divs = thread_soup.find_all("div")
        for div in divs:
            if(div.attrs.get("class")==["postAndSig"]):
                anxiety_threads.append([div.text, 'Anxiety'])
                print(f"Scraped {link.text}")
                break
    print("========================================") **# Step 3** 
    url = base_url+'/anxiety/page/'+str(i+1)
    time.sleep(60)**## Depression**depression_threads = []
url = base_url + '/depression'
for i in range(1,10):
    print("========================================")
    print(f"Scraping page#{i} ( url: {url} )........")
    page = requests.get(url, headers=headers)
    soup = BS(page.text, 'html.parser') **# Step 1**
    links = soup.find_all('a', class_="sfforumThreadTitle")
    for link in links:
        thread_url = base_url + '/depression/'+ (link.attrs['href'].split("/"))[1]
        thread_page = requests.get(thread_url, headers=headers)
        thread_soup = BS(thread_page.text, 'html.parser') **# Step 2**
        divs = thread_soup.find_all("div")
        for div in divs:
            if(div.attrs.get("class")==["postAndSig"]):
                depression_threads.append([div.text, 'Depression'])
                print(f"Scraped {link.text}")
                break
    print("========================================") **# Step 3**
    url = base_url+'/depression/page/'+str(i+1)
    time.sleep(60)**## Grief and Loss**grief_threads = []
url = base_url + '/grief-and-loss'
for i in range(1,10):
    print("========================================")
    print(f"Scraping page#{i} (url: {url})........")
    page = requests.get(url, headers=headers)
    soup = BS(page.text, 'html.parser') **# Step 1**
    links = soup.find_all('a', class_="sfforumThreadTitle")
    for link in links:
        thread_url = base_url + '/grief-and-loss/'+ (link.attrs['href'].split("/"))[1]
        thread_page = requests.get(thread_url, headers=headers)
        thread_soup = BS(thread_page.text, 'html.parser') **# Step 2**
        divs = thread_soup.find_all("div")
        for div in divs:
            if(div.attrs.get("class")==["postAndSig"]):
                grief_threads.append([div.text, 'Grief'])
                print(f"Scraped {link.text}")
                break
    print("========================================") **# Step 3**
    url = base_url+'/grief-and-loss/page/'+str(i+1)
    time.sleep(60)
```

最后，我们将收集到的所有数据放入一个 Pandas 数据框，并将其存储在一个 CSV 文件中以供进一步分析。为了区别，我甚至保存了特定文本的论坛类别。

```
data = anxiety_threads + depression_threads + grief_threads
print(f"Posts collected: {len(data)}")import pandas as pddata_df = pd.DataFrame(data = data, columns=['text', 'category'])
data_df.to_csv("BeyondBlue_Data.csv", index=False)
```

我们完了。

也就是数据收集。接下来是数据预处理部分，所有 NLP 爱好者(或者可能只有我)都对这个过程又爱又恨。

尽管如此，NLP 管道的一个非常重要的方面，我将在下一篇文章中介绍，敬请关注。

在那之前，快乐的(合法的)网络搜集！