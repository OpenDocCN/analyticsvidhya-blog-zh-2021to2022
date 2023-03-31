# EP5。在我们的 Streamlit Web 应用中添加媒体文件

> 原文：<https://medium.com/analytics-vidhya/ep5-adding-media-files-in-our-streamlit-web-app-74564af03642?source=collection_archive---------1----------------------->

## 您也可以使用 URL 添加图像！！！！

嘿伙计们！！欢迎回到 EP5 的“简化基础”系列。今天，在这篇文章中，我们将了解如何在我们的 Streamlit web 应用程序中添加图像、视频和音频文件等媒体文件，不再赘述，让我们开始吧！！！

![](img/47870281aa38daf1f1d79b1f5c894f78.png)

作者图片

像往常一样，我们将从导入所有需要的包开始。当我们处理 streamlit 时，我们将导入 streamlit，我们还需要导入图像文件，为此我们还将导入 PIL，它代表 Python 图像库，这是一个非常著名的包，当我们要在程序中使用图像时会用到它。

```
import streamlit as stfrom PIL import Image
```

# 添加图像

我们将从导入图像开始。这里最重要的一点是将媒体文件放在原始 python 代码所在的目录中。

首先，我们将创建一个变量“img ”,在这个变量中，我们将使用 PIL 图像模块中的 open()来打开图像。我的 open()将接受一个文件名参数。

```
img = Image.open("st.jpg")
```

下一步是在我们的 streamlit 中显示这个图像，因此，现在我们将使用 streamlit 中的 image()来完成此操作。这个 image()将采用我们存储图像的变量名。即“img”

```
st.image(img)
```

我们也可以直接从网上添加图片。为此，不需要在 image()中传递变量，只需传递图像的 URL。

```
st.image("[https://www.pexels.com/photo/landscape-nature-sky-man-6620743/](https://www.pexels.com/photo/landscape-nature-sky-man-6620743/)")
```

如果你想看这篇文章的视频版本，可以看看这个视频🤗👆。

# 添加视频

要添加视频，我们必须使用 open()并传入视频文件的名称，并使用以二进制模式读取的“rb”模式。稍后我们必须在我们的 streamlit 中显示它，所以，在我们的 streamlit 中使用 video()，并传入保存打开的视频文件的变量。

您也可以使用参数“start_time ”,并传入您必须开始播放视频的秒数。

```
video1 = open("Day83.mp4", "rb") 
st.video(video1)
#st.video(video1, start_time = 25)
```

# 添加音频

要添加音频，我们必须使用 open()并传入音频文件的名称，并使用以二进制模式读取的“rb”模式。稍后，我们必须在我们的 streamlit 中显示它，因此，在我们的 streamlit 中使用 audio()，并传入保存您打开的视频文件的变量。

```
audio1 = open("audio.mp3", "rb")
st.audio(audio1)
```

该系列的 EP4 是一个互动的插曲，在那里我教了如何在我们的 Streamlit web 应用程序中添加小部件，通过博客文章学习这样做可能是压倒性的，所以，请查看这里的 [*视频。*](https://youtu.be/6BgcdfyMpG4)

# 结论

就这样，我们结束了这篇文章。今天，在本文中，我们了解了如何在 streamlit 中添加媒体文件。

别忘了留下你的回答。✌

大家敬请关注！！为了把我的故事发到你的邮箱里，请**订阅我的时事通讯。**感谢您的阅读！不要忘记鼓掌，分享你的回答，并与朋友分享。

这篇文章早些时候发表于的[fit chie](https://fittechie.in/)