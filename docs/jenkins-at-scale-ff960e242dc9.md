# 詹金斯秤

> 原文：<https://medium.com/analytics-vidhya/jenkins-at-scale-ff960e242dc9?source=collection_archive---------34----------------------->

2021 年 1 月 16 日

![](img/772e30cbd91f8094b126a7ec92cd0e6d.png)

(*原载于*【https://ish-ar.io/jenkins-at-scale/】*[)](https://ish-ar.io/jenkins-at-scale/)*

# 介绍

本文介绍了一个大规模运行基于 CI/CD Jenkins 平台的解决方案。
要大规模部署和管理 Jenkins，自动化至关重要。更重要的是，自动化应该帮助你，而不是让你的生活更困难。对于詹金斯来说，自动化整个设置可能是痛苦的；通常，工程师会创建一些简单的解决方案，或者按照手动步骤让它“正常运行”。
显然，我们希望尽可能避免这些情况。

# 原则

当我想到创建一个具有弹性和可伸缩性的 CI/CD 平台时，我决定列出一些我希望在实现过程中遵循的原则/要求。

要求:

*   平台的供应应该尽可能简单和快速。
*   自动处理多个微型平台比超级平台更好。
*   应将管道定义为代码并进行测试。
*   CI/CD 平台基础设施应该被定义为代码并被自动测试。
*   CI/CD 平台必须安全。
*   工程师应该乐于打破常规，因为他们知道他们总是可以回滚/重新配置整个堆栈。
*   CI/CD 平台必须导出关于自身的指标，工程师应该使用这些指标来做出决策。

现在，所有这些方面可能看起来非常高级，事实上，他们是，但最好从我们想要什么的清晰想法开始，然后设计“如何执行”。

# 演示

对于本文，我创建了一个小的[演示](https://github.com/ish-xyz/jenkins-aws-platform)来说明如何在 AWS 上自动配置 Jenkins 并进行大规模管理。在这一部分，我将讨论在演示中所采取的决策和实现的工具。
虽然演示并不代表生产就绪平台，但这是一个很好的起点，它展示了有用的良好 Jenkins 特性和最佳实践。
在开发演示时，我遵循了上面列出的要求，尽管我没有实现所有的要求。

# 詹金斯组件

先说詹金斯高层组件。我们可以说，詹金斯有两大类组成部分:主和代理。
**代理**(也称为构建机器)负责运行我们的管道。
而**詹金斯大师**则负责:编排、用户管理、认证、授权、插件管理以及许多其他事情。
基础设施的这两个部分的设置可能非常标准，也可能非常复杂。然而，我今天要向您展示的内容将为您提供自动化整个 Jenkins 设置的基础，并允许您大规模运行它，无论设置有多复杂。

# 履行

让我们来谈谈我在我创建的 [Jenkins at scale demo](https://github.com/ish-xyz/jenkins-aws-platform) 上使用的工具。

(**小免责声明**:我知道在 Kubernetes 上有很多关于如何自动化 Jenkins 设置的例子。然而，我觉得没有一个真正的“用例/场景”来说明如何在 AWS 实例上部署它)

**阿米族创建**

这里使用的方法是“不可变的基础设施”简而言之，我们将创建预先安装了软件的 ami 并进行部署，而不是提供 EC2 实例并在以后进行配置。
使用不可变的基础设施将有助于我们的一致性和部署时间(除了我今天不打算讨论的许多其他优势之外)。

为了创建 ami，我决定使用 Packer + Ansible。打包机的工作原理如下:

1.  Packer 将从源 AMI 创建一个临时 EC2 实例(在名为 [packer.json](https://github.com/ish-xyz/jenkins-aws-platform/blob/1/images/master/packer.json#L4) 的文件中定义)
2.  然后它将连接到实例(通过 ssh)并运行 Ansible
3.  然后 Packer 会将配置的实例保存为一个新的 AMI，并将一些元数据输出到一个名为 [manifest.json.](https://github.com/ish-xyz/jenkins-aws-platform/blob/1/images/agents/default/manifest.json) 的文件中

**JENKINS 主机和作业供应/配置**

让我们先关注詹金斯大师。

现在我们已经有了 Packer，它创建了一个带有 Jenkins 的 AMI，并且已经安装了它的插件，对于我们来说…我们如何配置 Jenkins 呢？

简单回答:**利用詹金斯 CASC！**

詹金斯 CASC(配置为代码)是一个插件，可以让你从一个 [YAML 文件](https://github.com/ish-xyz/jenkins-aws-platform/blob/1/terraform/templates/jenkins-casc.yaml.tpl)中配置整个詹金斯主程序。

通过这个 YAML 文件，你也可以配置插件设置和**创建任务**。

这最后一部分对我来说至关重要。为了自动创建 Jenkins 作业，我使用了一个**种子作业**。种子作业是在 CASC 配置中定义的特定管道，它为我们创建其他 Jenkins 作业。

它是怎么做到的？**使用 DSL 插件**。(如果你不知道插件是什么，你应该看看这篇文章——>[https://plugins.jenkins.io/job-dsl/](https://plugins.jenkins.io/job-dsl/))

种子作业将下载包含基础设施代码的存储库，并在文件夹`/jenkins-jobs`中提供文件；通过这样做，我们可以将所有的 Jenkins 工作定义为代码。

要了解种子作业的样子，请查看 [CASC 配置](https://github.com/ish-xyz/jenkins-aws-platform/blob/1/terraform/templates/jenkins-casc.yaml.tpl#L84)。

**部署**

我们现在有一个 Jenkins AMI，一个将配置我们的 Jenkins Master 的 YAML 文件，一个将为我们创建管道的种子作业，但是我们如何部署所有这些呢？
在这个[演示](https://github.com/ish-xyz/jenkins-aws-platform/tree/1)中，我使用了 [Terraform](https://terraform.io) ，因为我认为这是目前进行 IAC 的最佳选择。
通过 Terraform，我们将部署整个基础架构+ CASC 文件。

*我们通过 Terraform 而不是 Packer 部署 CASC 文件的原因是，它需要仅在供应时可用的元数据。*

具体来说，Terraform 将执行以下操作:

**代理商拨备**

*   创建 IAM 角色+ IAM 策略(这是 Jenkins 主服务器连接到 AWS 服务所必需的。例如秘密管理器、EC2 等。)
*   创建 PEM 密钥(使用 RSA 算法)
*   Save the early created keys to AWS Secret Manager.
    ( **注意:使用 Jenkins AWS 凭证插件，代理的 SSH 密钥将自动创建为 Jenkins 内的凭证**)
*   渲染并上传 CASC 文件(`/terraform/templates/jenkins.yaml.tpl`)。
    **注意:**使用 CASC 的 jenkins 将创建一个名为“jenkins-agent-key-pair”的凭证，Jenkins 云需要该凭证来自动配置代理。
    实际密钥的内容由 Terraform 本身存储在 AWS Secret Manager 中，Jenkins 可以使用配置的 IAM 角色访问。
    然而，由于 AWS Secret Manager 中的密码名称在每次 Terraform 运行时都会改变，我需要对 CASC 配置进行模板化，使＄{ Jenkins-agent-key-pair }的值是动态的。
*   提供所需的安全组。
*   创建 EC2 实例来托管 Jenkins Master，并在其中部署呈现的 jenkins.yaml (CASC 文件)。

詹金斯特工可以不费吹灰之力搞定。
代理的映像创建使用与 Jenkins 主映像相同的工具和逻辑。
药剂应该是一次性的；它们应该是你在需要的时候创造出来，然后丢弃的东西。Jenkins 的“云”功能恰恰做到了这一点。
云将允许您创建、管理和销毁代理。詹金斯只会在需要的时候产生代理。
云配置只需要很少的参数，可以通过 CASC 进行配置。为了让您了解所需的 CASC 配置，请查看这个[示例](https://github.com/ish-xyz/jenkins-aws-platform/blob/1/terraform/locals.tf#L3)。

# 结论

**教程:**如果您想要部署本文中描述的基础设施，请遵循这里的教程。

优点& CONS :

我不打算列出优点，因为在这一点上，它们应该足够清楚了:)

最后，请务必查看演示部分[“注意事项”](https://github.com/ish-xyz/jenkins-aws-platform/tree/1#considerations)。