# 如何用 Python 自动化 Whatsapp(或任何网站)

> 原文：<https://medium.com/analytics-vidhya/how-to-automate-whatsapp-or-any-website-with-python-71d10dad7b68?source=collection_archive---------6----------------------->

![](img/ee5d60b0919d7dd1d231ceb76b1bd448.png)

本文将介绍如何使用名为 selenium 的 Python 模块以编程方式自动化 WhatsApp Web 或任何其他网页。这可能会受到一些页面的限制，这些页面专门使用验证码来防止使用机器人，但是 selenium 对于实验来说非常好。

这篇文章在很大程度上受到了我最近完成的一个项目的启发: [WhatsappPlusPlus](https://zee.gl/OiKGKfX) ，在这个项目中，我自动化了 WhatsApp Web，创建了一个浮动窗口来给我的朋友发短信，很像脸书的聊天工具，但要原始得多。该项目在 GUI 和其他功能的其他模块中使用 Tkinter，但是今天我们不会讨论它，所以如果您感兴趣，请查看代码。

**使用本文末尾的代码查看示例项目。**

# 硒是什么？

Selenium 是 web 自动化的 python 模块。这与网络抓取有很大不同，后者主要是在静态网站上完成的，HTML 通常是在静态网站上下载和解析的。相反，Web 自动化是用代码与网页上的元素进行交互的行为。Selenium 是一个一体化的包，允许我们解析动态页面中的 html，并与元素进行交互。理论上，selenium 可以用于 web 抓取，但是我不推荐它。

# 安装

**我们需要做的第一件事是安装硒。**这很容易做到，使用:

```
pip install selenium
```

如果这不起作用，请查看我在[如何用 PIP 安装模块(并在它失败时修复它)](https://kgdavidson.co.uk/python-bare-minima--how-to-install-modules-with-pip-and-fix-it-when-it-fails/)中的故障排除提示来安装 selenium。

**第二件事是让 webdriver 与 selenium 一起使用。webdriver 是浏览器的精简版本，专门用于自动化。大多数现代浏览器都有可供下载的网络驱动程序。然而，在本教程中，我们将使用 Chrome webdriver。下载可以在[这里](https://zee.gl/1kR8EgB)找到。你必须确保驱动程序版本与你安装的 Chrome 版本相匹配，并且你已经解压了存档文件。**

# 入门指南

既然我们已经有了所有需要的东西，我们可以开始写代码了。我建议创建一个新文件夹来包含所有内容。**我已经将下载的 chromedriver 放在了一个空 python 项目(main.py)旁边的文件夹中，我们的代码将放在那里。**

## 设置驱动程序

首先，我们将导入以下内容:

```
from selenium import webdriver import time
```

我们导入*时间*纯粹是为了延迟后面的事情，防止事情跑得太快。

然后，我们将使用以下代码来设置 selenium 以使用我们的驱动程序:

```
currentPath = __file__.split("main.py")[0] 
driverPath = currentPath + r"chromedriver.exe" 
driver = webdriver.Chrome(driverPath)
```

***current path*变量简单地获取包含我们的 python 文件**的文件夹的路径，并且通过扩展，获取包含我们的 chromedriver 的文件夹的路径。

***driver path*变量使用 *currentPath* 创建 chromedriver 的完整路径。**这样做是为了防止从不同的地方运行 python 文件所导致的错误。这两行确保 python 代码找到 chromedriver，只要它在同一个文件夹中。

***驱动*变量将作为我们对浏览器窗口的直接访问，并将用于所有未来的交互。**我们使用从 selenium 导入的 webdriver 组件指定 webdriver 用于 Chrome。我们将驱动程序的路径作为参数传入。如果可靠性不成问题，你可以用 chromedriver 的完整路径替换 *driverPath* 。

## 安全关闭浏览器

单独运行这段代码将会显示一个 chrome 窗口，通知说:“Chrome 正由自动化测试软件控制。”在 python 终端中还有一条消息:“DevTools 监听 ws://…..”。

你会看到 python 代码会继续运行，直到你关闭 chrome 窗口。但是，我们希望在结束程序时正确关闭 chrome 窗口。这很容易做到，使用:

```
driver.close()
```

将它添加到代码中会使浏览器窗口在突然关闭之前打开一会儿。我们想做的是在关闭浏览器窗口之前运行一些代码。**因此，我们将在开发过程中对该行进行注释。**

```
#driver.close()
```

## 加载网站

我们需要 selenium 的下一个基本功能是加载一个特定的网站。这也很简单，如下所示:

```
driver.get("http://web.whatsapp.com")
```

你可以把任何网站放在报价里面，它就会被加载。

# 设置 ChromeDriver 选项

ChromeDriver 选项只是您在代码中指定的使用驱动程序的首选项。有两个你应该知道的 ChromeDriver 选项。我们将只使用其中的一个来自动化 Whatsapp，但是另一个是值得了解的。

## 设置选项

首先，我们需要导入以下内容:

```
from selenium.webdriver.chrome.options import Options
```

这让我们创建一个 *chromeOptions* 变量:

```
chromeOptions = Options()
```

**我们必须在驱动变量创建之前创建此变量**，因为我们将调整驱动变量创建，如下所示:

```
driver = webdriver.Chrome(driverPath, options=chromeOptions)
```

既然我们的驱动程序变量已经被更改为使用我们的选项，我们需要通过 *chromeOptions* 变量指定您想要使用的选项。**我们添加的所有选项都需要在初始化 *chromeOptions* 变量之后、驱动程序创建之前直接出现。**

## 在会话之间保存 cookies

我们将利用的第一个选项是在脚本运行之间保存 cookies 的能力。在我们的情况下，这是必要的，以防止用户在跑步时扫描新的二维码。可以这样做:

```
chromeOptions.add_argument("user-data-dir=" + currentPath + "cookies")
```

这行代码的作用是指定 cookie 将保存在一个名为*cookie*的文件夹中，这个文件夹与我们的脚本在同一个文件夹中。这通过使用我们的 *currentPath* 变量来表示。文件夹路径可以是任何东西，这正是我所选择的。

## 隐藏浏览器窗口

在一些 selenium 项目中，您可能会发现需要隐藏浏览器窗口并向用户显示自定义 GUI。您可以使用以下方式在 selenium 中隐藏浏览器窗口:

```
chromeOptions.add_argument('headless') chromeOptions.add_argument( "user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3312.0 Safari/537.36")
chromeOptions.add_argument("remote-debugging-port=3333")
```

第一行是隐藏窗口的那一行。另外两个只是我的推荐，不完全需要。

包含第二行是因为一些无头模式的网站会出现错误。例如，WhatsApp Web 将浏览器注册为过时。**第二行绕过了这一点，让网站认为浏览器是更新版本。**

第三行指定了无头模式的远程调试端口。**这意味着如果你想在开发无头模式时看到你的浏览器窗口，你可以转到:**

```
localhost:3333
```

**在您的普通浏览器中，显示隐藏浏览器窗口的实时视图。**如果需要，端口号当然可以在代码中更改。

**在本例中，我们将使用 cookie 持久性，而不是 headless 模式。**

# 识别网站元素

自动化 WhatsApp 的下一步是识别网站的各个部分。然后可以点击这些部分或对其进行文本解析。selenium 中有一些内置的方法可以帮助实现这一点。

**列出的函数分为返回单个元素的函数和返回元素列表的函数。**每种类型在各自的情况下都很有用。除了这些功能，还有一些其他的功能可以在这里找到，然而我认为这些是最有用的。下面列出的所有函数只是接受一个对应于属性值的字符串。

```
driver.find_element_by_id() 
driver.find_element_by_tag_name() driver.find_element_by_class_name() driver.find_elements_by_tag_name() driver.find_elements_by_class_name()
```

下面是 WhatsApp Web 指定的一个例子。在加载 WhatsApp Web 并扫描二维码后，加载的页面会在左上角显示您的个人资料图片。使用 Inspect Element (Ctrl + Shift + C)来选择 profile 图片元素会显示一个 HTML 元素，如下所示:

这意味着 div 有一个“_3LtPa”类，因此我们应该使用:

```
driver.find_element_by_class_name("_3LtPa")
```

它获取具有给定类名的第一个元素。要获得该类的所有元素，我们可以使用:

```
driver.find_elements_by_class_name("_3LtPa")
```

同样，我们可以使用:

```
driver.find_element_by_id("_3LtPa")
```

如果 HTML 元素碰巧显示为:

最后两个功能:

```
driver.find_element_by_tag_name("div") driver.find_elements_by_tag_name("div")
```

将分别返回屏幕上 div 标记的第一个实例或所有实例。这个函数可以找到 HTML 中存在的任何标签。

所有这些函数都可以相互链接，以获取其他元素中的元素。例如:

```
driver.find_element_by_tag_name("div")[0].find_elements_by_class_name("_3LtPa")
```

这将查找页面上的第一个 div 标记，然后查找该 div 标记中类名为“_3LtPa”的所有元素。

# 等待页面加载

现在我们知道了如何识别页面上的元素，我们需要在开始自动化 WhatsApp 之前搞清楚最后一步。这是为了确定页面何时完成加载。遗憾的是，selenium 没有任何内置的功能来做到这一点。幸运的是，检查页面是否已经加载并不太困难。

**这个方法，简单来说，就是我们暂停执行，直到找到一个只存在于最终加载的页面中的元素。**在代码中，这是通过 while 循环和元素标识完成的，如下所示:

```
while not driver.find_elements_by_class_name("_3LtPa"):     ⠀⠀⠀⠀time.sleep(1)
```

**这段代码一直等到找到类名为“_3LtPa”的元素。**我们正在重用“_3LtPa”类名，据我所知，只有当用户扫描了二维码并加载时，才会加载该类名。您可以选择符合您标准的任何其他元素。***time . sleep*每次找不到元素时，代码执行延迟一秒钟。**在这个用例中，响应时间不是很重要，因此我们可以承受页面加载的延迟。您可以根据需要增加或减少延迟时间。

# 自动化所有的事情

现在我们已经完全准备好自动化 WhatsApp Web。在这里，我将讨论自动化时你可能需要的三个主要功能。**这将是从一个元素中获取文本，输入文本** **并单击。**

# 获取文本

在 selenium 中获取文本是通过访问任何 selenium 元素的文本变量来完成的:

```
driver.find_element_by_class_name("_1MZWu").text
```

**记住*文本*不是一个函数。**

# 键入文本

首先在文本框中键入文本需要您已经获得了文本框的元素。从那里你可以*发送 _keys()* 到元素。

```
driver.find_element_by_class_name("_1awRl").send_keys("Hello World!") driver.find_element_by_class_name("_1awRl").send_keys('\ue007')
```

第二行是 selenium 允许您发送的特殊字符(Enter/Return)的示例。这里是所有可用键的列表。

# 微小静电干扰声

点击进入 selenium 就像跑步一样简单。在识别的元素上单击()。

```
driver.find_element_by_class_name("_3Tw1q").click()
```

点击一个元素后，你可以检查一个新的元素出现，或者简单地设置一个持续的延迟。需要有某种延迟，以允许页面在点击后加载。

# 最终代码

这里显示的最后一段代码自动加载 WhatsApp，等待用户登录，然后选择第一个联系人并发送一条消息“Hello World！”。然后，程序等待几秒钟，并关闭浏览器窗口。

*原载于 2021 年 1 月 14 日*[*【https://kgdavidson.co.uk】*](https://kgdavidson.co.uk/miscellaneous--how-to-automate-whatsapp-or-any-website-with-python/)*。*