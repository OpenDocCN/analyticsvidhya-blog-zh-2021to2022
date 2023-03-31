# Git 101 —第 1 部分

> 原文：<https://medium.com/analytics-vidhya/git-101-part-1-251fe5fb2c0a?source=collection_archive---------19----------------------->

## git 基本概念的实用介绍。

![](img/4cfbeecfbabf6acadd2c219cc371e996.png)

照片由[扬西·敏](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Git 是一个 DVCS: *分布式版本控制系统*。

基本上，有了 git，你和你的团队可以合作创建、修改、扩展、维护在同一个代码库上并行工作的代码，而不用担心“破坏”代码。

当您想使用 git 开发一个项目时，您可以创建一个存储库——一个包含项目代码的目录和一个 git 子目录，git 自己使用这个子目录来跟踪所有的东西。

注意:为了简单起见，存储库通常被称为 **repo** 。

初始化文件夹的命令是:

```
git init 
```

初始化当前文件夹，或者

```
git init <directory_name>
```

初始化特定目录。

# 跟踪文件

Git 不会自动跟踪回购中的文件，你必须告诉他你想要它。为此，请使用以下命令:

```
git add <filename>
git add <directory>
```

注意:向 git 添加一个目录并不意味着 git 会添加该目录中包含的文件，您必须稍后再添加，或者使用如下命令:

```
git add <directory>/*
```

自动将目录中包含的所有文件添加到 git 后面的目录中。

要添加您创建的所有文件，请使用:

```
git add .
```

# 承诺

git 的基本变化单位是**提交**。提交是您的回购在特定时间点的快照。准确地说，提交并不强迫你“保存”你所做的所有更改。提交时，选择要提交的文件。
使用以下命令进行提交:

```
git commit <files blank-separated> -m 'what changed'
```

-m 选项是一个快捷方式，用于添加提交中出现的更改的简短描述(消息)。默认情况下(这是一种好的做法)，您必须向提交提供一条消息，但是您可以使用以下命令省略它:

```
git commit <files> --allow-empty-message -m ''
```

提交所有已更改文件的有用快捷方式:

```
git commit -a -m 'message'
```

# 第一部分结论

我认为这对于本系列的第一部分已经足够了，并且对于您开始在本地试验您的项目已经足够了。如果一开始你感到无所适从，或者对其背后的机制不够了解，或者经历了其他类型的问题，请不要担心，这是正常的。随着时间和经验的积累，你会进步的。

在接下来的文章中，我们将更深入地介绍提交和 git 机制，我们将介绍分支和合并的基础知识，稍后还将介绍如何使用 GitHub 在分布式环境中工作。

感谢阅读！