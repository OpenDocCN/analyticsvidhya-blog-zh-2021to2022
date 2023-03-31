# 使用 Python 的你自己的人工智能语音助手指南！！

> 原文：<https://medium.com/analytics-vidhya/a-guide-to-your-own-a-i-voice-assistant-using-python-17f79c94704?source=collection_archive---------0----------------------->

![](img/cd074e0a3e0418d3e1f3894d2efbb67d.png)

语音助手是懒人的福音！

你有没有想过，如果你有自己的虚拟人工智能助理(就像 J.A.R.V.I.S 一样)会有多酷，想象一下，不用输入一个单词就可以发送电子邮件，不用打开网络浏览器就可以在维基百科上搜索，以及在一个语音命令的帮助下执行许多其他日常任务，这将会变得多么容易。

在本教程中，我们将学习完全相同的东西，即如何使用 Python 编写和构建自己的人工智能虚拟语音助手。

**在进入教程之前，我们需要了解一个虚拟语音助手可以为我们执行哪些任务？**

*   它可以为你发送电子邮件。
*   它可以为你播放音乐。
*   它可以为你做维基百科搜索。
*   可以打开 Google、YouTube、Stackoverflow、freecodecamp 等网站。，在网络浏览器中。
*   它可以用一个声音命令打开你的代码编辑器或 IDE。

**所有这些事情都可以发生，无需你在浏览器中键入一个字！**

![](img/82712f4e7e1833915cc10ce8612abf42.png)

现在让我们进入我们的语音助手**的实际构建😎**

还有，别忘了事先给你的语音助手取个名字:P

# 设置编码环境:

我用 Pycharm 编写了这个代码，但是请随意使用您喜欢的任何其他 IDE。

**首先，我们将导入/安装必要的库:**

*   pyttsx3
*   日期时间
*   语音识别
*   维基百科（开放式百科全书）
*   网络浏览器
*   os.path
*   smtplib

# 定义朗读功能:

人工智能语音助手首先要做的就是说话。为了让我们的机器人说话，我们将编写一个 **speak()** 函数，它将音频作为输入，并将其发音作为输出。

**def speak(音频):pass**#暂且不说，条件我们以后再写。

现在，我们需要音频来实现用户和助手之间的正常交流。因此，我们将安装一个名为 **pyttsx3 的模块。**

**pyttsx 3 是什么？**

*   这是一个 Python 库，可以帮助我们将文本格式转换成语音格式。
*   此外，它还可以离线工作，并且与 Python 2 和 Python 3 兼容。

**安装:**

```
pip install pyttsx3
```

成功安装 pyttsx3 后，在您的程序中导入这个模块。

**用法:**

```
**import pyttsx3****engine = pyttsx3.init('sapi5')****voices = engine.getProperty('voices')** #gets you the details of the current voice**engine.setProperty('voice', voice[1].id)** # 0-male voice , 1-female voice
```

**什么是 sapi5？**

*   微软语音 API ( **SAPI5** )是微软提供的语音识别和合成技术。
*   它通常有助于声音的合成和识别。

**什么是 VoiceId？**

Voice id 帮助我们选择不同的声音。

*   语音[0]。id = *男声*
*   声音[1]。id = *女声*

# 编写我们自己的 speak()函数:

```
**def speak(audio):** **engine.say(audio) ** **engine.runAndWait()** #Without this command, speech will not be audible to us.
```

# 创建我们的主要功能:

现在，我们将创建一个 **main()** 函数，在这个 **main()** 函数中，我们将定义我们定制的 speak 函数。

```
**if __name__=="__main__" :** **speak('Hello Sir, I am Friday, your Artificial intelligence assistant. Please tell me how may I help you')**
```

**又及:我给我的语音助手起了个名字——星期五😁**

无论您在这个 **speak()** 函数中写什么，都将被完全转换成语音。恭喜你！有了这个，我们的语音助手就有了自己的声音，随时可以说话！

# 编写 wishme()函数的代码:

现在，我们要编写一个 **wishme()** 函数，通过这个函数，我们的语音助手将根据电脑上的时间向我们表示祝愿或问候。

为了向我们的助手提供当前时间，我们需要导入一个名为 **datetime** 的模块。使用以下命令将该模块导入到您的程序中:

```
**import datetime**
```

现在，让我们开始编写我们的 **wishme()** 函数:

```
**def wishme():
   hour = int(datetime.datetime.now().hour)**
```

这里，我们将当前小时或时间的整数值存储到一个名为 hour 的变量中。现在，我们将在 if-else 循环中使用这个小时值。

