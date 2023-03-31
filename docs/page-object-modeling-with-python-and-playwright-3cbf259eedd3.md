# 用 Python 和剧作家进行页面对象建模

> 原文：<https://medium.com/analytics-vidhya/page-object-modeling-with-python-and-playwright-3cbf259eedd3?source=collection_archive---------2----------------------->

## 如何构建有效的剧作家框架模型？

![](img/7bf8d9887b34cd64d6aa24691df77bc8.png)

克里斯·里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

微软的剧作家在 2020 年 5 月悄悄发布。由最初编写木偶库的一群工程师编写，剧作家有意与木偶师相似。虽然微软在剧作家的文档中提供了一个 page 对象的例子，但是这个例子相当简单，没有深入研究 page 属性或 helper 方法的使用。

# 什么是页面对象？

页面对象是使用编程语言对网页的抽象。目的是表示代码中的所有页面，以便对特定元素采取行动。页面对象通常用于测试自动化领域，质量工程师创建对象和测试以测试应用程序用户旅程。

本教程将着重于为 DemoQA 书店应用程序登录页面构建一个页面对象。登录页面具有用户名输入、密码输入、登录按钮和新用户按钮。

# 入门指南

为了开始，我们需要安装我们的依赖项。应该安装以下软件包:

*   pytest
*   pytest-剧作家
*   剧作家

只需使用 pip(或您选择的依赖管理器)安装以上程序。

包含了`pytest`库，因为它是 Python 最流行的测试运行程序。`pytest-playwright`包提供了在测试中使用的简单夹具，比如自动化浏览器设置的页面夹具。它也可以被配置成在失败时截屏，这有助于调试 flakey 或失败的测试。

# 抽象页面元素

我们的第一个任务是创建一个表示登录页面的类对象，该页面在初始化时具有一个`page`属性。在创建属性和方法时，将使用`page`属性。

构建类对象的原因是它允许我们的代码在增加可重用性的同时保持有序。

```
class Login:
    def __init__(self, page):
        self.page = page
```

现在我们有了 Login 类，我们可以开始添加表示页面中元素的属性。我们使用`@property`内置装饰器，因为它使得在 Python 中使用 getters 和 setters 更加容易。

每个属性返回剧作家所说的`ElementHandle`。这是剧作家对 DOM 元素的表现。对象可以单独使用，也可以作为其他方法的参数。

```
class Login:
    def __init__(self, page):
        self.page = page @property
    def new_user_button(self):
        return self.user_form.wait_for_selector("#newUser") @property
    def password_field(self):
        return self.user_form.wait_for_selector("#password") @property
    def submit_button(self):
        return self.user_form.wait_for_selector("#login") @property
    def user_form(self):
        return self.page.wait_for_selector("#userForm") @property
    def username_field(self):
        return self.user_form.wait_for_selector("#userName")
```

属性`username_field`是从`user_form`中调用的。这是因为用户名字段是 DOM 中用户表单的子元素。这是一个很好的实践，因为它将驱动程序的范围缩小到一个特定的区域，而不是整个页面。

剧作家的`wait_for_selector`方法允许我们安全地调用元素，因为它会等待元素在 DOM 中可见，直到达到超时限制。如果在超时限制内没有找到选择器，将引发错误。

`wait_for_selector`方法也可用于等待特定的元素状态，例如等待直到隐藏。但是，这超出了本指南的范围。

# 编写助手方法

页面对象属性可用于构建助手方法。应该为经常重复的工作流构造助手方法。

```
class Login: def submit_login_form(self, user):
        self.username_field.fill(user["username"])
        self.password_field.fill(user["password"])
        self.submit_button.click() def navigate(self):
        self.page.goto("https://www.demoqa.com/login")
```

上述方法允许页面导航和表单提交，这两个工作流在登录页面测试期间会经常使用。

# 编写和运行测试

现在我们有了一个用属性和助手方法构建的 page 对象，我们可以开始为用户之旅编写测试了。

```
from pages import Loginuser = {
    "username": "test",
    "password": "password"
} class TestLogin:
    def test_valid_login(self, page):
        login_page = Login(page)
        profile_page = Profile(page) login_page.navigate()
        login_page.submit_login_form(user) visible = profile_page.username_value_field.is_visible() assert visible and "profile" in page.url
```

在上面的测试中，我们初始化了登录页面和测试断言中使用的配置文件页面(示例未显示)。我们导航到登录页面，然后使用`submit_login_form`助手方法输入用户信息。

提交表单后，我们检查 Profile 页面上是否显示了某个元素。然后我们在断言中使用这个检查，因为这个元素在成功登录后显示。

要运行此测试，请在终端上输入以下命令之一:

*   无头地跑
*   精神饱满地奔跑

# 项目目录

完成本教程后，您的目录应该如下所示:

```
pages
    |__ __init__.py
    |__ login.pytests
    |__ test_login.py.gitignore
README.md
requirements.txt
```

# 摘要

使用页面对象模型编写测试相当快速和方便。使用 Python 和剧作家，我们可以毫不费力地将网页抽象成代码，同时自动等待元素。

Jonathan Thompson 是一名高级质量工程师，专门研究测试自动化。他目前和妻子以及一只名叫温斯顿的金毛猩猩住在北卡罗来纳州的罗利。你可以在 [LinkedIn](https://www.linkedin.com/in/jonathanmnthompson/) 上和他联系，也可以在 Twitter([*@ jacks _ where*](https://twitter.com/jacks_elsewhere))*或 Github([*Thompson jonm*](http://github.com/ThompsonJonM))上关注他。*