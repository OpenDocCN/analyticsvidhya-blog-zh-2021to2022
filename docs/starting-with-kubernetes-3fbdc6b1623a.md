# 从 Kubernetes 开始

> 原文：<https://medium.com/analytics-vidhya/starting-with-kubernetes-3fbdc6b1623a?source=collection_archive---------4----------------------->

![](img/3c951042de4a6306c17feb98899d4fd9.png)

云

在十年的云计算发展历程中，人们创造了多种工具和技术来处理云计算。K8s (Kubernetes)是作为云开发的一部分被广泛使用的工具之一。无论是生产环境的服务器管理或资源管理，还是开发应用程序并将其部署在云上，kubernetes 都扮演着至关重要的角色。

从运行包含代码二进制文件的容器(docker、rkt 等)开始，到使其高度可用于运行服务器，kubernetes 被用作编排工具。尽管每个组织中都有一个团队(运营团队)来处理所有的部署过程，但是理解 kubernetes 对于开发人员的基本姿态是很重要的。(devo PS——开发人员和运营团队合作向客户提供应用程序时使用的术语)。

Kubernetes 提供资源/对象来处理集群。pod、服务、部署、副本集等。是 kube 对象的一些例子。我将首先创建一个部署，并公开它以供在集群外部访问，从而给出一个关于如何使用 kubernetes 在云上部署您的应用程序的想法。

您可以从免费在 IBM cloud 上创建一个集群开始。

*   在 IBM cloud 上创建一个免费的 [lite 账户](https://cloud.ibm.com/registration)。
*   在 IBM [目录](https://cloud.ibm.com/catalog)中搜索`kubernetes service`。
*   创建一个[集群](https://cloud.ibm.com/kubernetes/catalog/create)并开始玩它。

IBM cloud 提供服务来创建 IBM kubernetes 集群，默认情况下，该集群带有一个主节点和一个工作节点。了解云及其组件很容易，或者您可以先在本地环境中安装 minikube。开始使用 IBM kubernetes 服务的好处是，它为您提供了 minikube 中没有的多区域负载平衡。

让我们从用一个基本的演示应用程序创建一个部署开始，然后使用一个服务向外部世界公开它。

D

演示应用程序的部署清单

在命令行中使用 kubectl 客户端:-

```
kubectl apply -f hello-world.yaml
```

将使用上述清单文件创建部署。我们可以使用`kubectl get pods`来验证部署中正在运行的 pod 的状态。

创建的 pod 将有一个从[gcr.io/kuar-demo/kuard-amd64:blue](http://gcr.io/kuar-demo/kuard-amd64:blue)拉的演示应用程序图像。部署的名称将在第 4 行声明为 hello-world-app。

部署清单文件中有三个主要部分:

*   元数据:包含每个 kubernetes 对象中的名称、标签和注释，用于链接资源，比如为 pod 创建服务或向部署添加副本。
*   规格选择器:指定选择器以匹配为部署使用而创建的 pod 的标签。
*   规格模板:该部分包含标签为`app=hello-world`的 pod 模板的详细信息。这一部分还声明了容器映像及其将运行的端口。

> kubernetes[pod](https://kubernetes.io/docs/concepts/workloads/pods/)被创建和销毁以匹配你的集群的状态。豆荚是非永久资源。如果您使用一个[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)来运行您的应用程序，它可以动态地创建和销毁 pod。

S*service*是一个 kubernetes 资源，用于集群内部的服务发现。用来发现跑荚的 ip。由于 pod 是临时的，每次在部署中发生操作或 pod 因错误而失败时，都会启动一个新的 pod，并为其分配一个新的 IP 地址。为了跟踪集群中正在运行的 pods IP，需要使用服务。

*   一种方法是创建一个 service.yaml 文件，其类型设置为 NodePort，具有 nodePort 地址和 targetPort，该文件将指向部署中运行演示应用程序的容器端口。

服务清单

```
kubectl apply -f hello-world-svc.yaml
```

将创建一个服务，在节点端口为`31345`的集群外部公开标签为`hello-world`的正在运行的 pod，您可以使用公共端点(`minikube ip`)来访问它。

*   另一种方法是使用 kubectl client 公开部署。应该传递的两个参数是名称和类型(将服务类型定义为负载平衡器或节点端口)。

```
kubectl expose deployment hello-world-app --type=NodePort --name=hello-world-svc
```

使用 kube 客户端公开部署的问题是，它会从一个范围内随机选择公开的端口(默认值:30000–32767)。虽然您也可以使用`--service-node-port-range`通过服务节点端口。

下一步将是查看您的应用从集群外部公开的集群 ip。我们可以使用`kubectl cluster-info`查看集群的外部 ip，并通过节点端口访问应用程序，如果是 minikube，我们可以使用命令`minikube ip`。然后我们可以使用 curl 或 rest 客户端在`[http://(minikube](http://(minikube) ip):<NodePort>`上查看一个正在运行的演示应用程序，例如上面的例子:`[http://(minikube](http://(minikube) ip):31345`。

现在，您已经准备好运行一个演示应用程序，并了解如何使用 kubernetes 部署一个应用程序，并通过使用上面的 kubernetes 上的一个简单应用程序部署示例，将它公开给外部用户使用 rest 客户端来访问它。关注 https://golang-geek.medium.com/[的](https://golang-geek.medium.com/)更新…