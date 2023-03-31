# 用 Numpy 和 Gunicorn 将 Django 归档

> 原文：<https://medium.com/analytics-vidhya/dockerizing-django-with-numpy-and-gunicorn-1b969cdb5f78?source=collection_archive---------11----------------------->

![](img/8ae68bf08b286540c87729636fd75bcb.png)

大家好。

事实上，网上有很多关于 django 的教程。但是在我工作的项目中，我们使用像 Numpy 和 Pillow 这样的库，alpine linux 不能编译这个，因为缺少依赖关系。

我们的堆栈:

*   姜戈
*   Numpy
*   枕头
*   Postgres

首先让我们创建基本的`Dockerfile`

```
FROM python:3.6.4-alpineADD . /appWORKDIR /appRUN pip install -r requirements.txtEXPOSE 8000CMD ["manage.py", "runserver"]
```

如果你不使用像`Numpy`这样的 c 编译库，它基本上可以工作。

问题是，我们需要在 alpine 中添加编译器，我有一个很大的层，因为我认为在一个层上运行这个进程更好。

完全依赖安装层:

```
RUN apk add --no-cache jpeg tiff-dev openjpeg-dev postgresql-dev && \
    apk add --no-cache \
    --virtual=.build-deps \
    g++ zlib-dev freetype-dev lcms2-dev \
    gcc libc-dev linux-headers tk-dev tcl-dev harfbuzz-dev \
    fribidi-dev build-base py-pip rsyslog cython file binutils \
    musl-dev python3-dev cython && \
    ln -s locale.h /usr/include/xlocale.h && \
    pip install --upgrade pip && \
    pip install -r requirements.txt --no-cache-dir && \
    rm -r /root/.cache && \
    find /usr/lib/python3.*/ -name 'tests' -exec rm -r '{}' + && \
    find /usr/lib/python3.*/site-packages/ -name '*.so' -print -exec sh -c 'file "{}" | grep -q "not stripped" && strip -s "{}"' \; && \
    rm /usr/include/xlocale.h && \
    apk --purge del .build-deps
```

# Alpine 编译器

我安装了一些像`g++`和`linux-headers`虚拟的包，并在 pip 安装后清理了它们，因为我只需要这些包来发布一些依赖关系的脚本，而大小是容器的问题。

```
-t, --virtual NAME    Instead of adding all the packages to 'world', create a new 
                        virtual package with the listed dependencies and add that 
                        to 'world'; the actions of the command are easily reverted 
                        by deleting the virtual package
```

这意味着当您安装软件包时，这些软件包不会添加到全局软件包中。并且这种改变可以容易地恢复。所以如果我需要 gcc 来编译一个程序，但是一旦程序被编译了，我就不再需要 gcc 了。

我可以在一个虚拟包中安装 gcc 和其他所需的包，并且所有的依赖项和所有东西都可以从这个虚拟包名称中删除。下面是一个用法示例

```
apk add --virtual mypacks gcc vim
apk del mypacks
```

下一个命令将删除用第一个命令安装的所有 18 个软件包。

# Postgres

我在我的本地机器上运行 Postgresql，而不是 docker。我需要把它连接起来。你可以使用`host.docker.internal`这个映射直接路由你的机器`localhost`

所以我添加了这个 env 变量

```
ENV DB_HOST=host.docker.internal
```

并在“settings.py”上使用了它

— -

# 格尼科恩

我决定用 Gunicorn 来运行用`n`工人处理的程序。因此，我将`gunicorn`添加到 requirements.txt 中，并添加了两行用于运行流程

```
ENV WORKER_COUNT=2CMD ["sh", "-c", "gunicorn --bind :8000 --workers ${WORKER_COUNT} project.wsgi:application"]
```

使用`WORKER_COUNT`环境变量作为默认值。每当我需要的时候，我都会覆盖这个值。

# 运转

我决定用`n` workers 创建 makefile 来运行这个容器。所以我用这个命令创建了 Makefile。

```
docker-run:
 docker run -e worker_count=${worker} -p 127.0.0.1:8000:8000 project${version}
```

# 完整的文档

```
FROM python:3.6.4-alpineADD . /app
WORKDIR /app# Need to Update at Settings.py
ENV DB_HOST=host.docker.internal# Default Worker Count is 2
ENV WORKER_COUNT=2# Install dependencies layer.
RUN apk add --no-cache postgresql \
                        jpeg tiff-dev openjpeg-dev \
                        libpq postgresql-libs postgresql-dev && \
    apk add --no-cache \
    --virtual=.build-deps \
    g++ jpeg-dev zlib-dev freetype-dev lcms2-dev \
    gcc libc-dev linux-headers tk-dev tcl-dev harfbuzz-dev \
    fribidi-dev zlib-dev build-base py-pip rsyslog cython file binutils \
    musl-dev python3-dev cython && \
    ln -s locale.h /usr/include/xlocale.h && \
    pip install --upgrade pip && \
    pip install -r requirements.txt --no-cache-dir && \ 
    rm -r /root/.cache && \
    find /usr/lib/python3.*/ -name 'tests' -exec rm -r '{}' + && \
    find /usr/lib/python3.*/site-packages/ -name '*.so' -print -exec sh -c 'file "{}" | grep -q "not stripped" && strip -s "{}"' \; && \
    rm /usr/include/xlocale.h && \
    apk --purge del .build-depsEXPOSE 8000CMD ["sh", "-c", "gunicorn --bind :8000 --workers ${WORKER_COUNT} project.wsgi:application"]
```

我希望这篇文章对你有用。

谢了。