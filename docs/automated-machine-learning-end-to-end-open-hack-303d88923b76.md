# 自动机器学习端到端开放黑客

> 原文：<https://medium.com/analytics-vidhya/automated-machine-learning-end-to-end-open-hack-303d88923b76?source=collection_archive---------16----------------------->

# 公民数据科学开放黑客

# 介绍

自动化机器学习黑客马拉松:学习如何使用自动化机器学习构建模型并部署到生产中。

# 议程

*   Openhack 简介
*   Azure 机器学习简介— 1 小时
*   机器学习操作简介— 1 小时
*   Openhack 使用案例介绍—4 小时
*   部署模型— 1 小时
*   清理— 15 分钟
*   回顾— 1 小时

# 用例

*   预测未来几年的人口增长。预测人口让经济学家知道如何在全球建立下一代供应链。
*   这些信息也使国家和州政府能够为医疗保健、消费者需求甚至城市发展规划未来。
*   构建端到端的管道
*   使用 Azure 数据工厂将数据从源移动到 Azure
*   在迁移到 Azure 存储时处理数据
*   我们可以使用数据工厂中的数据流来处理数据，以便在机器学习中使用
*   ETL/数据工程是数据处理的范围
*   不需要为 ML 算法格式化数据
*   表格数据就够了
*   确保所有功能和标签都可用
*   展示数据运营+ ML 运营
*   端到端流程来获取数据和处理，然后创建机器学习模型和消费
*   注意:模型的准确性并不重要
*   我们用回归作为样本来预测人口
*   假设是 Azure 开放数据集中的源数据并验证配置

# 体系结构

![](img/7a8f5b8b8b7c113e2204134ff56763d0.png)

# 蔚蓝资源

