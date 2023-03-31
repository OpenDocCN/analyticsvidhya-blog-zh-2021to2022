# 容器化 python 烧瓶应用程序

> 原文：<https://medium.com/analytics-vidhya/containerizing-python-flask-application-19faa9db031c?source=collection_archive---------12----------------------->

这篇文章是系列 [**准备和部署 python 应用到 Kubernetes**](/@augustasberneckas/prepare-and-deploy-python-app-to-kubernetes-736b13fe4cef) 的一部分

在这个系列的最后，我们将在 Kubernetes 中拥有一个完全可用的 flask 应用程序。我们正在使用的应用程序本身，可以在这里找到:[https://github.com/brnck/k8s-python-demo-app](https://github.com/brnck/k8s-python-demo-app)

# 创建 dockerfile 文件

在我们开始编写 Kubernetes 清单之前，我们需要有容器化的应用程序。本指南假设您对 [docker](https://docs.docker.com/) 以及如何[构建图像](https://docs.docker.com/engine/reference/builder)有一定的了解

让我们从创建 dockerfile 开始:

```
touch Dockerfile
```

我们的应用程序支持 python 3.x，所以让我们从最新的稳定 python(撰写本指南时为 3.9.4)开始构建:

```
FROM python:3.9.4-slim
```

现在你可能想知道标签末尾的单词 **slim** 是干什么的。你可能还见过其他关键词，比如**阿尔卑斯山**

有一个很好的解释贴[朱莉紫苏加西亚](/@geekgirl907)。

可以在这里查看:[阿尔卑斯、苗条、拉伸、巴斯特、杰西、靶心 Docker 形象有哪些不同？](/swlh/alpine-slim-stretch-buster-jessie-bullseye-bookworm-what-are-the-differences-in-docker-62171ed4531d)

出于安全目的，我们不想以 root 用户身份运行我们的应用程序，因此让我们创建应用程序用户:

```
RUN groupadd --gid 1000 app && \
    useradd --create-home --gid 1000 --uid 1000 app
```

现在让我们创建一个目录来存放我们的应用程序。有些人喜欢把一个应用放在`/app`或者`/appname`里。然而，我喜欢把应用程序放到`/home/app/src`中，甚至不放在容器中，也放在硬件或虚拟机中。这提供了跨环境的某种标准化。因此，让我们创建一个目录，并将其作为工作目录:

```
RUN mkdir -p /home/app/srcWORKDIR /home/app/src
```

差不多结束了，环境准备好了。让我们继续复制源代码并安装依赖项:

```
COPY ./ /home/app/src
RUN pip3 install -r requirements.txt
```

最后，我们希望我们的应用程序作为用户**应用程序**运行，使用 **flask** [入口点](https://docs.docker.com/engine/reference/builder/#entrypoint)，而 **run** 作为[命令](https://docs.docker.com/engine/reference/builder/#cmd)

```
USER appENTRYPOINT ["flask"]CMD ["run"]
```

我们完整的 docker 文件应该是这样的:

```
FROM python:3.9.4-slimRUN groupadd --gid 1000 app && \
    useradd --create-home --gid 1000 --uid 1000 appRUN mkdir -p /home/app/srcWORKDIR /home/app/srcCOPY ./ /home/app/src
RUN pip3 install -r requirements.txtUSER appENTRYPOINT ["flask"]CMD ["run"]
```

# 添加中。dockerignore

我们还希望防止将不必要的文件添加到 docker 映像中。那就是[的地方。dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file) 进来了。它的工作原理类似于。gitignore 通过忽略将构建到图像中的文件或文件夹。这有助于通过避免不必要地将大的或敏感的文件和目录发送到守护程序，以及潜在地使用`ADD`或`COPY`将它们添加到映像来减小映像大小

让我们创建`.dockerignore`文件并忽略不必要的文件:

```
.idea/
.vscode/
.git/
.venv/
__pycache__/
.gitignore
Dockerfile
```

# 构建和运行 docker 文件

```
docker build -t python-demo-app:init .
<...>
 => => naming to docker.io/library/python-demo-app:init
```

让我们运行我们的容器

```
docker run --rm python-demo-app:init run --host 0.0.0.0 --port 5000
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

打开另一个终端会话并 curl localhost

```
curl localhost:5000
curl: (7) Failed to connect to localhost port 5000: Connection refused
```

哎呀...端口不对主机公开。让我们用暴露的端口重新运行容器:

```
docker run --rm --publish 5000:5000 python-demo-app:init run --host 0.0.0.0 --port 5000
curl 127.0.0.1:5000
Hello, World!
```

# 切换到生产服务器

您可能已经注意到，甚至 flask 本身也会打印一条警告:

```
* Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
```

因为我们正在为生产准备应用程序，所以让我们切换到 WSGI 服务器。在这种情况下，我们将使用 [gunicorn](https://gunicorn.org/)

让我们通过添加 **gunicorn** 来编辑我们的 **requirements.txt** 文件:

```
gunicorn==20.0.4
```

继续编辑 Dockerfile 并从**烧瓶**切换到 **gunicorn** :

转换

```
ENTRYPOINT ["flask"]
```

到

```
ENTRYPOINT ["gunicorn"]
```

还有，开关

```
CMD ["run"]
```

到

```
CMD ["app:app"]
```

我们的 docker 文件现在应该是这样的:

```
FROM python:3.9.4-slimRUN groupadd --gid 1000 app && \
    useradd --create-home --gid 1000 --uid 1000 appRUN mkdir -p /home/app/srcWORKDIR /home/app/srcCOPY ./ /home/app/src
RUN pip3 install -r requirements.txtUSER appENTRYPOINT ["gunicorn"]CMD ["app:app"]
```

让我们重新构建我们的容器并再次运行它:

```
docker build -t python-demo-app:init .
```

因为 gunicorn 默认运行在端口`8000`上，所以让我们切换到它而不是`5000`端口

```
docker run --rm --publish 8000:8000 python-demo-app:init --bind 0.0.0.0 app:app
[2021-04-21 13:11:24 +0000] [1] [INFO] Starting gunicorn 20.0.4
[2021-04-21 13:11:24 +0000] [1] [INFO] Listening at: http://0.0.0.0:8000 (1)
[2021-04-21 13:11:24 +0000] [1] [INFO] Using worker: sync
[2021-04-21 13:11:24 +0000] [7] [INFO] Booting worker with pid: 7curl 127.0.0.1:8000
Hello, World!
```

# 运行 CLI 脚本而不是 web 服务器

我们可以选择只为 python CLI 脚本构建一个单独的映像，或者只通过更改**入口点**和 **cmd** 来重用我们当前的映像。让我们将**入口点**切换到 **python3** 而不是 **gunicorn** 并执行 **cli.py** 脚本

```
docker run --rm --entrypoint python3 python-demo-app:init cli.py
Hello, world from CLI!
```

# 结论

在本指南中，我们已经创建了 dockerfile，我们将使用它来部署和服务来自 Kubernetes 的应用程序。

最终的解决方案可以在这里找到:[https://github.com/brnck/k8s-python-demo-app/tree/docker](https://github.com/brnck/k8s-python-demo-app/tree/docker)

让我们转到下一个指南，为我们的应用程序创建 Kubernetes 清单。或者如果你不熟悉`minikube`，在进入清单部分之前，查看[这个](https://augustasberneckas.medium.com/getting-to-know-minikube-e93f9676dea7)帖子