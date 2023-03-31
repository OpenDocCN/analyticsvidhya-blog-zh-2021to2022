# 支持 Spark 和 AWS S3 的测试和本地开发

> 原文：<https://medium.com/analytics-vidhya/enabling-testing-and-local-development-with-spark-and-aws-cacbe4ed391f?source=collection_archive---------0----------------------->

## 开发工作流程

## 将 Spark 与 LocalStack 集成

![](img/d260dce154554a81fd14987fe5167a15.png)

[伊瓦·拉乔维奇](https://unsplash.com/@eklektikum?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

测试 Spark 是一项具有挑战性的任务。隔离测试环境是工程师很少或没有访问 AWS 服务(如 S3)的场景的需求。LocalStack 提供带有 AWS 兼容 API 的模拟服务。

这篇文章着眼于集成 Spark 和 Localstack 所需的最少工作。假设你了解 Docker、Docker Compose、Spark (pyspark)、LocalStack、AWS S3 和 AWS CLI 的基础知识。

# 问题陈述

作为一名工程师，我想在不连接 AWS 的情况下开发和测试我的 Spark 应用程序。

# 解决办法

起点是使用 Localstack 模拟 AWS S3。使用下面的 Docker 组合定义，我们可以很容易地让 S3 兼容的服务在本地运行。

```
version: '3.7'
services:
  localstack:
    image: localstack/localstack
    environment:
      - SERVICES=s3
      - DEFAULT_REGION=eu-west-1
      - AWS_ACCESS_KEY_ID=foo
      - AWS_SECRET_ACCESS_KEY=foo
    ports:
      - 4566:4566
```

# 火花和 S3 配置

下一步是查看 Spark 以及如何配置到 Localstack 的连接。很快，我们知道我们需要在 LocalStack 中更改模拟 S3 的 URL。如果您正在使用 AWS CLI，您知道`--endpoint-url`选项可用于此。对于 spark，这是类似的，但是需要更多的配置更新。

默认情况下，Spark 不提供 S3 数据源，所以我们需要添加 AWS SDK 和 Hadoop SDK。这就像添加以下包一样简单

```
--packages software.amazon.awssdk:s3:2.17.52,org.apache.hadoop:hadoop-aws:3.1.2
```

接下来，我们需要设置文件系统配置。

```
--conf spark.hadoop.fs.s3a.endpoint=http://localhost:4566 \
--conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
--conf spark.hadoop.fs.s3a.access.key=foo \
--conf spark.hadoop.fs.s3a.secret.key=foo \
--conf spark.hadoop.fs.s3a.path.style.access=true
```

*   端点 URL:这是 S3 文件系统的 URL
*   访问密钥和秘密密钥:AWS 身份验证详细信息。
*   实现:用`s3a`协议指定了 S3 文件系统的类
*   路径样式:这将强制使用 Localstack 支持的 HTTP URL 路径样式的访问。

既然我们已经知道了如何配置 Spark 来为任何 S3 URL 连接到 Localstack，那么让我们看看如何访问数据。我将使用 Python 来演示，但语言并不重要。

```
spark.read.csv('s3a://my-bucket/requests.csv').show()
```

## 引导本地堆栈 S3

在运行 Spark 应用程序之前，我们需要向 Localstack 的 S3 实例添加一些测试数据。为此，有两种方法，使用 AWS CLI 或使用带有 Localstack 的引导脚本。

首先使用 AWS CLI，我们需要像以前一样覆盖端点 URL。

```
export AWS_ACCESS_KEY_ID=foo 
export AWS_SECRET_ACCESS_KEY=foo # add data to Localstack S3
cat > requests.csv <<EOF
"email1"
"email2"
"email3"
EOFaws s3 --endpoint-url http://localhost:4566 --region eu-west-1 mb s3://my-bucket 
aws s3 --endpoint-url http://localhost:4566 --region eu-west-1 cp requests.csv s3://my-bucket/
```

这个代码片段创建了一个包含 3 行数据的`requests.csv`文件，创建了一个新的 bucket 并将 CSV 复制到 bucket 中。

第二种方法是编写启动 Localstack 容器时执行的引导脚本。为此，您需要将脚本目录挂载到`/docker-entrypoint-initaws.d/`。然后，Localstack 将执行该 repo 中的任何脚本。例如，您可以使用`awslocal` CLI 将上述内容作为脚本。

```
#!/usr/bin/env bashawslocal s3 mb s3://my-bucket cat > requests.csv <<EOF
"email1"
"email2"
"email3"
EOFawslocal s3 cp requests.csv s3://my-bucket/
```

现在这可以作为样本数据在 localstack 中获得，您可以运行您的 Spark 应用程序。

记住，如果你也在 Docker 中运行 Spark，使用相同的网络和 Localstack contain 的主机名，而不是`localhost`。可以在这里找到完整代码:[https://gist . github . com/athar vai/a0bed 7d 989 B3 b 316 Fe 6 f 056d 674 b 99 b 0](https://gist.github.com/atharvai/a0bed7d989b3b316fe6f056d674b99b0)