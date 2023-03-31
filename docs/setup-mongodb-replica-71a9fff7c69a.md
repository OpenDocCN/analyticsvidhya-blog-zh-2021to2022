# 设置 MongoDB 副本

> 原文：<https://medium.com/analytics-vidhya/setup-mongodb-replica-71a9fff7c69a?source=collection_archive---------5----------------------->

![](img/983489711bda83a895da86ec78d97e0f.png)

来源:[https://stocksnap.io/](https://stocksnap.io/)

在本文中，我们将为现有的 mongoDB 或新的 mongoDB 设置一个副本节点。我们还将看到如何使用 Python 编程语言访问 mongoDB 副本设置。MongoDB 中的副本集是一组维护相同数据集的 mongod 进程。副本集提供冗余和高可用性，是所有生产部署的基础。

首先，我们需要两个实例来托管 mongoDB。一个实例将充当主实例，另一个实例将充当辅助实例。更多关于初级和次级的信息可以在[这里](https://docs.mongodb.com/manual/replication/)找到。一旦实例准备好了，我们就可以从这里的[下载 mongoDB。MongoDB 应该安装在这两个实例中。如果有人已经有了 mongoDB，就不需要重新安装了。必须停止现有的 mongoDB 服务，并且必须遵循以下步骤来设置 mongoDB 副本。](https://www.mongodb.com/try/download/community)

第一步:

使用以下命令在主实例中启动 mongoDB

```
mongod.exe --dbpath <"path/to/your/db"> --port <port number> --replSet "rs0" --bind_ip <localhost / 0.0.0.0>
```

示例:

```
mongod.exe --dbpath "D:\mongoData\db\data" --port 27017 --replSet "rs0" --bind_ip 0.0.0.0
```

`--dpath`和`--port`是可选的，如果没有提供，将采用默认的`dbpath`和`port`。`--replSet`是“mongod 所属的副本集的名称，副本集中的所有主机必须具有相同的集名称”。在我们的例子中，我们给定为`rs0`，这可以是任何名称，但是所有副本集必须使用相同的名称。`--bind_ip`是 mongod 应该监听客户端连接的套接字路径，要公开 mongod 进行远程访问，`bind_ip`必须是`0.0.0.0`。

第二步:

使用以下命令在辅助实例中启动 mongoDB

```
mongod.exe --dbpath <"path/to/your/db"> --port <port number> --replSet "rs0" --bind_ip <localhost / 0.0.0.0>
```

第三步:

在主实例中打开新的命令提示符或终端，并启动`mongo shell`

```
mongo.exe
```

第四步:

使用以下命令在 mongo shell 中启动副本集

```
rs.initiate( {
   _id : "rs0",
   members: [
      { _id: 0, host: "<instance1_ipaddress>:<instance1_port>" },
      { _id: 1, host: "<instance2_ipaddress>:<instance2_port>" }
   ]
})
```

上述命令将启动复制副本。属性`_id: “rs0”`是我们在启动 mongod 时提供的副本集名称。通过将实例信息附加到`members`属性，可以进一步添加辅助实例。

第五步:

在 mongo shell 中，检查副本的状态

```
rs.status()
```

这将显示有关主节点和辅助节点及其状态的信息。有时，辅助节点的状态可能是`"stateStr": STARTUP2`，这意味着从主节点到辅助节点正在进行同步。根据主数据库中现有数据的大小，这可能需要一些时间。一旦同步结束，次节点的状态将变为`"stateStr": SECONDARY`。

现在，我们将了解如何使用 Python 编程语言连接到副本设置。

必需的包:

```
pymongo
```

语法:

```
pymongo.MongoClient('mongodb://<username1>:<password1>@<ipaddress1>:<port1>,<username2>:<password2>@<ipaddress2>:<port2>/?replicaSet=<replicaSetName>')
```

示例 1 使用用户名和密码:

```
pymongo.MongoClient('mongodb://jeril:abc123@201.202.203.204:27017,jeril:abc123@201.202.203.212:27017/?replicaSet=rs0')
```

没有用户名和密码的示例 2:

```
pymongo.MongoClient('mongodb://201.202.203.204:27017,201.202.203.212:27017/?replicaSet=rs0')
```

代码:

```
import pymongo
client = pymongo.MongoClient('mongodb://jeril:abc123@201.202.203.204:27017,jeril:abc123@201.202.203.212:27017/?replicaSet=rs0')
print(client)
print()
print(client.nodes)
```

输出:

```
MongoClient(host=['201.202.203.204:27017', '201.202.203.212:27017'], document_class=dict, tz_aware=False, connect=True, replicaset='rs0')frozenset({('201.202.203.204', 27017), ('201.202.203.212', 27017)})
```

传递给 MongoClient()的地址称为种子。只要至少有一个种子在线，MongoClient 就会发现副本集中的所有成员，并确定哪个是当前的主节点，哪个是辅助节点或仲裁节点。

编码快乐！！！

参考资料:

1.  [MongoDB 文档](https://docs.mongodb.com/manual/replication/)

2. [Stackoverflow](https://stackoverflow.com/a/42530573/2825570)

3. [PyMongo 文档](https://pymongo.readthedocs.io/en/stable/examples/high_availability.html#id1)