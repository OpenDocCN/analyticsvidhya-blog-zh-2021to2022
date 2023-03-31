# 如何在 Apache Kafka 中使用 PEM 证书

> 原文：<https://medium.com/analytics-vidhya/how-to-use-pem-certificates-with-apache-kafka-f3b444a00816?source=collection_archive---------6----------------------->

![](img/1b180b49ad714e5a0326d2c7b56d6343.png)

照片由[飞:D](https://unsplash.com/@flyd2069?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/@flyd2069?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这是一个漫长的等待，但它终于来了:从 Apache Kafka 2.7 开始，现在可以在代理和 java 客户端使用 PEM 格式的 TLS 证书。

你可能想知道——这有什么关系？

PEM 是一种将 x509 证书和私钥编码为 Base64 ASCII 字符串的方案。这使得处理您的证书更加容易。您可以简单地将密钥和证书作为字符串参数提供给应用程序(例如，通过环境变量)。如果您的应用程序在容器中运行，这一点尤其有用，因为将文件装载到容器会使部署管道变得更加复杂。在这篇文章中，我想向你展示在 Kafka 中使用 PEM 证书的两种方法。

# 以字符串形式提供证书

## 代理和 CLI 工具

您可以将证书直接添加到客户端或代理的配置文件中。如果您以单行字符串的形式提供它们，则必须通过在每行的末尾添加换行符(\n)将原始的多行格式转换为单行。属性文件的 SSL 部分应该是这样的:

```
security.protocol=SSL
ssl.keystore.type=PEM
ssl.keystore.certificate.chain=-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----
ssl.keystore.key=-----BEGIN ENCRYPTED PRIVATE KEY-----\n...\n-----END ENCRYPTED PRIVATE KEY-----
ssl.key.password=<private_key_password>
ssl.truststore.type=PEM
ssl.truststore.certificates=-----BEGIN CERTIFICATE-----\n...\n-----END CERTIFICATE-----
```

注意**SSL . keystore . certificate . chain**需要包含您的签名证书以及所有的中间 CA 证书。有关这方面的更多详细信息，请参见下面的 ***常见问题*** 部分。

您的私钥进入 **ssl.keystore.key** 字段，而私钥的密码(如果您使用的话)进入 **ssl.key.password** 字段。

## Java 客户端

Java 客户端使用完全相同的属性，但是常量有助于提高可读性:

```
Properties properties = new Properties();
properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");...omitted other producer configs...//SSL configs
properties.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SSL");
properties.put(SslConfigs.SSL_KEYSTORE_TYPE_CONFIG, "PEM");
properties.put(SslConfigs.SSL_KEYSTORE_CERTIFICATE_CHAIN_CONFIG, "<certificate_chain_string>");
properties.put(SslConfigs.SSL_KEYSTORE_KEY_CONFIG, "<private_key_string>");
// key password is needed if the private key is encrypted
properties.put(SslConfigs.SSL_KEY_PASSWORD_CONFIG, "<private_key_password>");
properties.put(SslConfigs.SSL_TRUSTSTORE_TYPE_CONFIG, "PEM");
properties.put(SslConfigs.SSL_TRUSTSTORE_CERTIFICATES_CONFIG, "<trusted_certificate>");

producer = new KafkaProducer<>(properties);
```

# 以文件形式提供证书

如果您已经对 Kafka 使用了 mTLS 身份验证，那么过渡到 PEM 证书的最简单的方法就是将它们作为文件使用，取代您现在使用的 java 密钥库和信任库。这种方法使得从 PKCS12 文件转换到 PEM 文件变得很容易。

## 代理和 CLI 工具

属性文件的 SSL 部分应该是这样的:

```
ssl.keystore.type=PEM
ssl.keystore.location=/path/to/file/containing/certificate/chain
ssl.key.password=<private_key_password>
ssl.truststore.type=PEM
ssl.truststore.location=/path/to/truststore/certificate
```

**ssl.keystore.type** 和 **ssl.truststore.type** 属性告诉 Kafka 我们以何种格式提供证书和信任库。

接下来， **ssl.keystore.location** 指向一个应该包含以下内容的文件:

*   你的私人钥匙
*   您的签名证书
*   以及任何中间 CA 证书

有关证书链的更多详细信息，请参见下面的 ***常见问题*** 部分。

如果您的私钥被加密(我希望是这样)，您将需要设置 ssl.key.password 。确保不要提供 **ssl.keystore.password** ，否则会出现错误。

## Java 客户端

Java 客户端也使用相同的属性，但是这里我们使用 Kafka 客户端库提供的常量:

```
Properties properties = new Properties();
properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");...omitted other producer configs...//SSL configs
properties.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SSL");
properties.put(SslConfigs.SSL_KEYSTORE_TYPE_CONFIG, "PEM");
properties.put(SslConfigs.SSL_KEYSTORE_LOCATION_CONFIG, "/path/to/file/containing/certificate/chain");
// key password is needed if the private key is encrypted
properties.put(SslConfigs.SSL_KEY_PASSWORD_CONFIG, "<private_key_password>");
properties.put(SslConfigs.SSL_TRUSTSTORE_TYPE_CONFIG, "PEM");
properties.put(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, "/path/to/truststore/certificate");

producer = new KafkaProducer<>(properties);
```

# 设置证书链时的常见问题

1.  如果您的私钥是加密的(它应该总是加密的)，您需要将它从 PKCS#1 格式转换为 PKCS#8 格式，以便 java/kafka 能够正确地读取它
2.  如果您想以单行字符串的形式提供 PEM 证书，请确保在每行的末尾添加换行符(\n)。否则，证书将被视为无效。
3.  证书链必须包括您的证书以及签署它的所有中间 CA 证书，按此顺序。例如，如果您的证书由证书 A 签名，而证书 A 由证书 B 签名，证书 B 由根证书签名，那么您的证书链必须依次包括:您的证书、证书 A 和证书 B。请注意，根证书不应该在链中。
4.  证书链中的证书顺序很重要(参见第 3 点)

# 带有 PEM 证书的 Kafka SSL 设置示例

测试客户机的 SSL 设置并不简单，因为设置带有 SSL 身份验证的 Kafka 集群并不是一个简单的过程。这就是为什么我用一个 zookeeper 和 broker 创建了一个 [docker-compose 项目](https://github.com/codingharbour/kafka-docker-compose/tree/master/kafka-ssl)，并启用了 SSL 认证。这个项目使用了许多来自 Confluent 优秀的 [cp-demo 项目](https://github.com/confluentinc/cp-demo)的想法。

要使用该项目，克隆 [docker-compose 存储库](https://github.com/codingharbour/kafka-docker-compose)，并导航到 **kafka-ssl** 文件夹。

```
git clone https://github.com/codingharbour/kafka-docker-compose.git cd kafka-ssl
```

运行 **start-cluster.sh** 脚本将生成自签名根证书。该脚本将使用它来签署所有其他证书。它还将为经纪人和动物园管理员生成和签署证书，并为一个生产者和消费者生成和签署证书。此后，脚本将使用 docker-compose 启动集群。

> 没有 docker-compose？查看:[如何安装 docker-compose](https://docs.docker.com/compose/install/)

此外，启动脚本将生成 **producer.properties** 和 **consumer.properties** 文件，您可以使用 **kafka-console-*** 工具。

consumer.properties 文件是如何将 PEM 证书用作字符串的示例。另一方面，producer.properties 使用存储在 PEM 文件中的证书。通过这种方式，您可以看到并测试这篇博文中描述的两种方法。

# 你想更多地了解卡夫卡吗？

我创建了一个卡夫卡迷你课程，你可以完全免费获得。[在编码港](https://codingharbour.com/kafka-mini-course/)报名。

【https://codingharbour.com】最初发表于[](https://codingharbour.com/apache-kafka/using-pem-certificates-with-apache-kafka/)**。**