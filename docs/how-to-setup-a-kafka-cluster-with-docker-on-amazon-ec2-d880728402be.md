# 如何在 Amazon EC2 上用 Docker 设置 Kafka 集群

> 原文：<https://medium.com/analytics-vidhya/how-to-setup-a-kafka-cluster-with-docker-on-amazon-ec2-d880728402be?source=collection_archive---------9----------------------->

![](img/1b4d399922fb0957bad2d0c1214adc83.png)

照片由 Norja Vanderelst 在 Pexels 上拍摄

在本例中，我们将使用 Amazon EC2 实例设置一个包含两个节点的 Kafka 集群。然而，即使有些配置细节是 AWS 特有的，本教程中描述的说明也可以很容易地推广到更大的集群和其他部署环境。

⚠️ **警告** ⚠️:一旦你的 EC2 实例开始运行，**你的亚马逊账户将** …