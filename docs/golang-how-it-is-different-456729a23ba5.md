# golang——与众不同

> 原文：<https://medium.com/analytics-vidhya/golang-how-it-is-different-456729a23ba5?source=collection_archive---------8----------------------->

![](img/b60ec44e8c9ffd33484e8f9df54b2185.png)

Golang 易于理解，编译速度更快。围棋中有一个著名的笑话；Golang 是在等待 C++编译时创建的。这似乎是一个笑话，但它是真实的,《golang》的作者之一 Rob Pike 一直在等待 C++代码编译，然后他决定，他再也不能处理它了，于是 golang 诞生了。

Golang 适合工业项目，大多数公司开始转向 golang 和 grpc 服务。简单易懂，易于维护应该是 golang 项目的基础。如果我们用 golang 创建的代码被另一个程序员看了之后很难理解，那就是坏代码的例子，应该使用 go 工具及其强大的包进行重构。

StorieI 在 4 年前开始学习 golang，从那时起，我试图用它来使我的代码简单，同时有效和不稳定地添加功能。在 golang 之前，我自己也在从事 node 和 python 方面的工作。尽管来自 OOPS 背景的程序员试图学习 go，仍然在结构化编程语言中实现面向对象的概念(这没什么错),但是如果它是通过记住 OOPS 而创建的，那么这种语言就不会是同样源自 c 的结构化语言。

golang 不能用作其他 OOPS 语言的一个重要例子是它处理接口的方式。我们都知道接口是作为方法的抽象来使用的。在 golang 中，我们以两种方式使用接口:-一种是作为接口来定义方法的行为，而不是像在其他面向对象语言中那样抽象方法。换句话说，它被用作一个泛型类型来存储和捕获任何原始或复杂数据类型的值。

让我们从 go 中的一个接口示例开始

在上面的例子中，我创建了一个具有 Read 方法的接口，由于它将实现 reader 接口，并且可以捕获任何实现 reader 接口的类型。

为什么我实现了一个 reader 接口，并在 CustomRead 接口中使用它来解释 golang 与 oops 语言在接口方面的不同。在这段代码中，我定义了实现阅读器接口的 CustomRead 接口的行为。

Go 更适合 UNIX 理念，创建小任务来完成更大的任务。试图在 golang 中实现 oops 概念有时会使项目代码变得更加混乱，并且将来难以维护。最好的例子是如何在 golang viz 中定义一个包或函数。阅读器接口或 net/http 包。Http 可以是 net 包的一部分，但在 golang 中，它是作为 net 包中的一个单独的包创建的，只是为了处理 http 部分。

> 下围棋时，要像地鼠那样做

 [## 下围棋时，要像地鼠那样做

### 编辑描述

talks.golang.org](https://talks.golang.org/2014/readability.slide#1) 

golang 的另一个独特之处是，一切都是按值传递的。Golang 创建一个传递给方法的参数副本，并对其进行处理。无论是作为方法的参数还是方法的接收方，一切都被认为是通过值传递的。

上面的代码是一个很好的例子，展示了它通过值传递的一切。上面代码的问题是它不能工作并抛出一个错误:

```
error.go:18:7: cannot use dog (type Dog) as type Animal in argument to Speak:
 Dog does not implement Animal (Speak method has pointer receiver)
```

上述代码将通过将 Dog struct 作为指针而不是值来传递。但是如果我把 cat 作为指针传递，它会工作吗？令人惊讶的是，即使 cat 接收器不是指针类型，它也能工作。

代码背后的原因是通过将 cat struct 作为指针传递，而不是将 dog struct 作为值传递——当将 Dog struct 作为值传递时，将创建一个副本，但它将指向新 Dog struct 值的不同地址，因此出现错误。当将 Cat 作为指针传递时，可以使用为 Cat 提供的地址指针来创建值。

总而言之，在与 golang 合作时，有几点需要考虑

*   接口应该用于行为，而不是抽象。
*   接口被用作空接口中的泛型类型。
*   接口应该包含特定行为的方法。
*   总是接受接口和返回结构。
*   指针类型变量可以调用值类型的方法，但反之则不行。
*   一切都是通过值传递的，即使是方法的接收者。

如果你对 golang 和理解它的架构有任何疑问。给我发关于 himanshusdec8@gmail.com 的邮件

关注[https://golang-geek.medium.com/](https://golang-geek.medium.com/)了解最新消息…