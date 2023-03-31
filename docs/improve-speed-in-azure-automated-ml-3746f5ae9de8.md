# 提高 Azure 自动化 ML 的速度

> 原文：<https://medium.com/analytics-vidhya/improve-speed-in-azure-automated-ml-3746f5ae9de8?source=collection_archive---------13----------------------->

# 如何提高自动机器学习的性能

# 加速基于 CPU 的模型中的自动运行

# 用例

*   改善模型训练时间
*   使用 CPU
*   添加更多节点
*   提高并发性
*   成本优化
*   测试回归模型

# 介绍

*   选择 data.gov 的 NasaPred 数据
*   使用回归的剩余使用寿命模型
*   使用 2 个训练集群配置运行
*   称为 cpu-cluster1 的计算集群，具有 2 个节点 Standard_D14_v2 (16 个内核、112 GB RAM、800 GB 磁盘)
*   具有 4 个节点的计算集群称为 cpu 集群 Standard_F16s_v2 (16 个内核、32 GB 内存、128 GB 磁盘)
*   相同的数据集
*   相同的回归模型
*   相同的默认配置
*   计算集群详细信息

![](img/88afc6b51a1cffa476392b0750cedd48.png)

*   计算机群集运行详细信息

![](img/14081d30f3d91d66020d7a7fd387c913.png)

*   节点状态

![](img/bf43f369b55eb5bc7b04e89900bb7c70.png)

*   运行比较

![](img/22a0c2914071e71fb1a507d67225c43d.png)

*   第一轮是 3 个小时
*   第二轮是 23 分钟
*   4 节点配置下的默认计算运行

![](img/f40cad85c462a6636ef0bb45855d9eb1.png)

*   F16 的配置

![](img/99eca7c560ae90ac67a9b30715509bcf.png)

*   再次运行 AutoML 以查看它是如何运行的
*   接近 27 分钟

![](img/0035c3c6c4f7e322bd884ec8bf99f443.png)

原文：<https://github.com/balakreshnan/Samples2021/blob/main/AzureML/AutoMLoptspeed.md>