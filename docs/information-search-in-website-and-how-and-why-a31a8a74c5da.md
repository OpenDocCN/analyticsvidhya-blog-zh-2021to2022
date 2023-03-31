# 网站中的信息搜索以及如何和为什么

> 原文：<https://medium.com/analytics-vidhya/information-search-in-website-and-how-and-why-a31a8a74c5da?source=collection_archive---------10----------------------->

![](img/2cd03ef56967de1315866ee69e53def5.png)

**搜索一些东西并不那么困难**

我喜欢在网站上寻找情感分析和自然语言处理实验的信息。但是在网站上找到信息有更多的方法。
我们可以从一个普通的例子开始，我的个人页面，每个程序员对一个 html 页面是如何工作的都只有最低限度的了解。

**测试的目标是找出我知道哪些编程语言:**

1.  从网站提取我们的目标页面，在这种情况下，我们可以很容易地用 python 执行 get 请求。
    在尝试之前，如果您没有安装请求模块，请尝试使用

```
python -m pip install requests  #invoke module in python version 2.xpython3 -m pip install requests #invoke module in python version 3.xpip3 install requests #or use pip for python 3pip install requests #in other case only pip
```

2.将这些代码行数字化:

```
import requests
url_target = "[https://andrewraieta.github.io/](https://andrewraieta.github.io/)"
res = requests.get(url_target)
page_text = res.text
```

3.现在我们获得了我的个人页面的 html 代码副本:

```
<!DOCTYPE html>
<html lang="en-us"><head>
        <meta name="generator" content="Hugo 0.87.0" />
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta name="description" content=""> <title>
            Hello
    </title><link rel="stylesheet" href="/css/style.css"><link rel="stylesheet" href="[https://fonts.googleapis.com/css?family=Montserrat:400,700](https://fonts.googleapis.com/css?family=Montserrat:400,700)">
    <link rel="stylesheet" href="[https://cdn.rawgit.com/jpswalsh/academicons/master/css/academicons.min.css](https://cdn.rawgit.com/jpswalsh/academicons/master/css/academicons.min.css)" crossorigin="anonymous">
    <link rel="stylesheet" href="[https://use.fontawesome.com/releases/v5.8.1/css/all.css](https://use.fontawesome.com/releases/v5.8.1/css/all.css)" integrity="sha384-50oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf" crossorigin="anonymous"><link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
    <link rel="manifest" href="/site.webmanifest"><link href="/index.xml" rel="alternate" type="application/rss+xml" title="Hello" /></head>
<body><main>
    <div class="block introduction">
        <div class="column left">
            <img src="images/portrait.jpg" class="portrait" alt="Portrait" />
        </div>
        <div class="column right"><h1>Hello.</h1><h2>I am Andrew.</h2>
            <p>I am a computer science student at La Sapienza University and in my spare time I coding a lot and I learn new languages and technologies.  <br>
<br>
Technical Skill I'm comfortable with:<ul>
    <li>Languages : Java, Python</li>
    <li>Frameworks/Tools: Flask, Heroku, MySql and Git</li>
    <li>Software Development:
        <li>IDE: Spyder, Eclipse, Intellij , Vscode, Pycharm</li>
        <li>Tools: Git, sublime merge, anaconda, maven</li>
        <li>OS: Unix, Windows, OSx</li>
    </li>
    <li>Good Mathematical Background</li>
</ul></p><div class="links"><a rel="me" href="[andrewraieta@gmail.com](mailto:andrewraieta@gmail.com)" title="E-mail">
                        <span class="fas fa-envelope"></span>
                    </a><a rel="me" href="[https://github.com/andrewraieta](https://github.com/andrewraieta)" title="Github">
                        <span class="fab fa-github"></span>
                    </a><a rel="me" href="[https://medium.com/@andrewraieta](/@andrewraieta)" title="Medium">
                        <span class="fab fa-medium"></span>
                    </a><a rel="me" href="[https://www.linkedin.com/in/andrea-raieta-a77085210/](https://www.linkedin.com/in/andrea-raieta-a77085210/)" title="Linkedin">
                        <span class="fab fa-linkedin"></span>
                    </a></div>
        </div>
    </div></main><footer>
    <div class="column"></div>
    <div class="column"></div>
</footer></body>
</html>
```

> 我们注意到有一个包含我们信息的
> 
> **bs4**

