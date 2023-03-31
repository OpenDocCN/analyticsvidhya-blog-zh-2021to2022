# GraphQL 阿波罗 Java 客户端与 Spring Boot 微服务集成

> 原文：<https://medium.com/analytics-vidhya/graphql-apollo-java-client-integration-with-spring-boot-micro-services-dbcfd2a1b8b9?source=collection_archive---------0----------------------->

![](img/c0411de9eb6b8151cc948620f395a53e.png)

GraphQL 现在是一个热门话题，它打破了前端-后端 API 通信的传统。在 tech spaces 上有众所周知的服务器/客户端包，在 NodeJs、Python、Java(Android)、Go 等语言中创建 graphqL 服务器也有大量的社区支持。

几乎没有为 java 客户端找到很好解释的工作流。由于 Apollo 客户端是为 Android 设计的，大部分代码片段最好用 Kotlin。这篇文章的目的绝对是为了帮助那些希望创建/集成带有子图的 graphql 客户端的用户。

# 1.添加依赖关系

*   对于*格拉德*

*   给 Maven 的

# 2.生成模式和查询

对于 gradle，使用以下步骤生成模式

```
# Create a directory for your GraphQL files:**mkdir -p src/main/graphql/com/schema/**

# --schema is interpreted relative to the current working directory. **./gradlew downloadApolloSchema \
  --endpoint="https://your.domain/graphql/endpoint" \
  --schema="src/main/graphql/com/schema/schema.graphqls"**
```

对于 Maven，请按照[这里的](https://github.com/aoudiamoncef/apollo-client-maven-plugin)解释配置。

接下来，将 graphql 查询添加到 graphql 文件夹中。

```
**src/main/graphql/com/queries/getInfo.gql**query GetProductInfo($products: ProductQuery!) {
    getProducts(products: $products) {
        opcoId
        productId }
}
```

# 3.构建查询类

```
./gradlew build ORmvn generate-sources
```

在生成的源文件夹中会为您的查询生成类。

# 4.客户端类和查询方法

*   创建一个 OkHttp 客户端并添加你的头(包括认证头)
*   使用 graphql 端点 URL 构建您的客户端

使用生成的查询类，您可以查询数据。

如果调用应该是同步调用，那么应该应用下面的包装器。

例如:

```
toComplelatableFuture(client.query(new GetProductInfoQuery()));
```

感谢花在阅读上的时间。如果你需要更多信息，滑入评论。

和平🙌