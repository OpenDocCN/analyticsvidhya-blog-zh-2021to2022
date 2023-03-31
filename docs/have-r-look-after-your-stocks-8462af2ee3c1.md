# 让 R 看好你的股票！

> 原文：<https://medium.com/analytics-vidhya/have-r-look-after-your-stocks-8462af2ee3c1?source=collection_archive---------17----------------------->

![](img/23d5da71c9c0a1a9e0cc3fc13c836aed.png)

> “睡觉的时候不找到赚钱的方法，就一直工作到死。”—沃伦特自助餐。

如果你喜欢涉足股票市场，或者只是出于好奇，你可能会面临这样的困境:你应该多久检查一次你的股票价格。断断续续的价格检查可能是一个真正的生产力杀手。如果你正在奋斗中，这篇文章给你带来了好消息！您可以使用 R 来完成持续检查的枯燥工作，并在有趣的事情发生时通知您！

在编码领域，有一个非常流行的原则叫做“三法则”,它的意思是

> 如果你不得不使用某个东西超过三次，你不应该复制和粘贴代码，而应该把它们放在一个过程(函数)中。

查股价也是你每次去查价格都要重复的一系列任务。这可以很容易地用 R！

# 概观

我们试图解决的问题场景有两大部分:

1.  不断检查一组股票的价格变动
2.  一旦发生有趣的事情(盈利/亏损),就做某事(出售)

在本教程中，我们将通过使用以下工具来完成这两部分(实际上部分完成了第二步，因为我们不会进行任何自动股票交易):

1.  连续价格检查:使用 Quantmod 包中的 getQuote (),
2.  触发通知做某事:使用 Twilio 包中的 tw_send_message()。

# 准备