现在，如果您从未使用过它，开始时 bs4 的工作方式听起来有点奇怪，但请记住，bs4 接收一个表示目标 html 页面副本的字符串，并解析(逐步分析字符串)页面以找到感兴趣的信息。

```
from bs4 import BeautifulSoup
soup = BeautifulSoup(page_text, "html.parser") #create an instance of an object that contain information of webpage and it's content(webpage itself)
soup.prettify #print a formatted version of page
```

我们可以使用 find_all()方法，并返回所有

*   作为字符串。我们可以直接获得所有的
*   列表:

```
li_element_list = soup.find_all("li")
for idx in li_element_list:
    print(idx)Output:0 <li>Languages : Java, Python</li>
1 <li>Frameworks/Tools: Flask, Heroku, MySql and Git</li>
2 <li>Software Development:
        <li>IDE: Spyder, Eclipse, Intellij , Vscode, Pycharm</li>
<li>Tools: Git, sublime merge, anaconda, maven</li>
<li>OS: Unix, Windows, OSx</li>
</li>
3 <li>IDE: Spyder, Eclipse, Intellij , Vscode, Pycharm</li>
4 <li>Tools: Git, sublime merge, anaconda, maven</li>
5 <li>OS: Unix, Windows, OSx</li>
6 <li>Good Mathematical Background</li>
```

好了，现在我们感兴趣的是列表中的索引 0:

```
**0 <li>Languages : Java, Python</li>
we isolate in a variable:
language = li_element_list[0]
type(language)
>> bs4.element.Tag**
```

这意味着我们在 bs4 模块的标签类型下有一个元素，要提取它，我们有两种方法，第一种方法是硬编码，只提取与编程语言名称列表进行比较的单词，另一种方法是利用标签属性文本或内容:

```
A little explaination of attribute in bs4.element.Tag
if we print the content of language variable we obtain:language.contents 
Output: ['Languages : Java, Python'] #as list--------------------------------------else we use textlanguage.text
Output: 'Languages : Java, Python' #as list 
```

我们可以使用文本属性并提取 Java 和 python 标记。
很简单我们把字符串拆分成一个字符串列表:

```
split_language = language.split(" ")
and obtain **['Languages', ':', 'Java,', 'Python']**Before the final step we clean the java string from comma character
we simply write:lang[0] = "Java" 
Output: **['Languages', ':', 'Java', 'Python']**
```

最后一步，我们把 split_language 列表转换成 set，并在它上面使用一个 intersect 函数，把它与一组语言名称进行比较。

```
split_language = set(split_language) **{':', 'Java', 'Languages', 'Python'}**#and crate a set of language name
LANGUAGE_SET_NAME = {'C', 'C++', 'CPP', 'Go', 'Java', 'Python', 'Rust'}we can use intersect **language = lang.intersection(LANGUAGE_SET_NAME)**
Output:
**{'Java', 'Python'}**
```

现在，如果我们的目标不仅仅是提取语言，而是创建一种程序员的身份证，我们可以定义一个字典并复制信息，我们可以用以下方法提取程序员的姓名:

```
target_name = soup.find("h2")
target_name.text.strip('.,')
LANGUAGE_NAME = { ALL NAME EXIST IN A CERTAIN LANGUAGE..... }
target_name = set(target_name)
name = LANGUAGE_NAME.intersection(target_name)
```

最后一步，我们创建一个字典来存储信息:

```
programmer_informations = {} #initialize a dictionary
programmer_informations[name] = language
{'Andrew': 
    {'Java', 'Python'},
}
```

好了，最终我们达到了我们的目标，但这是结束吗？数据科学家在网站上寻找信息，却发现一个人知道哪些编程语言，这种情况可能并不少见。但是他可以用它来创建一个潜在候选程序员列表，这些候选程序员只懂 Java，或者知道某个云系统 AWS。

# **结论**

**为什么寻求信息很重要？**

当然，上面显示的方法并不是最有效的，但是对新手来说是有效的，所以让他们清楚地知道用几行代码可以做什么。原因很简单，知道如何捕捉信息并以 2021 年最有用的方式进行分类是知识的来源，数据就像数字黄金。

**怎么做？**

嗯，这并不像我们作为例子使用的页面那样简单，通常当我们有一个具有相同 html 结构的数千个页面的列表时，就需要使用这种方法，因为没有绝对的语法识别器来识别页面之间的差异。这是因为每个页面都是由不同的自动网站建设者，网络程序员生成的，所以我们有一个未定义的和非有限的结构系统。