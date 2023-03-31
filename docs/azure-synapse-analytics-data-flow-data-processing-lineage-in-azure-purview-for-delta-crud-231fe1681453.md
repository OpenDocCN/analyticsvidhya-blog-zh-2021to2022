# 用于 delta CRUD 的 Azure 权限中的 Azure Synapse analytics 数据流数据处理沿袭

> 原文：<https://medium.com/analytics-vidhya/azure-synapse-analytics-data-flow-data-processing-lineage-in-azure-purview-for-delta-crud-231fe1681453?source=collection_archive---------2----------------------->

# 使用 delta CRUD 操作的 Azure 权限中的数据流数据处理沿袭

# 注意

# 先决条件

*   Azure 帐户
*   Azure synapse 分析工作区
*   蔚蓝视界
*   把这两者联系起来
*   为 synapse analytics 托管身份提供权限贡献者
*   Azure 存储
*   将此报告中的 movidedb1.csv 从数据文件夹上传到名为 movidedincoming 的文件夹
*   创建一个名为 moviesoutput 的新文件夹

# 数据流活动

*   登录 Azure synapse analytics 工作区或工作室
*   首先上传数据

![](img/5f7911bd739e257779a4ad2cee9446af.png)

*   转到开发和创建数据流
*   创建新的数据流
*   这是整个流程

![](img/d411b1ebed803951a2b0af082d651cb2.png)

*   将源文件作为 csv 文件

![](img/61224f8bf4cc0155fe4512af88b4e493.png)![](img/3ae51d309ebfe15c89d124ad720cc188.png)

*   现在，我们将选择所需的列

![](img/04b7eb23569c064f1d43b3e4ef330a81.png)

*   接下来，我们将使用 delta 进行 CRUD 操作
*   因此，我们将从数据集中筛选出几年

![](img/780c53fe031afa81adcf65770df654a7.png)

```
YEAR==1960 || YEAR==1988 || YEAR==1950
```

*   现在我们将应用一些条件并导出列

![](img/3384296b245384a2a2798eda1e91544d.png)

*   用于评级

```
iif(YEAR==1998,1, toInteger(Rating))
```

*   多年来

```
iif(YEAR==1960, 2021, toInteger(YEAR))
```

*   对于电影

```
iif(YEAR==1998,toInteger(movies)+1000, toInteger(movies))
```

*   现在将 Alter row 用于 delta 的 CRUD 操作

![](img/480116d6cc0c6d97a88924a69c6bc0fe.png)

*   应用如上图所示的公式

```
YEAR==2021
YEAR==1998
YEAR==1950
```

*   现在通过插入、更新和删除下沉到增量位置

![](img/248b541c1fbfa32d7bdf16df7fbc3e6b.png)

*   配置设置

![](img/b8dfa8a585b703931606418c1a02cbb3.png)

*   选择 YEAR 和 movies 作为执行 CRUD 操作的关键字
*   现在创建一个新集成管道
*   拖放数据流活动，并选择上面创建的数据流

![](img/94723a0361a2d78ed89c9f2d2e8ccda6.png)

*   现在在调试模式下运行管道
*   给几分钟时间启动调试集群并运行上面的数据处理
*   查看输出
*   在发展中

![](img/e04e50ecd630d8ea02addb858abaa01b.png)

*   等到它完成

![](img/3eb1c233f6b1f705b22dbb8feca810ce.png)

*   检查输出

![](img/9d6c3215b7a266bc61e8b792ad9798fd.png)

*   检查接收器活动
*   检查接收器活动

![](img/5cd1c7c403038ecb173a0ef7c9d21e8c.png)

*   现在检查 delta 输出

![](img/bf830738060febbe56e442040f31f7ec.png)

*   现在转到无服务器 sql 查询和增量表，查看输出并进行验证
*   我将检查 2021 年，因为它在实际数据中不存在

```
SELECT TOP 10 *
FROM OPENROWSET(
    BULK 'https://storagename.dfs.core.windows.net/synapseroot/moviesoutgoing1/',
    FORMAT = 'delta') as rows Where YEAR = 2021;
```

![](img/4dcbcad5debde307ebaafe1d7ed784ef.png)

*   在工作区内，如果您有链接的权限，则转到搜索栏

![](img/e0a77bb963ae113538526abd3b23abae.png)

*   选择数据流活动
*   点击血统

![](img/b88e304db7c7ceedbeab131dc06c40e1.png)

# 蔚蓝视界

*   登录权限
*   转到浏览资产
*   选择 Azure synapse 分析
*   选择您的实例名称

![](img/a6cfd608759d1ed5d4332cc8ffab67c9.png)

*   现在选择带有数据流的管道

![](img/709234f4da94a0bf1d4f7dd1da871d10.png)

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Samples2021/blob/main/Synapseworkspace/dataflowlineage.md)*。*