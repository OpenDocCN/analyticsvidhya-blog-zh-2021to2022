# 为 python flask 应用程序编写 Kubernetes 清单

> 原文：<https://medium.com/analytics-vidhya/write-kubernetes-manifests-for-python-flask-app-aa9e3bee710b?source=collection_archive---------1----------------------->

这篇文章是系列 [**准备和部署 python 应用到 Kubernetes**](https://augustasberneckas.medium.com/prepare-and-deploy-python-app-to-kubernetes-736b13fe4cef) 的一部分

[👈前一篇文章:容器化 python flask 应用程序](https://augustasberneckas.medium.com/containerizing-python-flask-application-19faa9db031c)

[👈前贴:了解 Minikube](https://augustasberneckas.medium.com/getting-to-know-minikube-e93f9676dea7)

在这个系列的最后，我们将在 Kubernetes 中拥有一个完全可用的 flask 应用程序。我们正在使用的应用程序本身，可以在这里找到:[https://github.com/brnck/k8s-python-demo-app/tree/docker](https://github.com/brnck/k8s-python-demo-app/tree/docker)

# 先决条件

我们将使用`minikube`将我们的应用程序部署到 Kubernetes。如果你对它不熟悉，请到[这篇文章](https://augustasberneckas.medium.com/getting-to-know-minikube-e93f9676dea7)了解更多，因为我们将在这里讨论如何使用 Minikube 等话题。

# 定义用例

在我们开始编写 Kubernetes 清单之前，我们需要为自己清楚地定义我们的应用程序用例。确定我们需要向 Kubernetes 部署什么资源会更容易。

因此，我们有一个应用程序:

*   处理我们的应用程序用户的 HTTP 请求并返回一些内容；
*   有一个 CLI 命令，它也能做一些事情。这可能是一次性的工作或 cronjob。假设我们需要一份一次性工作。

这意味着我们需要:

*   从群集外部访问应用程序；
*   能够独立扩展应用程序；
*   能够运行一次性的工作。

这些用例清楚地表明了我们的应用程序将使用什么样的[工作负载](https://kubernetes.io/docs/concepts/workloads/)。现在，我不打算深入解释工作负载，因为我假设您已经或多或少地了解了它们之间的区别。除了工作负载，我们还必须围绕它们添加额外的资源，以便金融机构客户能够访问我们的应用。

总而言之，这些是我们将要部署到我们的 Kubernetes 集群的 Kubernetes 资源:

*   [名称空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)
*   [部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
*   [工作](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
*   [服务](https://kubernetes.io/docs/concepts/services-networking/service/)
*   [入口](https://kubernetes.io/docs/concepts/services-networking/ingress/)

# 准备目录和文件

为清单创建一个目录，并添加空(目前)文件:

```
mkdir k8s-manifests
touch k8s-manifests/namespace.yaml
touch k8s-manifests/deployment.yaml
touch k8s-manifests/job.yaml
touch k8s-manifests/service.yaml
touch k8s-manifests/ingress.yaml
```

另外，将`k8s-manifests`文件夹添加到`.dockerignore`文件中，因为将清单添加到映像中没有意义。如果您使用 Kubernetes 已经有一段时间了，您可能会注意到一些公共供应商只使用一个文件来保存所有的清单。虽然这没有什么错，但它产生的结果与将所有内容放在单独的文件中并一个接一个地部署是一样的。事实上，将所有清单放在一个文件中有助于资源部署的排序，因为必须首先部署名称空间。然而，为了清晰和可读性，我们将按资源分割文件。

# 正在创建命名空间清单

创建名称空间通常基于上下文。最常见的大概就是团队或者项目。您可以为整个开发团队创建一个名称空间，它将用于部署所有类型的应用程序。他们甚至可能根本没有关系。另一种方法是只为项目创建一个名称空间。这也是我们将对我们的应用程序做的事情。为一个项目而不是一个团队创建一个名称空间可以防止类似“如果这个团队与其他团队合作管理应用程序会怎么样？”甚至不会出现，因为从管理员的角度来看，我们可以更容易地控制访问，并将它们只授予开发人员或团队需要访问的应用程序。

我们的应用叫做 **k8s-python-demo-app** 。我们可以去掉前缀`k8s-`，将我们的名称空间命名为`python-demo-app`。

我们最终的`namespace.yaml`文件应该是这样的:

```
apiVersion: v1
kind: Namespace
metadata:
  name: python-demo-app
```

# 正在创建部署清单

为了更好地理解应用程序的 web 部分需要什么，让我们更深入地研究并定义用例，就像我们对整个应用程序所做的那样。这将有助于更好地理解如何编写部署清单。

从一开始，必须在名称空间`python-demo-app`中创建一个名为`python-demo-app-web`的部署。它应该使用`python-demo-app`作为图像，使用`init`作为图像标签。此外，容器必须作为没有权限提升的`app` (id 1000)用户启动。容器应该请求 128Mi 内存和 100 个 CPU 周期。此外，资源应该限制在 256Mi 和 200 个 CPU 周期。应该有 2 个副本部署到 Kubernetes。最后，必须进行健康检查。[准备就绪探针](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes)应不断检查`8000`端口和`/`端点。[活性探测器](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-command)应寻找 Gunicorn 主进程并确保其正常运行。

用例已定义。现在开始将它转换成部署资源清单。

首先，从添加元数据开始:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
```

继续进行`.spec`部分。将`replicas: 2`添加到`.spec`:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  replicas: 2
```

我们还需要加上`.spec.selector`。根据文档，`.spec.selector`字段定义了部署如何找到要管理的 pod。在这种情况下，您选择在 Pod 模板中定义的标签(`app: python-demo-app`和`role: web`)。然而，更复杂的选择规则是可能的，只要 Pod 模板本身满足该规则。

让我们来定义这两者:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: python-demo-app
      role: web
  template:
    metadata:
      labels:
        app: python-demo-app
        role: web
```

进一步移动到`.spec.template.spec`。在定义容器之前，让我们先处理安全性。我们可以通过添加值为`runAsGroup: 1000`和`runAsUser: 1000`的`securitycontext`键来做到这一点(因为我们的图像用户是 app (id 1000))。为一个 pod 定义`securitycontext`将使 Kubernetes 为该 pod 中的所有容器应用该上下文。继续添加容器名称、图像、标签和容器端口。还记得 docker 的前一课吗？我们已经讨论过默认监听端口`gunicorn`，即`8000`。它应该被定义并命名为`gunicorn`。虽然命名端口是可选的，但它稍后会对我们有所帮助。

经过上述更改后，部署文件现在应该如下所示:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: python-demo-app
      role: web
  template:
    metadata:
      labels:
        app: python-demo-app
        role: web
    spec:
      securityContext:
        runAsGroup: 1000
        runAsUser: 1000 
      containers:
        - name: python-demo-app-web
          image: python-demo-app:init
          ports:
            - name: gunicorn 
              containerPort: 8000
```

需要为容器分配和限制的资源。默认情况下，工作负荷可以没有已定义的资源。这意味着您的应用程序有可能接管整个集群。想象一下，一个应用程序出现了内存泄漏。这将在临时集群中造成问题，并可能在生产集群中造成灾难。这就是为什么为所有容器分配和限制资源通常是一个非常好的做法。然而，这是一个非常深入的话题，需要很好地了解您的应用程序，以及它在启动和空闲时如何在大负载下工作。设置低限制将导致不必要的应用程序重新启动。另一方面，不要设置太高的限制，因为这是一种资源浪费，会降低集群的效率。考虑到这只是一个指南，而且我们正在学习，我们不要在这里集中精力，只定义一些逻辑资源`requests`和`limits`。正如我上面提到的:

> *容器应该请求 128Mi 内存和 100 个 CPU 周期。此外，资源应限制在 256Mi 和 200 个 CPU 周期内*

最后再补充一下`liveness`和`readiness`健康检查。使用`readiness`我们将检查我们的 web 服务器是否响应状态为`200`的 HTTP 请求，使用`liveness`我们将检查`gunicorn`进程的 PID 以确保它正在运行。此外，让我们给它一些初始延迟，以便`gunicorn`工人可以启动。

最终的部署清单应该如下所示:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: python-demo-app
      role: web
  template:
    metadata:
      labels:
        app: python-demo-app
        role: web
    spec:
      securityContext:
        runAsGroup: 1000
        runAsUser: 1000
      containers:
        - name: python-demo-app-web
          image: python-demo-app:init
          ports:
            - name: gunicorn
              containerPort: 8000
          resources:
            requests:
              memory: 128Mi
              cpu: 100m
            limits:
              memory: 256Mi
              cpu: 200m
          readinessProbe:
            initialDelaySeconds: 10
            httpGet:
              port: gunicorn
              path: /
          livenessProbe:
            initialDelaySeconds: 10
            exec:
              command:
                - /bin/sh
                - -c 
                - "pidof -x gunicorn"
```

# 正在创建服务清单

一种将运行在一组 pod 上的应用程序公开为网络服务的抽象方式。Kubernetes 为一组 pod 提供它们自己的 IP 地址和一个 DNS 名称，并可以在它们之间进行负载平衡。

通常，首先创建元数据:

```
apiVersion: v1
kind: Service
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
```

转到`.spec`，需要定义`selector`。它的工作原理和《T2》中的差不多。服务需要知道哪些 pod 有资格将流量路由到。甚至可以添加相同的`labels`:

```
spec:
  selector:
    app: python-demo-app
    role: web
```

最后，服务不仅需要知道流量路由到哪个 pod，还需要知道流量路由到公开端口中的哪个端口。虽然它可以将任何端口映射到目标 pod 端口，但为了方便起见，targetPort 被设置为与端口字段相同的值。让我们将 80 端口用于服务，将其命名为`http`，并将流量路由到 pod 中的`gunicorn`(是的，我们可以使用名称而不是端口号)端口。如果您做的一切都正确，您的最终服务清单应该如下所示:

```
apiVersion: v1
kind: Service
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  selector:
    app: python-demo-app
    role: web
  ports:
    - name: http
      port: 80
      targetPort: gunicorn
```

如果我们现在部署应用程序，那么在集群中运行的任何其他应用程序都可以通过使用`service-name.namespace-name`语法访问这个应用程序。您可以在此阅读更多关于内部集群 DNS 解析[的信息。然而，我们的目标是让我们的客户消费应用程序。这就是`ingress`发挥作用的地方](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#namespaces-of-services)

# 正在创建入口清单

入口基本上是 HTTP 和 HTTPS 路由器从集群外部提供的服务。路由是基于规则的，因此您可以使用一个域并基于规则路由到完全不同的 pod。这只有一个路由`/`，所以我们不打算深入研究通过一个入口路由到多个服务。相反，当对 Kubernetes 的 HTTP 请求带有一个主机`python-app.demo.com`和`/`端点时，我们将配置 ingress 将流量路由到`python-demo-app-web`服务

我想在这里放一个大大的**免责声明**。 **Nginx** 入口控制器中有一个 [bug](https://github.com/kubernetes/ingress-nginx/pull/6187) 。将在 minikube 集群中使用的那个。这就是为什么旧的 **apiVersion** 会被使用(当应用 ingress 时你会得到一个警告)。你可以把它当作一个挑战，用修复了这个错误的镜像版本部署入口控制器。

你已经知道程序了。元数据:)

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
```

在`.spec`部分，我们将定义入口类名，我已经在声明中提到过了。另外，`rules`数组必须被定义。我们可以从添加主持人`python-app.demo.com`的规则开始

```
spec:
  rules:
    - host: python-app.demo.com
```

主持完毕。现在我们需要添加一个端点并将服务附加到该规则。在`host`同胞- `http`下，一切都是可定义的。最终结果应该是这样的:

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: python-demo-app-web
  namespace: python-demo-app
spec:
  rules:
    - host: python-app.demo.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              serviceName: python-demo-app-web
              servicePort: http
```

因为键名很容易解释，所以您可能已经知道这个清单可以翻译为:

> *通过入口控制器、主机 python-app.demo.com 和以/开头的端点发出的所有 http 请求必须转发到名为 python-demo-app 的服务及其端口 http，该端口 http(如果我们再次查看服务清单)应将流量转发到其中一个应用:python-demo-app，role: web 标签的 pods 及其 gunicorn 端口 8000*

网络流量处理的一切都准备好了。让我们为运行一次性作业创建最后一个清单

# 正在创建作业清单

[作业](https://kubernetes.io/docs/concepts/workloads/controllers/job/)基本上是一个或多个 pod，它们被安排运行直到成功完成或被删除。人工智能作业可用于运行数据库模式更新，执行某种维护任务。我们在这个应用程序中的工作非常简单——打印出`Hello, world from CLI!`。

与所有其他清单一样，如上所述，作业需要 apiVersion、kind 和 metadata 字段。

```
apiVersion: batch/v1
kind: Job
metadata:
  name: hello-world-job
  namespace: python-demo-app
```

`.spec.template`是`.spec`的唯一必填字段。更何况`.spec.template`是一个 pod 模板。它与 pod 具有完全相同的模式，除了它是嵌套的并且没有`apiVersion`或`kind`。事实上，当我们完成创建这个文件时，您会注意到`deployment`和`job`有许多相似之处。

如`deployment`所示，我们将`labels`添加到`template`中:

```
spec:
  template:
    metadata:
      labels:
        app: python-demo-app
        role: hello-world-job
```

添加`.spec.template.spec.securityContext`继续。我们希望这个容器也能作为用户**应用**运行。你想过如果工作失败会发生什么吗？Kubernetes 会重启作业吗？或者它只是将其标记为失败而什么也不做？根据文件记载:

> *作业仅适用于重启策略等于 OnFailure 或 Never 的 pod。(注意:如果没有设置 RestartPolicy，默认值总是*

或者换句话说，Kubernetes 默认将`restartPolicy`设为`Always`。如果在没有设置适当的`restartPolicy`的情况下部署作业，部署本身就会失败。为了解决这个问题，我们还可以添加`restartPolicy: OnFailure`

```
spec:
  template:
    metadata:
      labels:
        app: python-demo-app
        role: hello-world-job
    spec:
      securityContext:
        runAsGroup: 1000
        runAsUser: 1000
      restartPolicy: OnFailure
```

对于容器部分，会有一些不同。首先，我们不需要公开任何容器端口，因为这是一个 CLI 命令。接下来，我们不会做任何`readiness`检查，因为该容器不接受任何流量。此外，为了这个指南，我们不要做`liveness`检查，因为这个命令会运行得很快，所以在这种情况下没有用。

应用更改`containers`后，零件应如下所示:

```
 containers:
        - name: python-demo-app-hello-world-job
          image: python-demo-app:init
          command:
            - python3
          args:
            - cli.py
          resources:
            requests:
              memory: 128Mi
              cpu: 100m
            limits:
              memory: 256Mi
              cpu: 200m
```

您可能已经注意到有两个新的键(`command`和`args`，它们在`deployment`中没有出现)。让我解释一下我们在这里做什么。如果您从一开始就遵循本指南，您可能会记得我们使用了相同的映像来运行我们的 web 服务器和 CLI 命令。唯一的不同是我们改变了容器`entrypoint`和`cmd`。这也是我们在这里所做的。关于`command`和`args`的更多解释可以在[这里](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)找到。像这样的方法并不总是好的。大多数情况下，专门为 CLI 创建单独的映像会更好，但我们不要讨论细节，因为这超出了本指南的范围。

我们的最终工作应该是这样的:

```
apiVersion: batch/v1
kind: Job
metadata:
  name: hello-world-job
  namespace: python-demo-app
spec:
  template:
    metadata:
      labels:
        app: python-demo-app
        role: hello-world-job
    spec:
      securityContext:
        runAsGroup: 1000
        runAsUser: 1000
      restartPolicy: OnFailure
      containers:
        - name: python-demo-app-hello-world-job
          image: python-demo-app:init
          command:
            - python3
          args:
            - cli.py
          resources:
            requests:
              memory: 128Mi
              cpu: 100m
            limits:
              memory: 256Mi
              cpu: 200m
```

# 部署到 Kubernetes

是时候将应用程序部署到 Kubernetes 了。

首先，创建一个`minikube`集群。我将为我的 Kubernetes 集群提供`virtualbox`驱动程序和`kubernetes-version`版本 1.20.5 。强烈建议使用与我相同的命令，否则，某些部分可能不会产生我们期望的结果。只有当你知道你在做什么的时候，才进行不同的配置。

```
minikube start --driver=virtualbox --kubernetes-version=1.20.5
```

启用入口插件:

```
minikube addons enable ingress
```

确认群集已启动并正在运行:

```
kubectl get nodes                                        
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   81s   v1.20.5
```

本指南不介绍如何构建一个映像并将其推送到私有或公共注册中心。相反，为了简单起见，我们将直接在 minikube 虚拟机上构建一个映像。确保您位于终端的应用程序目录中:

```
eval $(minikube docker-env)
docker build -t python-demo-app:init .
<...>
Successfully built 22008520508b
Successfully tagged python-demo-app:init
```

确认:

```
minikube ssh docker images | grep python-demo-app
python-demo-app                           init         22008520508b   About a minute ago   125MB
```

一切准备就绪。继续创建一个名称空间并确认它:

```
kubectl apply -f k8s-manifests/namespace.yaml
kubectl get namespaces
NAME              STATUS   AGE
default           Active   11m
ingress-nginx     Active   10m
kube-node-lease   Active   11m
kube-public       Active   11m
kube-system       Active   11m
python-demo-app   Active   12s # <-- Here is our namespace
```

现在部署应用程序的其余部分:

```
kubectl apply -f k8s-manifests/deployment.yaml \
  -f k8s-manifests/service.yaml \
  -f k8s-manifests/ingress.yaml \
  -f k8s-manifests/job.yaml
```

Kubectl 将返回一个输出:

```
deployment.apps/python-demo-app-web created
service/python-demo-app-web created
Warning: networking.k8s.io/v1beta1 Ingress is deprecated in v1.19+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
ingress.networking.k8s.io/python-demo-app-web created
job.batch/hello-world-job created
```

我已经提到了为什么我们使用`networking.k8s.io/v1beta1`而不是`networking.k8s.io/v1`

让我们检查是否所有资源都已部署并正在运行:

```
kubectl get pods,jobs,service,ingress -n python-demo-app
NAME                                      READY   STATUS      RESTARTS   AGE
pod/hello-world-job-9l9w9                 0/1     Completed   0          10s
pod/python-demo-app-web-5f756fbcc-628cd   0/1     Running     0          10s
pod/python-demo-app-web-5f756fbcc-cz5kl   0/1     Running     0          10sNAME                        COMPLETIONS   DURATION   AGE
job.batch/hello-world-job   1/1           3s         10sNAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
service/python-demo-app-web   ClusterIP   10.100.245.172   <none>        80/TCP    10sNAME                                            CLASS   HOSTS                 ADDRESS   PORTS   AGE
ingress.networking.k8s.io/python-demo-app-web   nginx   python-app.demo.com             80      10s
```

网络豆荚还在启动。让我们再等几秒钟。在我们等待的时候，我们可以检查`job`箱`pod/hello-world-job-9l9w9`，因为工作被标记为已完成:

```
kubectl logs hello-world-job-9l9w9 -n python-demo-app                         
Hello, world from CLI!
```

完美！一次性作业成功运行并完成。返回 web 窗格:

```
kubectl get deployment,replicaset,pods -n python-demo-app
NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/python-demo-app-web   0/2     2            0           6m9sNAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/python-demo-app-web-5f756fbcc   2         2         0       6m9sNAME                                      READY   STATUS      RESTARTS   AGE
pod/hello-world-job-9l9w9                 0/1     Completed   0          6m9s
pod/python-demo-app-web-5f756fbcc-628cd   0/1     Running     0          6m9s
pod/python-demo-app-web-5f756fbcc-cz5kl   0/1     Running     0          6m9s
```

pod 正在运行，但未标记为就绪。有点不对劲。我们需要检查一个豆荚的日志:

```
kubectl logs python-demo-app-web-5f756fbcc-628cd -n python-demo-app
[2021-04-22 21:22:32 +0000] [1] [INFO] Starting gunicorn 20.0.4
[2021-04-22 21:22:32 +0000] [1] [INFO] Listening at: http://127.0.0.1:8000 (1)
[2021-04-22 21:22:32 +0000] [1] [INFO] Using worker: sync
[2021-04-22 21:22:32 +0000] [7] [INFO] Booting worker with pid: 7
```

没什么奇怪的。描述一下那个豆荚怎么样:

```
kubectl describe pod python-demo-app-web-5f756fbcc-628cd -n python-demo-app
<...>
Events:
  Type     Reason     Age                     From               Message
  ----     ------     ----                    ----               -------
  Normal   Scheduled  9m5s                    default-scheduler  Successfully assigned python-demo-app/python-demo-app-web-5f756fbcc-628cd to minikube
  Normal   Pulled     9m3s                    kubelet            Container image "python-demo-app:init" already present on machine
  Normal   Created    9m3s                    kubelet            Created container python-demo-app-web
  Normal   Started    9m3s                    kubelet            Started container python-demo-app-web
  Warning  Unhealthy  3m55s (x30 over 8m45s)  kubelet            Readiness probe failed: Get "http://172.17.0.3:8000/": dial tcp 172.17.0.3:8000: connect: connection refused
```

通过检查两个输出，我们可以发现问题。`gunicorn`清楚地显示它在`localhost`监听，而`Readiness`试图向容器 IP 发出 HTTP 请求。解决方案是将`gunicorn`绑定到所有接口。这可以通过向带有`--bind 0.0.0.0`标志的`container`添加`args`部分来解决。当然，有更多的方法来解决这个问题，但是我将把那个留给你，以防这个太容易。

从`Dockerfile`我们知道，提供给`entrypoint`的参数是`app:app`。这意味着它也必须添加到`args`中。

前往`deployment`清单，并在`image`键下添加`args`:

```
<...>
          image: python-demo-app:init
          args:
            - '--bind'
            - '0.0.0.0'
            - 'app:app'
          ports:
<...>
```

重新部署

```
kubectl apply -f k8s-manifests/deployment.yaml
deployment.apps/python-demo-app-web configured
```

30-60 秒后，pod 应该开始运行

```
kubectl get pods -n python-demo-app                      
NAME                                   READY   STATUS      RESTARTS   AGE
hello-world-job-9l9w9                  0/1     Completed   0          22m
python-demo-app-web-6c4cc75ddc-tmxjl   1/1     Running     0          67s
python-demo-app-web-6c4cc75ddc-wb79r   1/1     Running     0          87s
```

的确如此。我们走吧。我们发现并修复了一个错误！

我们可以从外部访问我们的应用程序吗？让我们检查一下:

```
curl -H "Host: python-app.demo.com" $(minikube ip)/
Hello, World!
```

HTTP 请求成功，返回答案！没有别的事可做了。我们现在可以清除集群中的所有内容:

```
kubectl delete -f k8s-manifests/deployment.yaml \
  -f k8s-manifests/service.yaml \
  -f k8s-manifests/ingress.yaml \
  -f k8s-manifests/job.yaml
```

此外，删除名称空间:

```
kubectl delete -f k8s-manifests/namespace.yaml
```

# 结论

恭喜你写了你自己的 Kubernetes 清单。如果这太简单了，而你想要更多，这里有几个可以额外完成的任务:

*   修改 CLI 命令以读取名为`MESSAGE`的`environment variable`，并使用[配置图](https://kubernetes.io/docs/concepts/configuration/configmap/)传递值
*   使用`configmap`创建`gunicorn`配置文件，并将其安装到`web`吊舱。确保`bind`是通过配置文件传递的，而不是通过`args`

应用程序的清单可以在[这里](https://github.com/brnck/k8s-python-demo-app/tree/manifests)找到。继续下一个指南，我们将[将清单包装到舵图](https://augustasberneckas.medium.com/write-helm-charts-for-python-flask-app-ee2777fb458c)