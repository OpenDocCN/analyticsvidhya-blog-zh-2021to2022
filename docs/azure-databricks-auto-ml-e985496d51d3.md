# Azure Databricks Auto ML

> 原文：<https://medium.com/analytics-vidhya/azure-databricks-auto-ml-e985496d51d3?source=collection_archive---------16----------------------->

# 在 Azure 数据块中使用 AutoML 特性

# 要求

*   Azure 帐户
*   Azure 存储帐户
*   Azure 数据块
*   使用的数据集是 titanic 开源数据集

# 步伐

*   使用 8.3beta ML 运行时创建集群

![](img/004e7b54b1676c03f516e42231fa4e7f.png)

*   等待群集启动
*   一旦开始，你就可以看到数据
*   创造新的实验

![](img/b39713b730d44d899377ce80eeddbb53.png)

*   现在将 ML 类型配置为分类
*   浏览表格并选择默认，然后选择 titanictbl
*   要创建表，请将 titanic.csv 文件上传到名为 titanictbl 的表中
*   数据见 data\titanic.csv

![](img/0ab4e9f37be19644b92fb0acd3b5a590.png)

*   高级功能

![](img/e786b7674e87b7858246d82bdfe88ad5.png)

*   进行实验

![](img/75dd11c08880cb858da2a106e7aadcbe.png)

*   列出所有运行的模型

![](img/0ec3027c9feaacfc08d09dae257821ab.png)

*   最佳模特

![](img/10cd3f1dfdac96f1167929569c598a95.png)

*   数据工作手册—特征工程

![](img/84c7f0bcbd8a56f65425230add205f75.png)![](img/fc5370f943d11a86a945b6ab5c329172.png)![](img/3c359f19161583bf192cd4c63fd42862.png)![](img/0cd57f5f558f37922811154baebb2638.png)![](img/6eaefeecddf7c0ece7e42a4adddbddf4.png)![](img/412e63a69d52db47d0839435f3447d7a.png)

*   相关

![](img/48824bde849d455d4a3a8d8e4c8a0a74.png)

*   [samples 2021/adbautoml . MD at main balakreshnan/samples 2021(github.com)](https://github.com/balakreshnan/Samples2021/blob/main/adb/adbautoml.md)