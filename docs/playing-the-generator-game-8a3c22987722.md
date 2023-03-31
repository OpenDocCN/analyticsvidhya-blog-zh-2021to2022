# 玩发电机游戏

> 原文：<https://medium.com/analytics-vidhya/playing-the-generator-game-8a3c22987722?source=collection_archive---------14----------------------->

不久前，我写了一篇文章

 [## 我是如何误解龙目岛的

### 我当时正在做一个项目，我写了一些代码，我希望能很好地工作。

开发到](https://dev.to/satylogin/how-i-misunderstood-lombok-1k44) 

所以这一次，我不想犯同样的错误，这让我们回到这个帖子。

先给熟悉`lombok`和`dependency injection`的人做一个简单的练习。您认为下面的代码块应该生成什么样的代码

我想大多数人会猜测(看过 lombok 生成的代码的人)这将转化为

但是如果是这样的话，我们就不会在这里了，不是吗……
所以让我们看看它实际上会产生什么

但奇怪的是，我可以清楚地看到我已经用@Named 注释了我的字段，如果在依赖项注入期间没有名称发现，那么它如何找到要注入的正确依赖项呢？

事实证明，lombok 并不尊重所有的注释，因为实现这种行为太复杂了。你可以通过 github 问题了解详情[1](https://github.com/rzwitserloot/lombok/issues/1634)2

所以现在我们知道这是事实，问题是如何解决这个问题。

1.  在 lombok 的新版本中，可以在`lombok.config`文件中添加`lombok.copyableAnnotations += com.google.inject.name.Named`，lombok 会在生成构造函数时尝试复制它们。
2.  使用我们老式的基于构造函数的注入和编写

我更喜欢 2，因为有几个原因

1.  在 lombok.config 中放一个 conf 来决定复制什么隐藏了太多的信息，坦白地说，这只是一个等待发生的意外，当有人看到这个代码样本并试图复制它时，它以某种方式工作，因为没有冲突。
2.  它只复制已经为它们实现了 copyable 的注释，所以万一我们遇到不可复制的东西，我们还是要实现 2。
3.  我喜欢第二种，因为它更具表达性，我喜欢编写更具表达性的代码，因为最终我们是为人类编写它们，如果我们不隐藏太多东西并保持简单，以便阅读代码的人可以更多地关注业务逻辑，而不是花时间理解他们的注入为什么或为什么不工作，那会更好。

*原载于 2021 年 3 月 26 日*[*https://dev . to*](https://dev.to/satylogin/playing-the-generator-game-2bk4)*。*