*   Azure 账户—[https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F)
*   创建一个名为 automlopenhack 的资源组—[https://docs . Microsoft . com/en-us/azure/azure-Resource-manager/management/manage-Resource-groups-portal # create-Resource-groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
*   创建名为 autoloutput—[https://docs . Microsoft . com/en-us/Azure/Storage/common/Storage-account-create 的 Azure 存储帐户？tabs=azure-portal](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal)
*   创建 Azure 数据工厂—autoladfopenhack—[https://docs . Microsoft . com/en-us/Azure/Data-Factory/quick start-create-Data-Factory-portal # create-a-Data-Factory](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)
*   Azure 开放数据集配置
*   创建 Azure 机器学习服务—[https://docs . Microsoft . com/en-us/Azure/machine-learning/quick start-create-resources](https://docs.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources)
*   还要创建一个容器注册表来存储模型 pickle 文件
*   创建计算集群—[https://docs . Microsoft . com/en-us/azure/machine-learning/how-to-create-attach-compute-cluster？tabs=python](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-create-attach-compute-cluster?tabs=python)
*   为 AKS 创建推理集群—[https://docs . Microsoft . com/en-us/azure/machine-learning/how-to-create-attach-kubernetes？tabs = azure-portal # create-a-new-aks-cluster](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-create-attach-kubernetes?tabs=azure-portal#create-a-new-aks-cluster)
*   创建的资源列表

![](img/76d19c0d6d5455e62db23268df2f997d.png)

# 步伐

*   创建数据工厂复制管道
*   创建 blob 存储
*   https://azureopendatastorage.blob.core.windows.net/[URI](https://azureopendatastorage.blob.core.windows.net/)
*   对于 SAS 密钥，请在“”中键入
*   测试连接，看连接是否成功
*   创建 blob 源
*   将输入另存为 azureopendataset

![](img/71507ee0f72a414f56880dd90b6371ab.png)![](img/cec3f74193a875b0286b857381f26001.png)

*   选择容器
*   选择或键入容器名称“censusdatacontainer”
*   键入或选择文件夹名“release/us_population_zip/”
*   对于文件名类型为" *。如果你看到下面的文件，选择一个正确的如下
*   文件的名称和类型可以改变——因为它是一个开源数据集。

![](img/e82e11d905a310174b043b5f01b92638.png)![](img/e4ff46de65e0b61c4119e1bc23c90dcc.png)![](img/6c5f0453580820fe11c9f5f328b31543.png)

*   将其命名为 ADLSoutput
*   选择订阅和存储帐户名

![](img/c63a3ddf8aa4883bdfd60cdeea4a3972.png)![](img/20c64bee7bd3686dfffd27b8f476cf73.png)

*   单击下一步和下一步
*   保留所有内容为默认值
*   单击完成

![](img/2cd756c3980dc04e4a281e30581b1ec6.png)![](img/85c7bb22f0f5a2003d17f6c0490906a6.png)![](img/4881ad76ad3b50937d82b5332fb8edd8.png)

有时需要 15 到 20 分钟或更长时间，具体取决于数据大小。为了提高速度，我们可以使用更多的 DIU 单元进行复制，或者并行化复制活动。

# 模型开发

# 现在来看看 Azure 机器学习系统

*   让我们创建一个设置
*   首先创建数据集和数据存储

![](img/2d1dd03814dfcdec99f28dbe0b20f2af.png)![](img/c93cc9d1fba8d487b3184737eb6e6995.png)![](img/c54252f1a803241afe97b813ec8d6407.png)![](img/b6a06680c4d3899923edd1feec98efbd.png)![](img/ed3df02e1d038a45de7c953801db39e8.png)![](img/d2fe56d8a4c9709f10d3aaffe82daf44.png)![](img/c4536ca27e840995659f7fccfc4e4919.png)![](img/a67d92e81daab40a4990d944242651ba.png)

*   创建计算 cpu-compute
*   使用 0 到 2 个节点是好

![](img/86cff15d17c04d30bb4d261ec4460ff9.png)![](img/c0ced3c7b57f9ce500370466d8f2f6eb.png)![](img/c026e97cd8291c5f331e9162a361bde4.png)![](img/d89c41cc8c843de6bd78b602921422ed.png)![](img/a3703624fd72f9e8acc335cdcf9d3b12.png)

*   一旦实验完成
*   通常需要 2 到 3 个小时

![](img/63b4c6211674e87b2f7ede33624bd53d.png)![](img/316ad7b7b47e3f326e169642082e6e4d.png)![](img/c6b84f8bb73f0a9326ceedfff30fe140.png)![](img/5d373f72d8f4a37445d714969cf295dd.png)![](img/c0a394f16d2589565c3659295d7ee6b6.png)![](img/9126284fb83e0ab55233d6c019e68191.png)

*   检查解释或功能重要性，查看哪些列对预测影响最大
*   日志模型信息

![](img/83c49701e0e24c056b03e81f67086ca5.png)

*   这些是我们需要使用的列

# 模型部署

*   选择最佳模型并部署
*   部署到 AKS 集群
*   创建 AKS 集群
*   转到计算并创建推理群集

![](img/25ceda5ce1ebd03f250ad11c466c78dc.png)

*   选择虚拟机配置
*   我选择开发/测试，因为这是为了黑客马拉松
*   对于生产说明，必须为 HA 创建至少 12 个节点的 AKS 群集
*   生产配置如下

![](img/d5549f3bc11984e7c3814104398c7f5e.png)![](img/46bb1c8a879b6b83537a7d7a6181fc0f.png)

*   然后单击创建
*   AKS 集群创建已开始

![](img/dc91eff77364dee0c3a097b2f29dd5ab.png)![](img/9832aa5519de740b4e0b8e23519533be.png)![](img/348059cdb8e7d26b80e39b1ccddecc94.png)![](img/1a2f7a586199dc5ff33f34db74d17fa3.png)![](img/f26de97143bb64677e068fd596600f80.png)

*   单击左侧的 aksdeploy 可成功转到端点信息
*   创建后，将会创建一个端点

![](img/3902a7b2ea4ef08d2458f6afaa300ce7.png)

*   单击测试选项卡
*   键入以下信息

![](img/84b277b34f61df18e2985251190d47bc.png)

```
decennialTime : 2020 zipCode: 77480 race: WHITE ALONE sex: Female minAge: 56 maxAge: 59
```

![](img/8cbe2b994d8f8fdaf422864834405aeb.png)![](img/8b544b637b170e846c0f391429ecbefb.png)![](img/2009cccb108b686ffc6ca38f1d0efc36.png)

# 清除

*   删除所有资源
*   删除整个资源组以删除所有组件

# 评论和反馈

*原载于*[*https://github.com*](https://github.com/balakreshnan/AutomatedML/blob/main/Beginner/Intro.md)*。*