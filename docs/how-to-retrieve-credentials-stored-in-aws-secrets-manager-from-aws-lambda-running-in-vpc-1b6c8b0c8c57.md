# 如何从在 VPC 运行的 AWS Lambda 检索存储在 AWS Secrets Manager 中的凭据

> 原文：<https://medium.com/analytics-vidhya/how-to-retrieve-credentials-stored-in-aws-secrets-manager-from-aws-lambda-running-in-vpc-1b6c8b0c8c57?source=collection_archive---------0----------------------->

![](img/f76a657f002f6decfd0fea6668dab958.png)

斯蒂芬·斯坦鲍尔在 [Unsplash](https://unsplash.com/s/photos/secrets?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

AWS Secrets Manager 是一个机密管理服务，它使您能够存储凭据并在需要时动态检索凭据。它有助于保护对您的应用程序和服务的访问。使用 AWS Secret Manager，您可以-

*   在运行时以编程方式检索加密的秘密值