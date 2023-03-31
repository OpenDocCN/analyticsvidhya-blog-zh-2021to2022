# EP2。Streamlit 中的文本功能

> 原文：<https://medium.com/analytics-vidhya/ep2-textual-functions-in-streamlit-daad2630cd30?source=collection_archive---------10----------------------->

## 写()—高人一等的家伙！！！

嘿！！！伙计们欢迎回来。今天在这篇文章中，我将带您进入 streamlit，并向您展示如何使用内置的文本函数。没有更多的麻烦了！！让我们开始吧。

如果你不知道什么是 streamlit，我们为什么要使用 streamlit，以及如何设置 streamlit。请务必查看下面的视频😄。

现在让我们从 EP2 开始🎉。 ***我们将在这个博客*** 中讨论什么

```
1) Text
2) Header
3) SubHeader 
4) Title
5) Markdown
6) Colorful Bootstrap Text
7) Write — The Superior Guy
```

![](img/23ca8bdee5389a1ee6f92053f3a6e2ae.png)

按作者

# 文本

首先，我们将从一些基本的文本开始。正文是手稿、书籍、报纸等的主要内容。

为了在我们的 web 应用程序中使用文本，我们将使用函数 text()。

```
st.text("Hey this is a text")
```

我们也可以使用 python 中的格式函数。

```
a = "sreeram"st.text("this is {}".format(a))
```

# 页眉

现在让我们继续看**头球**。标题是那些代表介绍性内容的元素，基本上是一组介绍性或导航性的帮助。

为了在我们的 web 应用程序中使用文本，我们将使用函数 header()。

```
st.header("This is a header")
```

# 副标题

**副标题**基本上是紧跟在主标题之后的附加标题，即我们的页眉或标题。

为了在我们的 web 应用程序中使用文本，我们将使用 subheader()函数。

```
# Sub headerst.subheader("This is a subheader")
```

# 标题

标题基本上是描述特定对象的词，标题是我们 streamlit 中可用的最大字体大小。

为了在我们的 web 应用程序中使用文本，我们将使用函数 title()。

```
# Titlest.title("This is a Title")
```

# 降价

markdown 是我们在编码时使用的一段文本，它有助于我们的同行在查看代码时理解代码。

**这个减价功能**的特点是我们可以创建所有不同的字体大小。也就是说，在我们的函数中有一个“#”，我们可以创建一个与我们的标题相等的字体大小，如果我们不使用任何“#”，它将只是一个简单的文本。

为了在我们的 web 应用程序中使用文本，我们将使用函数 markdown()。

```
# Markdownst.markdown(“This is a markdown”) # size of a simple text
st.markdown(“# This is a markdown”) # Size of a title
st.markdown(“## This is a markdown”) # Size of a header
st.markdown(“### This is a markdown”) # Size of a sub header
```

你们想想这些功能通常如何在后端工作，并体验观看这部分视频。

# 彩色引导文本

在 success()、error()、warning()和 info()等函数的帮助下，我们可以创建一些丰富多彩的引导文本，例如👇。

![](img/52fa8d33494ed24c35d1f2dc633721c3.png)

```
st.success("Hurray") # Green colored success message
st.warning("This is a warning") # Orange Colored warning message
st.info("This is an advice") # Blue Colored Info message
st.error("You made a mistake") # Red Colored error message
```

# 写

最后，我们将以 write()结束。这是很酷的一个，我认为这是最好的一个。

这是因为通过这个函数，我们可以将它用作降价函数，而且我们还可以使用这个特定的函数来执行数学运算以及创建数据帧。

这不是很有趣吗？现在让我告诉你怎么做。

```
st.write(“You are on earth”) # size of a simple text
st.write(“# You are on earth”) # Size of a title
st.write(“## You are on earth”) # Size of a header
st.write(“### You are on earth”) # Size of a sub headerst.write(300*3) # Performing Mathematical Operations# Using write() to Create a dataframest.write(pd.DataFrame({"Name": ["Adith", "Sreeram"], 
"age":[17,18]}))
```

# 结论

就这样，我们结束了这篇文章。今天，在本文中，我们了解了如何使用我们的 streamlit 中提供的内置文本函数。

别忘了留下你的回答。✌

大家敬请关注！！为了把我的故事发到你的邮箱里，请订阅我的时事通讯。感谢您的阅读！不要忘记鼓掌，分享你的回答，并与朋友分享。

这篇文章早些时候发表在的[菲特奇](https://fittechie.in/)