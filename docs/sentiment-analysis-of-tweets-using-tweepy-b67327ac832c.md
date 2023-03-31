# 使用 Tweepy 的推文情感分析

> 原文：<https://medium.com/analytics-vidhya/sentiment-analysis-of-tweets-using-tweepy-b67327ac832c?source=collection_archive---------2----------------------->

# 介绍

本文中我们的主要目标是使用 tweepy 从 twitter 中收集数据，并对废弃的 tweepy 执行情感分析，以获得关于某个主题的公众意见。

维基百科定义的情感分析:

> 情感分析(也称为意见挖掘或情感人工智能)是指使用自然语言处理、文本分析、计算语言学和生物统计学来系统地识别、提取、量化和研究情感状态和主观信息

因此，我们在这里要做的是使用 NLTK 对文本进行预处理，并在 TextBlob 的帮助下将其分类为正面、负面或中性。

在开始之前，我们需要从 twitter 上收集数据。为此，你需要一个 Twitter 开发者账户。此外，您将需要安装 NLTK、Tweepy 和 Wordcloud 来进行编码。如果你用的是 google colab，这些库已经安装好了。

可以使用以下命令安装这些库:

```
pip install **nltk**pip install **wordcloud**pip install **tweepy**
```

# **代码**

## **导入所需的库**

让我们从导入所需的模块开始。

```
**import** **re** 
**import** **tweepy** 
**import** **nltk**
**import** **pandas** **as** **pd**
**import** **matplotlib.pyplot** **as** **plt**
**from** **wordcloud** **import** WordCloud,STOPWORDS
nltk.download('punkt')   
nltk.download('stopwords')
**from** **nltk.corpus** **import** stopwords
**from** **nltk.tokenize** **import** word_tokenize
**from** **nltk.stem** **import** PorterStemmer
**from** **tweepy** **import** OAuthHandler 
**from** **textblob** **import** TextBlob
```

## **认证**

导入所需的模块后，我们需要连接到 Twitter API，这是通过创建一个身份验证对象来完成的。

我们使用 OAuth 将凭证传递给应用程序，也就是说，按照 Wikipedia 的定义:

> “OAuth 是一种开放的访问授权标准，通常用于互联网用户授权网站或应用程序访问他们在其他网站上的信息，但不需要给他们密码。亚马逊、谷歌、脸书、微软和 Twitter 等公司使用这一机制，允许用户与第三方应用程序或网站共享其账户信息。”

简单地说，OAuth 允许我们的代码通过我们的 Twitter 帐户访问数据，而不需要我们给它我们帐户的用户名和密码。

下面的代码创建了一个函数，帮助你使用你的 twitter 账户来抓取推文:

```
**def** connect():
  *# Replace the xxxxx with your twitter api keys*
  consumer_key = 'xxxxx'
  consumer_secret = 'xxxxx'
  access_token = 'xxxxx'
  access_token_secret = 'xxxxx'

  **try**:
    auth = OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth)
    **return** api
  **except**:
    print("Error")
    exit(1)
```

在这里，用您的凭证替换标有“xxxxx”的参数，您将在通过您的 twitter 开发人员帐户创建应用程序后获得这些凭证。作为参考，您可以使用这两个视频:

1) [Twitter API 初学者教程(Python)](https://www.youtube.com/watch?v=ae62pHnBdAg&t=2s)

2) [如何轻松获取 Twitter API KEY](https://www.youtube.com/watch?v=vlvtqp44xoQ&t=199s)

虽然视频中显示的申请表有点过时，但它应该会让你对需要什么有一个很好的了解。

## **数据预处理**

我们在这里主要要做的是删除*****所有提到的*** (@ username) *、* ***停用词*** (如“the”、“and”等词)。).这些参数意义不大，因为它们不给出任何情绪的指示，并且移除这些参数节省了我们的存储器、计算能力，并且有时还增加了模型的准确性。**

**我们将在这里使用正则表达式来做预处理。我们还将执行 ***词干*** ，因为这也有助于节省内存和计算能力。**

**可以参考下面的代码来更好地理解数据是如何预处理的。**

```
****def** cleanText(text):
  text = text.lower()
  *# Removes all mentions (@username) from the tweet since it is of no use to us*
  text = re.sub(r'(@[A-Za-z0-9_]+)', '', text)

  *# Removes any link in the text*
  text = re.sub('http://\S+|https://\S+', '', text)

  *# Only considers the part of the string with char between a to z or digits and whitespace characters*
  *# Basically removes punctuation*
  text = re.sub(r'[^\w\s]', '', text)

  *# Removes stop words that have no use in sentiment analysis* 
  text_tokens = word_tokenize(text)
  text = [word **for** word **in** text_tokens **if** **not** word **in** stopwords.words()]

  text = ' '.join(text)
  **return** text**
```

