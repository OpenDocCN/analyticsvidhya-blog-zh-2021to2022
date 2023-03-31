# 使用 Selenium 和 Python 制作 Instagram 机器人

> 原文：<https://medium.com/analytics-vidhya/making-an-instagram-bot-using-selenium-and-python-ea94f217d0dd?source=collection_archive---------3----------------------->

![](img/c57391fd7d7826a5fa2691c6908d6858.png)

想获得更多 Instagram 关注者？不想创造好的吸引人的内容？这篇文章是给你的。

几乎每个人都想在自己选择的社交媒体上拥有更多关注者。如果你不愿意花大价钱购买粗略的追随者，并希望自动化这项任务，我发现最好的解决方案是使用 Selenium。Selenium 是一个自动化 web 浏览器的 Python 库。Instagram API 被有意设计成几乎不可能通过使用它来培养追随者，所以我们不得不依赖一些非正统的解决方案。

我们将使用书中最古老的技巧来获得一些追随者。我们只是随机跟踪人们，希望他们也能跟踪我们。从一些广泛的测试中，我确定如果你随机跟随别人，大约十分之一的人会跟随你回来。不太好，但总比没有好。

首先，您需要设置并安装 Selenium 库。这个网站和几个谷歌搜索应该足以让这个开始运行。[https://www . Selenium . dev/documentation/en/web driver/#:~:text = Selenium % 20 web driver % 20 reference % 20 to % 20 both，reference % 20 to % 20 as % 20 just % 20 web driver](https://www.selenium.dev/documentation/en/webdriver/#:~:text=Selenium%20WebDriver%20refers%20to%20both,referred%20to%20as%20just%20WebDriver)。

一旦设置好 selenium，我们就可以创建 python 文件 InstaBot.py 了

这背后的主要策略是使用 Selenium 提供的自动浏览工具登录 Instagram 网站上的帐户，浏览所有那些烦人的弹出窗口，搜索一些知名人士的个人资料，并开始关注随机关注他们的人。

我在大多数动作之间放置了随机时间延迟，以模拟人类用户使用网站的随机性，并尝试避免 Instagram 的机器人检测算法。

如果你想定制这个脚本，你只需在普通浏览器中打开 Instagram，右键点击屏幕，然后按 Inspect Element。将鼠标悬停在网站的任何部分，您都会看到元素 xpath，当将其放入 find_element_by_xpath()方法中时，Selenium 驱动程序可以对其进行修改。你只需要 send_keys()和 click()就可以开始制作你自己的有趣的东西了。

这是一个非常基本的脚本，旨在展示如何使用 Selenium 工具与 Instagram 网站进行交互，还有大量的改进空间。在以后的文章中，我计划展示如何将这个脚本连接到 google cloud 上的虚拟机，以便让它每天运行，而不需要您付出任何努力，而不是每次都手动运行它。我可以深入研究的另一个可能的领域是制作一个 CSV 文件，其中包含脚本之前追随的人，然后在以后的日期随机取消追随他们(必须保持这个比例)。

注意:这个脚本有一些问题，其中最重要的是你可以跟随任何人。我们只是关注名人的最新追随者，这可能包括普通人、其他机器人或你的老板。没有人想在自己的个人账户上使用一个机器人，最后跟着一群孩子(令人毛骨悚然)。我找不到解决这个问题的办法，所以在用一个废弃的 Instagram 账户测试后，我决定不在我的个人账户上继续使用它。如果有人有如何解决这个问题的想法，请在下面留下评论。

另一个小问题是，Instagram 网站偶尔会略有变化，您必须使用 Inspect Element 工具仔细检查并找到新的 xpath 元素，但这只是几分钟的工作，非常容易完成。如果您得到某种错误，比如没有找到元素，这可能就是问题所在。

感谢您的阅读，如果您有任何问题，请发表评论。