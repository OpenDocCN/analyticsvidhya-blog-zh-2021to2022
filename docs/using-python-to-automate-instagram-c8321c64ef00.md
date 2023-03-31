# 使用 python 实现 Instagram 自动化

> 原文：<https://medium.com/analytics-vidhya/using-python-to-automate-instagram-c8321c64ef00?source=collection_archive---------12----------------------->

![](img/c9eb9374ee09904a71693f422a11a51b.png)

在 Instagram 上自动化喜欢帖子的过程是非常容易的。我将快速介绍如何创建一个 python 脚本来处理这个日常任务。

如果您有使用 Selenium python 库的经验，请继续阅读。否则，要么在这里查看 Selenium 文档:[https://www.selenium.dev/documentation/en/](https://www.selenium.dev/documentation/en/)。或者看看我之前用 Selenium 的一篇文章:[https://medium . com/analytics-vid hya/sending-the-rock-a-DM-on-insta gram-using-python-9ca 002 a3 e 9 CB](/analytics-vidhya/sending-the-rock-a-dm-on-instagram-using-python-9ca002a3e9cb)或者[https://medium . com/analytics-vid hya/making-an-insta gram-bot-using-Selenium-and-python-ea 94 f 217d 0 DD](/analytics-vidhya/making-an-instagram-bot-using-selenium-and-python-ea94f217d0dd)。

我们只需使用 Selenium 提供的浏览器自动化功能，就可以在 Instagram web 应用程序上编写操作脚本。通过 X-path 选择 HTML 元素，并使用 Selenium 工具按下按钮并将输入发送到文本字段，我们可以像按下按钮一样轻松地喜欢 Instagram 帖子。你可能会问“在 Instagram 上喜欢一个帖子通常不是一键操作吗？”。答案是肯定的，但是你不能为了这么做而编写质量有问题的 python 代码。

为了使用这个脚本，你必须下载 chromedriver 和 Selenium 库到你的电脑上。然后你必须更改 chromedriver 的路径，更新用户名和密码字段，并像这样更改 Instagram 的目标。

如果你在设置这个的时候遇到任何问题，或者对如何将它用于个人用途感到好奇，欢迎在下面的评论中提问。