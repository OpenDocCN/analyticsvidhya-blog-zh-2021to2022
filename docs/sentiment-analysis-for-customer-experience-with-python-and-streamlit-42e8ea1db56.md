# 使用 Python 和 Streamlit 进行客户体验情感分析

> 原文：<https://medium.com/analytics-vidhya/sentiment-analysis-for-customer-experience-with-python-and-streamlit-42e8ea1db56?source=collection_archive---------10----------------------->

## 开始利用 Vonage 消息 API 改善客户服务，例如整理来自脸书等社交媒体平台的评论反馈。

情感分析使用自然语言处理技术从客户反馈中挖掘见解，以确定反馈数据是积极的、消极的还是中性的。企业利用情感分析来监控产品和品牌情感，并了解客户的需求。

本教程涵盖以下内容:

*   初始化 Vonage 上的消息 API 沙箱
*   通过脸书从客户那里收集数据
*   通过以下方式创建一个机器人来处理脸书上的客户反馈:
*   将这些数据存储到数据库或文本文件中
*   向客户返回相关响应
*   通过 Streamlit 创建仪表板，实时了解客户的反馈意见
*   用积极、消极和中立的尺度衡量顾客的情绪
*   通过可视化分析客户的关键难点

# 先决条件

*   [Node.js](https://nodejs.org/en/download/)
*   [蟒 3](https://www.python.org/download/releases/3.0/)

# Vonage API 帐户

要完成这个教程，你需要一个 [Vonage API 账户](http://developer.nexmo.com/ed?c=blog_text&ct=2021-05-04-sentiment-analysis-for-customer-experience-with-python-and-streamlit)。如果你还没有，你可以今天[注册](http://developer.nexmo.com/ed?c=blog_text&ct=2021-05-04-sentiment-analysis-for-customer-experience-with-python-and-streamlit)并开始用免费信用点数进行构建。一旦你有了一个帐户，你可以在 [Vonage API 仪表板](http://developer.nexmo.com/ed?c=blog_text&ct=2021-05-04-sentiment-analysis-for-customer-experience-with-python-and-streamlit)的顶部找到你的 API 密匙和 API 秘密。

![](img/8b2ad68d40f7d4b615fc9511bda4d8f0.png)

# 创造一个脸书机器人

Messages API 沙箱允许企业通过各种社交渠道发送和接收消息，如 WhatsApp、脸书和 Viber。它还允许企业通过这个 API 发送短信和彩信。为了通过任何外部社交渠道发送消息，您需要每个提供商的商业帐户，并将其连接到您的 Vonage APIs 帐户。

在本教程中，我们将使用脸书，所以请确保您有一个脸书帐户准备测试。

要开始创建我们的应用程序，在您的项目目录中运行以下命令来初始化节点项目，然后安装所需的第三方库:

```
npm init -y
npm install express body-parser nedb @vonage/server-sdk@beta dotenv
```

以上命令将创建`package.json`、`package.lock`文件以及一个`node_modules`目录。在`package.json`文件中，你会看到我们刚刚安装的库，比如`express`和`vonage/server-sdk`。您将看到的示例如下所示:

```
{
  "name": "facebook-bot",
  "version": "1.0.0",
  "description": "Sign up for a Vonage API account at https://dashboard.nexmo.com.",
  "main": "index.js",
  "scripts": {
    "serve": "node index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@vonage/server-sdk": "^2.10.7-beta",
    "body-parser": "^1.19.0",
    "dotenv": "^8.2.0",
    "express": "^4.17.1",
    "nedb": "^1.8.0"
  },
  "devDependencies": {}
}
```

当来自脸书的消息进来时，Vonage 将向预先配置的 webhook URL 发送一个 HTTP 请求。您的节点应用程序应该可以访问互联网来接收它，所以我们建议使用 [Ngrok](https://learn.vonage.com/blog/2017/07/04/local-development-nexmo-ngrok-tunnel-dr) 。

在您的终端中，运行以下命令在端口`3000`上启动 ngrok(请注意，每次 ngrok 重新启动时，转发 URL 都会改变，因此如果您要重新启动 Ngrok，请记住这一点):

```
npx ngrok http 3000
```

我们现在需要在[仪表板](https://dashboard.nexmo.com/applications/new)中创建一个应用程序。确保添加一个与应用程序目的相关的名称，然后打开`Messages`，因为该应用程序将需要使用消息 API。然后点击“生成公钥和私钥”按钮。这将迫使你的浏览器下载一个`private.key`文件。将它放在项目的根目录下。最后，单击`Generate new application`按钮，这将把您重定向到应用程序显示页面。制作页面上显示的`Application ID`的副本，因为这在下面的代码中也会用到。

当你在这个页面上时，我们需要定义 webhooks URLs。webhook 是 API 的扩展，但是 Vonage 将数据发送给你，而不是你的代码从 API 平台请求数据。数据以 web 请求的形式到达您的应用程序，这可能是早期 API 调用的结果(这种类型的 webhook 也称为“回调”)，例如对消息传递 API 的异步请求。Webhooks 也用来通知你的应用程序一些事件，比如一个传入的消息被存储在一个文件中，或者向应用程序发送一个传出的消息。

url 需要在[仪表板](https://dashboard.nexmo.com/messages/sandbox)中更新，如下所示(确保用您的 ngrok url 替换`<ngrok URL>`):

*   入站网页挂钩:`http://<ngrok URL>/webhooks/inbound`
*   状态网页挂钩:`http://<ngrok URL>/webhooks/status`

导航回[消息沙箱](https://dashboard.nexmo.com/messages/sandbox)将你的脸书账户加入 Facebook Messenger 沙箱的白名单。点击`Add to sandbox`并按照页面上的说明进行操作。

将您的凭证存储到一个可以通过`index.js`访问的`.env`文件中。

```
VONAGE_API_KEY=###
VONAGE_API_SECRET=###
VONAGE_APPLICATION_ID=e0c5d5d8-###
```

接下来，在`index.js`文件中，包含之前已经安装的模块，如 express、Vonage server-sdk，如下例所示:

```
const app = require('express')()
const bodyParser = require('body-parser')
const nedb = require('nedb')
const Vonage = require('@vonage/server-sdk')
```

然后，我们需要用文件名`messages.db`创建一个数据库，将下面一行添加到您的`index.js`文件中:

```
const messages = new nedb({ filename: 'messages.db', autoload: true })
```

我们现在需要用我们的凭证初始化一个新的 Vonage 对象，凭证由 API 密钥、API 秘密、应用程序 ID 以及 private.key 的路径和文件名组成。在您的`index.js`文件中添加以下代码:

```
const vonage = new Vonage({
  apiKey: process.env.VONAGE_API_KEY,
  apiSecret: process.env.VONAGE_API_SECRET,
  applicationId: process.env.VONAGE_APPLICATION_ID,
  privateKey: './private.key'
}, {
  apiHost: 'https://messages-sandbox.nexmo.com'
})
```

我们将创建一个名为`sendMessage`的函数，它将向脸书发送消息，以响应客户的查询、反馈或评论。在如下例所示的函数中，有一个`to`和一个`from`，它们是 API 的指令。`to`(或收件人)需要是白名单中的脸书帐户。将下面的函数添加到您的`index.js`文件中:

```
function sendMessage(sender, recipient, text) {
  const to = { type: 'messenger', id: recipient }
  const from = { type: 'messenger', id: sender }
  const message = { content: { type: 'text', text: text } }

  vonage.channel.send(to, from, message, function(error, result) {
    if(error) { return console.error(error) }
    console.log(result)
  })
}
```

在上面的例子中，您将看到`vonage.channel.send`，它发送一条带有参数`to`、`from`和 message 的消息，如果出现错误，将返回该错误，并将记录错误消息。

接下来，我们需要在应用程序中添加 webhook 来监听 URL 中的路径`/webhooks/inbound`,每当收到脸书消息时，它就会从 Messages API 接收消息。它从正文中解析并获得发送者的 messenger id 以及发送的文本，连同时间戳，并插入到响应消息中，给出错误条件 if-else 将成功理解用户的反馈。

如果出现错误，发送回发送者的消息将是“对不起！你能重复一下吗？”。另一方面，如果 messenger 检索并向应用程序发送客户的消息，该消息将存储在`messages.db`中，同时回复“感谢您的反馈和评论！”将被返还给用户。因此，将以下内容添加到您的`index.js`文件中:

```
app.post('/inbound', function(request, response) {
  if (request.body.message.content.text.toLowerCase().trim() === 'recap') {
    messages.find({'from.id': request.body.from.id }, function (error, records) {
      if (error) { return console.error(error) }
      const message = records.map(function(record) {
        return record.message.content.text + ' (sent at ' + record.timestamp + ')'
      }).join('\n\n')
      sendMessage(request.body.to.id, request.body.from.id, message)
    })
  } else {
    messages.insert(request.body, function (error, record) {
      if (error) {
        sendMessage(request.body.to.id, request.body.from.id, 'Sorry! Could you repeat?')
        return console.error(error)
      }
      sendMessage(request.body.to.id, request.body.from.id, 'Thanks for your feedback and review!')
    })
  }

  response.send('ok')
})
```

最后，我们需要为`/webhooks/status`端点添加另一个监听器，它将返回消息“ok”。我们还需要添加 listen 功能，使应用程序作为 web 服务器在端口`3000`上运行和监听。因此，在您的`index.js`文件的底部添加以下内容:

```
app.post('/status', function(request, response) {
  console.log(request.body)
  response.send('ok')
})
app.listen(3000)
```

现在是时候测试您的应用程序了！首先您需要运行您的 web 应用程序，在您的终端中，运行以下命令:

```
node index.js
```

现在，如果你去 [Facebook Messenger](https://www.messenger.com/) ，找到 Vonage 沙盒用户并给它发送消息。您将在 Messenger 中收到回复。

# 分析用户的反馈

通过 Vonage 沙箱从客户脸书获得的数据存储在`messages.db`文件中。在模拟演示中，一个用户评论了一家定制公司的产品和服务，数据样本记录在下面的示例脚本中。

```
{"message_uuid":"e6e659be-3cdb-464f-96ef-8597cb307586","from":{"type":"messenger","id":"3819505444810553"},"to":{"type":"messenger","id":"107083064136738"},"message":{"content":{"type":"text","text":"how terrible can this product be, does not solve my pain point"}},"timestamp":"2021-03-15T08:43:30.993Z","_id":"6TdDhlo8CVxEr7tq"}
{"message_uuid":"7775fa83-f073-481c-8866-c60bca28ec97","from":{"type":"messenger","id":"3819505444810553"},"to":{"type":"messenger","id":"107083064136738"},"message":{"content":{"type":"text","text":"how can there be no response for so long on such bad service"}},"timestamp":"2021-03-15T08:44:37.924Z","_id":"GSVzREWsYllKHOlJ"}
```

我们可以看到，对于发送给脸书机器人的每条消息，每个`message_uuid`都是唯一的。`from`和`to`分别指定了客户和机器人的身份。该消息包含文本形式的内容，只是客户提供的反馈。为了分析这种反馈，将通过 python Streamlit 框架和 TextBlob、WordCloud、Matplotlib 和 Pandas 等库来可视化它们。创建一个名为`dashboard.py`的新文件，并将以下代码添加到这个新文件中:

```
import streamlit as st
from textblob import TextBlob
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
import matplotlib.pyplot as plt
import pandas as pd
st.set_option('deprecation.showPyplotGlobalUse', False)
```

[Streamlit](https://streamlit.io/) 是一个面向机器学习和数据科学团队的开源应用框架。 [TextBlob](https://textblob.readthedocs.io/en/dev/) 是一个用于处理文本数据的 Python 库。它提供了一个简单的 API 来处理常见的自然语言处理(NLP)任务。WordCloud 是一种显示给定文本中哪些单词出现频率最高的技术。Matplotlib 是一个全面的库，用于在 Python 中创建静态、动画和交互式可视化。Pandas 是一个快速、强大、灵活且易于使用的开源数据分析和操作工具，构建于 Python 编程语言之上。

使用所有这些工具，首先，我们打开存储在 messages.db 中的数据，读取并处理从所有用户那里收集的每条反馈记录。我们查找消息/内容并删除时间戳，因为这对于反馈中的情感分析是不需要的。处理后的反馈将存储在列表中。

下面的示例执行上面列出的指令。将此添加到您的`dashboard.py`文件的底部:

```
file1 = open('messages.db', 'r')
Lines = file1.readlines()
feedback_list = list()
for line in Lines:
   feedback = line.split(":")[11]
   feedback = feedback.replace('}},"timestamp"',"")
   feedback_list.append(feedback)
```

接下来，我们生成一个词云来可视化用户反馈中提供的词的频率。我们可以看到，某些关键词更加突出，如“可怕”、“长”、“坏”等。Wordcloud 只是统计每个单词在客户提供的反馈的所有句子中出现了多少次。更好的方法是分析客户的整体情绪。

我们可以对句子使用 TextBlob 基本情感极性分析，根据他们的文本反馈给出相对可靠的客户情感体验估计，并存储在新的情感值列表中。现在在您的`dashboard.py`中添加以下代码:

```
sentiment_values = list()
for feedback in feedback_list:
    sentiment_feedback = TextBlob(feedback).sentiment.polarity
    sentiment_values.append(sentiment_feedback)
```

接下来，我们将展示-1 到 1 范围内的整体情绪。对于高于 0 的情绪极性，输出将是绿色的，而对于那些意味着不快乐的负值，情绪将在网页上显示为红色。将以下内容复制到您的`dashboard.py`文件中:

```
st.title("Overall Sentiment")
if average_sentiment > 0:
   colors = 'green'
else:
   colors = 'red'
```

目前，总体情绪为-0.417，这表明总体上许多反馈并不支持公司的产品和服务。我们可以在表格中看到反馈，其中显示了每个反馈意见。很少有 0 值的感悟。这意味着分析发现这种反馈是中性的，不能理解信息的负面含义。我们可以看到“该产品无法匹配…”和“只有免费赠送给我时，我才会使用该产品”会暗示负面情绪，但结果是中性或积极的。因此，可以为这个用例的情感分析开发一个更复杂的学习和训练模型。此外，整体情绪分析可能不想只是简单的平均，而是潜在的更复杂的计算或加权平均。

可以通过初始化以下命令来启动 Streamlit web dashboard 应用程序。要设置 web 仪表板，需要安装 Streamli。flask 或 Django 中的其他部署方法也同样有效，可以进行探索。

```
streamlit run dashboard.py
```

总之，本教程展示了如何设置并连接一个 Vonage Messages API 到脸书，并收集来自客户的反馈，然后使用情绪分析对这些反馈进行分析，以对客户的情绪进行实时监控。