```
**def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning!")    

    elif hour>=12 and hour<18:
        speak("Good Afternoon!")       

    else:
        speak("Good Evening!")      

   speak('Hello Sir, I am Friday, your Artificial intelligence assistant. Please tell me how may I help you')**
```

# 定义 takeCommand()函数:

对于我们的语音助手来说，下一个最重要的事情是，它应该能够在我们系统的麦克风的帮助下进行指挥。所以，让我们开始编写我们的 **takeCommand()** 函数。

在我们的 **takeCommand()** 功能的帮助下，我们的人工智能语音助手还将能够通过麦克风接收我们的输入，从而返回一个字符串输出。

但是，在定义 **takeCommand()** 函数之前，我们需要安装一个名为 **speechRecognition，**的模块，命令如下:

```
**pip install speechRecognition**
```

![](img/ee252021632e5b7eac3426abfb847d2a.png)

成功安装该模块后，通过编写 **import** 语句将该模块导入到程序中。

```
**import speechRecognition as sr**
```

让我们开始编写我们的 **takeCommand()** 函数:

```
**def takeCommand():
**    #It takes microphone input from the user and returns string output   ** 
     r = sr.Recognizer()
     with sr.Microphone() as source:
         print("Listening...")
         r.pause_threshold = 1
         audio = r.listen(source)**
```

我们已经成功地创建了我们的 **takeCommand()** 函数。现在我们将在程序中添加一个 try 和 except 块来有效地处理我们的错误。

```
**try:
        print("Recognizing...")    
        query = r.recognize_google(audio, language='en-in')** #Using google for voice recognition.
        **print(f"User said: {query}\n")  #User query will be printed.** **except Exception as e:**
        # print(e)  use only if you want to print the error!
        **print("Say that again please...")** #Say that again will be printed in case of improper voice **return "None"** #None string will be returned **return query**
```

现在，我们终于可以开始定义我们的任务，并从它们那里获得想要的输入。

# 任务 1:在维基百科上搜索一些东西:

要进行维基百科搜索，我们需要将 ***维基百科*** 模块安装并导入到我们的程序中。

安装维基百科模块的命令如下:

```
**pip install wikipedia**
```

成功安装维基百科模块后，**通过编写 **import** 语句将**维基百科模块导入到程序中。

```
**if __name__ == "__main__":
    wishMe()
    while True:**
        **query = takeCommand().lower()** #Converting user query into lower case                # Logic for executing tasks based on query
        **if 'wikipedia' in query:** #if wikipedia found in the query then this block will be executed **speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=5) 
            speak("According to Wikipedia")
            print(results)
            speak(results)**
```

在上面的代码中，我们使用了一个 if 语句来检查 **Wikipedia** 是否在用户的搜索查询中。如果在用户的搜索查询中找到了维基百科，那么维基百科页面摘要中的五个句子(你可以通过将句子数量从 5 改为你想要的任何数量来改变)将在 speak 功能的帮助下转换为语音。

# 任务 2:在网络浏览器中打开 YouTube:

要打开任何网站，首先我们需要导入一个名为 ***webbrowser*** 的模块。

它是一个内置的模块，我们不需要用 pip 语句来安装它，我们可以通过编写一个 import 语句直接将其导入到我们的程序中。

**代码:**

```
**elif 'open youtube' in query:
            webbrowser.open("youtube.com")**
```

这里，我们使用 elif 语句来检查 YouTube 是否在用户的查询中。让我们假设，用户给出一个命令，比如“请打开 YouTube。”

因此，open YouTube 将出现在用户的查询中，elif 条件将为真，因此代码将被执行。

# 任务 3:在网络浏览器中打开谷歌搜索引擎:

```
**elif 'open google' in query:
            webbrowser.open("google.com")**
```

我们在网络浏览器中打开谷歌，使用的逻辑与我们打开 YouTube 时使用的相同。

# 任务 4:播放音乐:

要播放音乐，我们需要导入一个名为 **os** 的模块。使用 Import 语句直接导入此模块。

```
**elif 'play music' in query:
            music_dir = 'music_dir_of_the_user'
            songs = os.listdir(music_dir)
            print(songs)    
            os.startfile(os.path.join(music_dir, songs[0]))**
```

在上面的代码中，我们首先打开用户的**音乐目录**，然后在 **os 模块**的帮助下列出目录中所有的歌曲。

在 **os.startfile** 的帮助下，你可以播放任何你选择的歌曲。您还可以借助**随机**模块播放一首随机歌曲。每当你命令播放音乐时，人工智能语音助手将从歌曲目录中播放任意一首歌曲。

