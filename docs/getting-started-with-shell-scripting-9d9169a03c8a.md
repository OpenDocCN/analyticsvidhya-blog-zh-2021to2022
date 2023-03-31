# Shell 脚本入门

> 原文：<https://medium.com/analytics-vidhya/getting-started-with-shell-scripting-9d9169a03c8a?source=collection_archive---------3----------------------->

嘿，大家好！在这个博客中，我们将了解一些我们感兴趣的东西: **Shell 脚本**。

![](img/9c6bc053ececba09abd00b88b8a13bdd.png)

[赛·基兰·阿纳加尼](https://unsplash.com/@_imkiran?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/linux?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

还记得在电影中我们看到黑客在键盘上嘎吱嘎吱地敲着键，在黑屏上敲着什么吗，那感觉有多酷，不是吗？所以我们先来了解一下到底什么是 Shell 脚本。

通常有两种方式可以与任何操作系统交互

1.  图形用户界面或 GUI
2.  命令行界面或 CLI

GUI 是我们大多数人使用的，它有一个图形视图，在鼠标的帮助下，你可以与操作系统进行交互。

CLI 是当有一个叫做终端的黑屏(而不是 GUI)时，你在终端中键入命令来做你想做的任何事情。

> 如果有人想知道哪一个功能更强大，那么答案无疑是 CLI，因为在 CLI 中您有更多的自由和访问权限，您可以键入自己定制的命令来访问任何内容和执行任何操作。

Shell 脚本由两个词组成，Shell 和脚本。那么它们的意义是什么呢？

## 壳牌是什么？

所以当你在终端中输入命令时，一定有某个程序接受这些命令，运行它们，并在黑屏上打印输出，对吗？这个程序被称为 **Shell** 。

市场上有各种类型的 shell，如 Bourne shell (sh)、C shell (csh)、TC shell (tcsh)、Korn shell (ksh)、Bourne Again SHell (bash)。但是用的比较多的是 Bourne shell **(sh)** 和 Bourne Again SHell**(bash)**。Bash 就像是 sh 的升级版。在 windows 中，我们有 PowerShell。虽然 shell 脚本在 Linux 中更受欢迎。

## 什么是脚本？

现在，可能有一些活动需要您重复执行，并且每次为它们键入命令会令人厌倦，因此您可以将所有这些命令聚集在一个文件中，然后执行该文件来自动化您的任务/活动，这被称为**脚本**。该文件使用的扩展名是“”。测试

> 因此，所有的 shell 脚本实际上就是将所有需要的命令组合在一起，并对它们应用逻辑，以形成一个良好的工作流，从而自动化您的任务。 *:)*

所以让我们来学习 Shell 脚本。但是首先你需要知道一些基本的 Linux 命令。所以，看看下面在 Linux 终端中经常使用的命令。

```
**Pwd :** Current Directory**mkdir :** Create new directory**cd :** Change Directory**Ls :** Lists all files in a Directory**ls -la :** Lists all files with hidden files**Ls -L** : Lists files with size & other details**touch** : Create an empty File**mv** : Move/rename a file**cp** : Copy a file**cp -r :** Copy a directory**rm** : Delete a file**rm -r :** Delete a directory**more** : Read the whole file content**tail** : Read the last 10 lines, usually for log files**grep** : Find pattern or text inside a file**history** : History of recently typed commands**top** : lists top 10 processes consuming memory**comm** : Lists files with details**df & du** : Check disk free space & disk used**date** : Shows current date/time**uptime** : Shows current uptime**finger user** : Displays information about
```

Shell 脚本就像任何其他编程语言一样，我们在其中使用与任何其他编程语言相同的逻辑。

我们可以在里面写**函数**、 **if-elif-else** 条件、 **case、**当然还有**循环**。

现在我们来看两个 shell 脚本的用例，这样我们就可以在其中应用一些逻辑，并且边做边学。

**A)创建一个函数，该函数将来自用户的输入作为名称，并问候他们。**

```
#!/bin/bashecho "Hey, there! What is your name?"read nameecho "Nice to meet you, $name"
```

这里' **#！**被称为 SheBang 或 HashBang。它用来告诉操作系统在运行这个脚本时使用哪个解释器。这里我们告诉它使用 bash。(/bin/bash 是 bash 的路径)

然后，我们使用' **echo** 来询问用户的姓名，稍后使用' **read** 来捕获用户的输入。

最后，我们打印一条消息“很高兴见到你，$name ”,其中 **'$'** 用于告诉我们的脚本‘name’是一个变量，而不是字符串。

问题陈述:-创建一个 IP 清除器，它将清除网络中所有活动设备的 IP

这很容易做到。我们只需要运行一个 for 循环，并对给定网络中的每个 IP 执行 **ping** 操作，如果该 IP 是活动的，那么我们的输出将是这样的-" `64 bytes from 200-147-67-142.static.uol.com.br (200.147.67.142): icmp_seq=20 ttl=241 time=253 ms`"

因此，活动的 IPs 将有“64 字节”，所以我们将使用它来对活动的 IPs 输出进行 grep

```
#!/bin/bashif ["$1" == ""]
then
echo "Oops! You forgot an IP Address"
echo "Syntax: ./ipsweep.sh" 192.168.4"else
for ip in `seq 1 254`; do
ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &
done
fi
```

然后我们使用' **cut** '命令从上面的句子中选择第四个字段，即`(200.147.67.142):`

在这之后，我们使用' **tr** '命令去掉 IP 末尾的冒号(:)

最后，我们使用了' **&** '，这样所有的进程都会在后台同时运行，并且会快速执行。

如果你不在最后使用' & '，那么这个过程将会变得非常慢。通过首先运行不带' & '的脚本，然后再运行带' & '的脚本，您可以自己看到不同之处。

现在，我们在一个 for 循环中运行这些命令，该循环也在 if-else 条件中运行，以检查用户是否提供了网络范围字段。如果没有给出，那么脚本会在终端上打印“哎呀！您忘记了一个 IP 地址”并使用正确的语法来运行脚本。

就这样。您已经成功完成了本节 shell 脚本课程！我希望现在您对什么是 shell 脚本至少有一点点了解，或者至少对它感兴趣以了解更多。

所以这就是我的博客，我会在下一篇博客中与你见面。(附:我知道这不是 youtube 视频，但我喜欢这么说。:) )

感谢您阅读到这里，如果您有任何疑问，请随时在这里发表评论。