由于我们将使用 Twilio 的免费短信服务，我们首先需要在 Twilio 的网站上开一个免费账户。这是一个非常简单的程序。您可以按照这个[视频](https://www.youtube.com/watch?v=kTdMEc4LkKk#action=share)中的说明来设置您的 Twilio 账户，并准备好与 r 一起使用。确保您的个人电话号码通过 Twilio 验证，以便向其发送股票走势通知。

设置好帐户后，存储 Twilio 帐户中的两条信息:SID 和 Token。r 将需要使用这两条信息来连接到您的 Twilio 帐户。

# 使用的库

对于这个项目，我们将使用三个库:

*   发送信息
*   Quantmod:获取股票价格
*   dplyr:用于数据处理程序

# 设置环境变量

要使用 Twilio，我们需要在 R 中设置两个系统环境变量:TWILIO_SID 和 TWILIO_TOKEN。

我们将使用 Sys.setenv()函数来设置这些环境变量，如下所示:

> *sys . setenv(TWILIO _ SID = " TWILIO _ SID _ NUMBER ")
> sys . setenv(TWILIO _ TOKEN = " TWILIO _ TOKEN _ NUMBER ")*

```
# LIBRARIES ----
library(quantmod)
library(twilio)
library(dplyr)Sys.setenv(TWILIO_SID = 'ACf43caa6608884e01b935971dffce9d54')
Sys.setenv(TWILIO_TOKEN = '2c68a2e9f06ac668e105f4c806374929')
```

# 获取股票价格

我们将使用的主要函数是 Quantmod 包中的 *getQuote()* 函数。例如，要获得 BestBuy 股票的最新定价细节，可以像这样调用函数:

> *quantmod::getQuote(' BBY ')*

```
quantmod::getQuote('BBY')##              Trade Time   Last     Change    % Change   Open  High    Low
## BBY 2020-08-28 16:00:01 111.23 0.01000214 0.008993109 111.68 111.7 110.11
##      Volume
## BBY 2324806
```

输出将显示您的时间呼吁，最后的价格，当天的开盘价，最高和最低价格的一天，以及其他信息。

出于我们的目的，我们需要重复调用这个函数，因此我们将把这个 *getQuote()* 放在一个包装函数中，这个包装函数将重复调用这个函数来获取我们所选股票的最新价格。

# 发送短信

我们将使用 Twilio 包中的 *tw_send_message()* 函数来发送通知消息。要检查您的 Twilio 设置是否正常工作，在加载库并设置系统环境变量之后，您可以调用-

> *tw _ send _ message(to = " YOUR _ VARIFIED _ CELL _ NO "，from = "YOUR_TWILIO_CELL_NO "，body = body )*

确保您使用的单元格编号以国家代码开头，中间没有空格或特殊字符(例如+100756245)。

出于我们的目的，我们需要为每个调用动态地修改消息体，因此我们将把这个 *tw_send_message()* 函数放在一个定制的函数中。

# 创建包装函数

## 发送短信的包装功能

下面的包装函数 *send_msg()* 将允许我们动态更新消息体，以显示股票名称、更新后的价格和其他信息。

```
# SEND MESSAGE ----
send_msg = function(body){
  tw_send_message(
  to = Sys.getenv("YOUR_VARIFIED_CELL_NO"),
  from = Sys.getenv("YOUR_TWILIO_CELL_NO"),
  body = body
)
}
```

## 股票价格检查的包装函数

这个包装函数 *stock_price_check()* 将允许我们自动运行 *getQuote()* 函数来获取我们想要的股票的详细股价信息。 *stock_price_check()* 基本上是一个 while 循环，它会以一定的间隔持续执行，直到手动停止循环。

在 *stock_price_check()* 内部将要完成的任务是这样的:

> *stock _ price _ check = function(ticker，price，quantity，threshold){
> while(logic){
> 1 .获取股票价格
> 2。计算利润 3。在 R 控制台
> 4 中打印计算结果。检查利润是否达到设定的阈值
> 4.1 如果达到阈值:发送短信
> 4.2 如果没有达到阈值:什么都不做
> 5。等待一定的间隔
> 6。重复步骤 1 至 5 } }*

```
# Fetch stock price and send SMS
# @param tickers A String vector of stock tickers 
# @param price A numeric vector of stock prices. In the same order of tickers
# @param quantity A numeric vector of stock quantity purchased. In the same order of tickers
# @param threshold A numeric input that shows the desired profit margin. Defaults to 4% stock_price_check = function(tickers, price, quantity, threshold = 4){

  # setting up a counter. 
  i = 0   
  while(i < 2){ 

    # Step 1\. Fetching stock price
    latest_price = quantmod::getQuote(tickers)

    # Step 2\. Calculating profit
    latest_price[['Buy']] = c(price)
    latest_price[["Q"]] = c(quantity)
    latest_price[["Profit/Loss"]] = c(latest_price[["Last"]] - latest_price[["Buy"]])
    latest_price[["Total_PL"]] = c(latest_price[["Profit/Loss"]] * latest_price[["Q"]])
    latest_price[["Total_PL%"]] = c(round(latest_price[["Total_PL"]] / (latest_price[["Buy"]] * latest_price[["Q"]]) * 100, 2))

    # Stp 3\. Printing out calculations in R console
    print(latest_price[c('Trade Time', 'Last', 'Buy', 'Total_PL', 'Total_PL%')])
    cat("\n")

    # Step 4\. Checking if the profit meets desired profit threshold
    df = as.data.frame(latest_price) %>%
      filter(`Total_PL%` > threshold) 

    if(nrow(df) > 0){
      # Step 4.1 Sending SMS if profit meets threshold
      msg = paste0(threshold, "% profit alert!\n",  
                   paste0(rownames(df), 
                          ": Profit %: ", df[,"Total_PL%"], 
                          "| Total Profit: ", round(df[, "Total_PL"]),
                          collapse = ' ; \n'))
      send_msg(body = msg)
      message(msg)
    }

  # Step 5\. Waiting for certain interval (60 seconds)
  Sys.sleep(60) # Step Drop: Incrementing counter
  i = i + 1 # comment out this line when you run. I had to put an increment to make sure the while loop stops after 4 executions
  }
}
```

## 例子

现在让我们假设我们拥有一个迷你投资组合，包含两只股票**Best Buy**(BBY——以 114.09 美元购买的 14 只股票)和**惠普企业**(HPE——以 9.30 美元购买的 645 只股票)。我们有兴趣建立一个提醒，一旦投资组合中的任何股票达到 4%的利润率，就会通知我们。

**注意**我已经将该功能设置为只运行两次。确保注释掉了函数内部的计数器增量。

```
stock_price_check(tickers = c("BBY", "HPE"), price = c(114.09, 9.30), quantity = c(14, 645))##              Trade Time   Last    Buy Total_PL Total_PL%
## BBY 2020-08-28 16:00:01 111.23 114.09   -40.04     -2.51
## HPE 2020-08-28 16:03:43   9.83   9.30   341.85      5.70## 4% profit alert!
## HPE: Profit %: 5.7| Total Profit: 342##              Trade Time   Last    Buy Total_PL Total_PL%
## BBY 2020-08-28 16:00:01 111.23 114.09   -40.04     -2.51
## HPE 2020-08-28 16:03:43   9.83   9.30   341.85      5.70## 4% profit alert!
## HPE: Profit %: 5.7| Total Profit: 342
```

# 设置 R 作业

一旦这个函数运行 *stock_price_check()* 它将一直运行，除非被停止。但它会占用 R 控制台，这可能不是每个人的理想选择。如果你想让它在后台运行，你可以使用 R Studio 的“作业”功能。请遵循以下步骤:

*   转到“工具”部分，选择“启动本地作业”。
*   对于 R 脚本，浏览到该脚本的位置并选择该脚本
*   点击开始！

你应该看到 R Studio 将启动这个任务，它基本上是调用这个脚本并运行它。运行后，您应该会看到以下两个输出:

*   您的投资组合详细信息(交易时间、最新价格、您的购买价格、总利润美元和利润%)每分钟都在打印，
*   如果一只股票超过了利润率阈值，短信就会发送到你的手机上。

# 限制

这是一个非常简单的脚本，解决了一个非常小的股票投资组合的问题。一些明显的和一些不太明显的限制是:

*   这个股价电话下班后就不管用了。一旦正式运营结束，您将不断获得活跃时段的最新价格，
*   stock_price_check()函数可以通过添加从平面文件获取投资组合详细信息的功能变得更加用户友好。这可能会让拥有较大股票投资组合的人变得非常容易。

# 感谢阅读！

通过阅读这篇文章，您应该对 R 中的函数是如何工作的有一个基本的了解，以及如何在您的个人生活中使用它！

**不确定接下来该读什么？我为你挑选了另一篇文章:**

[](https://towardsdatascience.com/automate-your-repetitive-reports-5ee60a53bda2) [## 自动化您的重复性报告！

### 自动化 R 脚本

towardsdatascience.com](https://towardsdatascience.com/automate-your-repetitive-reports-5ee60a53bda2) 

# 阿拉法特·侯赛因

*   ***如果你喜欢这个，*** [***跟我上媒***](/@curious-joe) ***了解更多***
*   ***让我们连线上*** [***领英***](https://www.linkedin.com/in/arafath-hossain/)
*   ***有兴趣合作吗？查看我的*** [***网站***](https://curious-joe.net/)