**下面的函数演示了词干处理是如何完成的:**

```
****def** stem(text):
  *# This function is used to stem the given sentence*
  porter = PorterStemmer()
  token_words = word_tokenize(text)
  stem_sentence = []
  **for** word **in** token_words:
    stem_sentence.append(porter.stem(word))
  **return** " ".join(stem_sentence)**
```

## ****情绪分析****

**我们将使用 TextBlob 对象执行情感分析。要了解更多，你可以参考这篇博文。**

**我们将把文本转换成 TextBlob 对象，然后使用它的。情感方法来获得给定的陈述是积极的、消极的还是中性的。**

**使用。情绪法，我们得到每种说法的极性和主观性。Polarity 是一个范围在[-1，1]之间的整数，其中-1 表示该语句为负数，1 表示该语句为正数。主观性告诉我们给出的陈述是个人观点还是基于事实。在这里，我们不会使用主观性，而只是根据极性对推文进行分类。**

**我们将首先编写一个函数，它将一个 TextBlob 对象作为输入，然后将其分类为阳性、阴性或中性。下面给出的函数是它的一个实现。**

```
****def** sentiment(cleaned_text):
  *# Returns the sentiment based on the polarity of the input TextBlob object*
  **if** cleaned_text.sentiment.polarity > 0:
    **return** 'positive'
  **elif** cleaned_text.sentiment.polarity < 0:
    **return** 'negative'
  **else**:
    **return** 'neutral'**
```

**现在，我们要做的是使用 Twitter API 来获取 tweets，然后使用我们之前编写的 cleanText 函数来清理它们，然后将其附加到一个列表中来存储数据。我们还需要确保不考虑 retweets，我们将通过在 API 向 Twitter 发出的查询中添加-filter:retweets 来修改查询。**

**这可以通过以下方式实现:**

```
****def** fetch_tweets(query, count = 50):
  api = connect() *# Gets the tweepy API object*
  tweets = [] *# Empty list that stores all the tweets*

  **try**:
    *# Fetches the tweets using the api*
    fetched_data = api.search(q = query + ' -filter:retweets', 
count = count)
    **for** tweet **in** fetched_data:
      txt = tweet.text
      clean_txt = cleanText(txt) *# Cleans the tweet*
      stem_txt = TextBlob(stem(clean_txt)) *# Stems the tweet*
      sent = sentiment(stem_txt) *# Gets the sentiment from the tweet*
      tweets.append((txt, clean_txt, sent))
    **return** tweets
  **except** tweepy.TweepError **as** e:
    print("Error : " + str(e))
    exit(1)**
```

**我们已经完成了主要逻辑的实现，现在我们将调用这些函数，之后我们将在 wordcloud 中绘制这些函数。**

```
**tweets = fetch_tweets(query = 'Hindi Language Imposition', count = 200)
*# Converting the list into a pandas Dataframe*
df = pd.DataFrame(tweets, columns= ['tweets', 'clean_tweets','sentiment'])

*# Dropping the duplicate values just in case there are some tweets that are copied and then stores the data in a csv file*
df = df.drop_duplicates(subset='clean_tweets')
df.to_csv('data.csv', index= **False**)ptweets = df[df['sentiment'] == 'positive']
p_perc = 100 * len(ptweets)/len(tweets)
ntweets = df[df['sentiment'] == 'negative']
n_perc = 100 * len(ntweets)/len(tweets)
print(f'Positive tweets **{**p_perc**}** %')
print(f'Neutral tweets **{**100 - p_perc - n_perc**}** %')
print(f'Negative tweets **{**n_perc**}** %')**
```

**这里我们调用函数并将数据保存在一个 csv 文件中。然后我们找出正面、负面和中性推文的百分比。**

**最后，我们将使用下面给出的代码绘制一个包含最常用单词的单词云:**

```
**twt = " ".join(df['clean_tweets'])
wordcloud = WordCloud(stopwords=STOPWORDS, background_color='black', width=2500, height=2000).generate(twt)

plt.figure(1,figsize=(13, 13))
plt.imshow(wordcloud)
plt.axis('off')
plt.show()**
```

**![](img/1b25374a07493e20f26f4fb06e32e513.png)**

**单词云的输出**

**我希望这篇文章让你很好地理解了如何使用 Twitter API 对推文进行情感分析。你可以在这里找到我的代码的链接。**