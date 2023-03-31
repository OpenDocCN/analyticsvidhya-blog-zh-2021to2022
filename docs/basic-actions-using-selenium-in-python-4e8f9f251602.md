# Python 中使用 Selenium 的基本操作

> 原文：<https://medium.com/analytics-vidhya/basic-actions-using-selenium-in-python-4e8f9f251602?source=collection_archive---------1----------------------->

输入数据，查找元素，点击对象！

![](img/6e2cddbb011d60bf4d3dcd1ef73409cc.png)

[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Selenium 是一个开源框架，主要用于 web 应用程序的自动化测试。它可以用于不同的编程语言，其中 Java 和 Python 是最常用的。它用于创建测试脚本，在非技术语言中，这意味着在不同阶段测试我们的应用程序的代码。

尽管除了 Selenium 之外，还有许多工具和技术可用于测试，但它是需要理解的重要资源。我已经分享了 selenium 中使用的一些基本命令，这些命令将执行某些任务或输入某些数据。我使用 python 作为编程语言，使用 Google Chrome (chromedriver)作为网络浏览器。

想法是它复制了整个过程，这有助于测试，但也重建或复制工作流程。我们从本地保存 chromedrive 开始，并针对不同的用例遵循这些基本命令

1.  导入**图书馆**

不同的库用于不同的功能。下面提到的方法在大多数情况下可能会有帮助。

注意:在下面的代码中并没有使用所有的库，这只是作为参考，可能涵盖了大部分的用例

```
**import** **time
from** **selenium** **import** webdriver
**from** **selenium.webdriver.common.keys** **import** Keys
**from** **selenium.webdriver.support.ui** **import** Select
**from** **selenium.webdriver.support.select** **import** Select
**from** **selenium.webdriver.common.action_chains** **import** ActionChains
**from** **selenium.webdriver.common.by** **import** By 
**from** **selenium.webdriver.support.ui** **import** WebDriverWait 
**from** **selenium.webdriver.support** **import** expected_conditions **as** EC
```

2.给出本地存储的 **chrome 驱动程序**的路径

```
PATH = "C:\Program Files (x86)\chromedriver.exe"
driver = webdriver.Chrome(PATH)
```

3.我们现在给**网站**我们想要执行自动化或测试

```
*driver.get("https://podcasts.google.com/")*
```

4.我们现在使用特定按钮的 **click** 函数，我们发现它使用了 HTML 中的不同元素。这里我们使用了 **xpath** 来获得唯一值。

注意:找到 xpath 最简单的方法是右键单击某个元素>右键单击 Inspect >将鼠标悬停在突出显示该元素的代码上>右键单击 copy > Copy Xpath

```
search = driver.find_element_by_xpath('//*[[@id](http://twitter.com/id)="sdgBod"]/span[2]')search.click()
```

5.我们看到的下一个函数是向文本框发送数据，从代码中给出输入

```
search = driver.find_element_by_xpath('//*[[@id](http://twitter.com/id)="gb"]/div[2]/div[2]/div/form/div/div/div/div/div/div[1]/input[2]')search.send_keys('Lex Fridman Podcast')
```

有一些最常用于自动化的基本功能，通过单击输入数据并在应用程序中向前移动，主要是在下一步或发送按钮上，但取决于类型和用例。