# 任务 5:了解当前时间:

```
**elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")    
            speak(f"Sir, the time is {strTime}")**
```

在上面的代码中，我们使用了 **datetime()** 函数，并将系统的当前时间存储到一个名为 **strTime** 的变量中。

将时间存储在 **strTime** 中后，我们将该变量作为 speak 函数中的一个参数进行传递，因此，时间字符串将被转换为语音。

# 任务 6:打开 Stackoverflow:

```
**elif 'open stack overflow' in query :**  **webbrowser.open('stackoverflow.com')**
```

我们使用了与谷歌搜索引擎和 YouTube 相同的术语。

# 任务 7:打开 freecodecamp:

```
**elif 'open free code camp' in query :**  **webbrowser.open('freecodecamp.org')**
```

我们使用了与谷歌搜索引擎和 YouTube 相同的术语。

# 任务 8:打开 Pycharm(或任何 IDE):

```
**elif 'open code' in query:
            codePath = "/Applications/PyCharm CE.app"** #that's the code path. **os.startfile(codePath)**
```

要打开 Pycharm IDE 或任何其他应用程序，我们需要应用程序的代码路径。

![](img/d1f3aca286c3d53180e198d775fec912.png)

# 任务 9:发送电子邮件:

要发送电子邮件，我们需要导入一个名为 *smtplib* 的模块。

**什么是 smtplib？**

*   **简单邮件传输协议(SMTP)** 是一种允许我们发送电子邮件以及在不同邮件服务器之间路由电子邮件的协议。

名为 **sendmail** 的实例方法出现在 **SMTP 模块**中。这个实例方法允许我们发送电子邮件。

它需要/采用 3 个参数:

*   ***发件人:*** 发件人的邮件地址。
*   ***收件人:*** 收件人的邮件。
*   ***消息:*** 需要发送给一个或多个接收人的字符串消息。

现在，我们将创建一个**sendmail()**函数，它将帮助我们向一个或多个收件人发送电子邮件。

```
**def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('youremail@gmail.com', 'your-password')
    server.sendmail('youremail@gmail.com', to, content)
    server.close()**
```

**注意:**不要忘记在你的 Gmail 账户中启用*’****不太安全的应用****’*功能。否则，发送电子邮件功能**将无法正常运行**。

# 在 main()函数中调用 sendEmail()函数:

```
**elif 'email to receiver's name)' in query:
            try:
                speak("What should I say?")
                content = takeCommand()
                to = "receiver's email id"    
                sendEmail(to, content)
                speak("Email has been sent!")
            except Exception as e:
                print(e)
                speak("Sorry sir. I am not able to send this email")**
```

我们使用 try and except 块来处理发送电子邮件时可能出现的任何错误。

现在，是时候复习或回顾我们到目前为止所学的内容了！

*   首先，我们创建了一个 **wishme()** 函数，根据系统时间提供问候用户的功能。
*   在 **wishme()** 函数之后，我们创建了一个 **takeCommand()** 函数，它可以帮助我们的人工智能语音助手从用户那里接受命令，并相应地发挥作用。这个函数还负责以字符串格式返回用户的查询。
*   我们开发了打开不同网站的代码逻辑，如谷歌搜索引擎、YouTube、Stackoverflow、freecodecamp、维基百科等。
*   我们还开发了用于打开 Pycharm IDE 和其他一些应用程序的代码逻辑。
*   最后，我们开发了发送电子邮件的功能(这也不需要输入一个字，是不是很神奇！😵)

那么现在最矛盾的问题来了，这真的是人工智能吗？

技术上来说不是，因为这个语音助手只是一堆语句的结果。但是，如果我们深入研究人工智能的科学/目的，它的主要目标只是减少人类的努力，并以与人类相同的效率(甚至更好)执行任务。

而我们的语音助手在相当程度上解决了这个目的。

所以最后的结论是，**它是一个人工智能**😛！

**结束了！！！**

恭喜各位！🎉

我们已经成功打造了我们的个人 A.I .虚拟语音助手，成功向前迈进了一步，为我们的懒惰加油！！😂

![](img/afd42e4b1cd73b54b438d958d7e1ba4f.png)

我希望你们都喜欢这个博客！

你也可以看看我的 [Github](https://github.com/saurav935/AI-voice-assistant/blob/master/desktop%20assistant.py) 库，以便更好地理解代码。

也不要忘了给一些掌声(你想给多少就给多少😉).