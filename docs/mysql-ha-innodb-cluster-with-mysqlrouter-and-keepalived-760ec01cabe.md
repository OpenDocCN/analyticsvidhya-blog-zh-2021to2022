# 带有 MySQLRouter 和 KeepAlived 的 MySQL HA InnoDB 集群

> 原文：<https://medium.com/analytics-vidhya/mysql-ha-innodb-cluster-with-mysqlrouter-and-keepalived-760ec01cabe?source=collection_archive---------8----------------------->

大家好，

我今天在这里有一个新的话题:)。

我要写的是 HA MySQL InnoDB 集群。开始吧！

首先，我有 6 台服务器，其中 3 台用于 MySQL 实例，2 台用于 mysqlrouter，最后一台用于集群管理和监控(我们使用 MySQL Enterprise Monitor 进行监控)。

以下是主机名:

MySQL 实例:

**inno1
inno2
inno3**

mysqlrouter 服务器:

**MySQL router 1
MySQL router 2**

集群管理和监控服务器:

**inno-server-mon**

为了配置 innodb 集群，我们需要对我们的服务器进行一些更改。

1.  禁用 selinux

为了做到这一点:

> **vi /etc/selinux/config**

我注释掉了由“#SELINUX=enforcing”参数组成的行，并添加了以下参数:

> **SELINUX =禁用**

2 禁用防火墙(如果您有 IP 表配置您的设置)

要禁用防火墙:

> **系统 ctl 停止防火墙 d**

我跳过了在每个数据库服务器上安装 MySQL 服务器，因为这不是我们的主题。

对了，我们用的 MySQL 版本是 8.0.23。

安装 MySQL 服务器后，我们创建一个用户来管理所有数据库实例:

> **创建由“your_password”标识的用户“root”@“%”；
> 授予*的所有权限。* TO 'root'@'% '带有 GRANT 选项；
> 同花顺特权；**

我们将部署一个托管的 InnoDB 集群，因此我们需要安装 mysqlsh。在我的 **inno-server-mon** 服务器上:

如果您没有 MySQL yum 存储库，您应该按照以下步骤操作:

 [## 使用 MySQL Yum 存储库的快速指南

### 首先，将 MySQL Yum 存储库添加到系统的存储库列表中。按照以下步骤操作:转到下载页面…

dev.mysql.com](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/#repo-qg-yum-repo-setup) 

sudo yum 更新

sudo yum 安装 mysql-shell

成功安装 mysql-shell 后，在 inno-server-mon 上

> **mysqlsh—uri root @ inno 1:3306—用户 root**

现在，我们可以检查我们的 MySQL 服务器实例是否适合 InnoDB 集群。

> **DBA . check instance configuration(' root @ inno 1:3306 ')
> DBA . check instance configuration(' root @ inno 2:3306 ')
> DBA . check instance configuration(' root @ inno 3:3306 ')**

如果实例的配置不符合要求，您可以使用以下命令配置实例:

> **DBA . configure instance(' root @ inno 1:3306 ')
> DBA . configure instance(' root @ inno 2:3306 ')
> DBA . configure instance(' root @ inno 3:3306 ')**

然后，您可以使用以下命令再次检查配置:

> **DBA . check instance configuration(' root @ inno 1:3306 ')
> DBA . check instance configuration(' root @ inno 2:3306 ')
> DBA . check instance configuration(' root @ inno 3:3306 ')**

示例输出:

> **验证 inno1:3306 上的 MySQL 实例，以便在 InnoDB 集群中使用…**
> 
> **该实例报告其自己的地址，因为 inno1:3306
> 默认情况下，客户端和其他集群成员将通过该地址与其通信。如果不正确，应该更改 report_host MySQL 系统变量。**
> 
> **检查现有表是否符合组复制要求……
> 未检测到不兼容的表**
> 
> **检查实例配置…
> 实例配置与 InnoDB 集群兼容**
> 
> 实例“inno1:3306”可以在 InnoDB 集群中使用。
> 
> **{
> “状态”:“正常”
> }**

现在，我们可以创建一个集群:

> **var cluster = DBA . create cluster(' myinno cluster ')
> 验证 root@inno1:3306…
> 此实例报告其自己的地址，因为 ic-1
> 实例配置是合适的。
> 在“root@inno1:3306”上创建 InnoDB 集群“test Cluster”…
> 添加种子实例…
> 集群已成功创建。使用 Cluster.addInstance()添加 MySQL 实例。
> 集群至少需要 3 个实例才能承受一个服务器故障。**

要添加新实例:

> **cluster . add instance(' root @ inno 2:3306 ')
> cluster . add instance(' root @ inno 3:3306 ')**

由于前面提到的 MySQL 版本(8.0.23 ),我得到了类似于 2 号实例(inno2)的输出:

> **验证 inno2:3306 处的实例…
> 该实例报告其自己的地址，因为 inno2:3306
> 实例配置是合适的。
> 一个新实例将被添加到 InnoDB 集群中。根据集群上的
> 数据量，这可能需要几秒到几个小时。
> 将实例添加到集群…
> 监视新集群成员的恢复过程。按下^C 停止监测，让它继续在后台。
> 基于克隆的状态恢复正在进行中。
> 注意:服务器重启是克隆过程的一部分。如果
> 服务器不支持重启命令或者在
> 一段时间后没有恢复，您可能需要手动重启。
> *等待克隆完成…
> 注:inno2:3306 正在从 inno1:3306 克隆
> * * Stage DROP DATA:Completed
> * *克隆传输
> 文件复制########### # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # 100%完成
> 页面复制# # # # # # # # # # # 正在等待克隆完成…
> **阶段重新启动:已完成
> *克隆过程已完成:在 38 秒内传输了 1.2 GB(30.26 MB/s)
> 已完成“inno2:3306”的状态恢复
> 实例“inno2:3306”已成功添加到群集**

为了检查集群的状态，我们使用 cluster.status()

> **JS>Cluster . status()
> {
> " Cluster name ":" myinnocluster "，
> " defaultReplicaSet ":{
> " name ":" default "，
> "primary": "inno1:3306 "，
> "ssl": "REQUIRED "，
> "status": "OK "，
> "statusText ":"集群在线，最多可以容忍一个故障。"、
> "拓扑":{
> "inno1:3306": {
> "地址":" inno1:3306 "、
> "模式":" R/W "、
> "readReplicas": {}、
> "replicationLag": null、
> "role": "HA "、
> "status": "ONLINE "，
> "version": "8.0.23"
> }、"**

我们可以设置一个新的主实例:

> **JS>cluster . setprimaryinstance(" inno2 ")
> 将实例' inno 2 '设置为集群' myinnocluster '的主实例……**
> 
> **实例‘inno 3:3306’仍然是次要的。
> 实例‘inno 1:3306’已从主实例切换到辅助实例。
> 实例‘inno 2:3306’已从辅助节点切换到主节点。**
> 
> **警告:集群内部会话不再是主要成员。对于集群管理操作，请使用 dba.getCluster()获取新的集群句柄。**
> 
> **实例“inno2”成功当选为主实例。**

再次检查状态:

> **JS>Cluster . status()
> {
> " Cluster name ":" myinnocluster "，
> " defaultReplicaSet ":{
> " name ":" default "，
> "primary": "inno2:3306 "，
> "ssl": "REQUIRED "，
> "status": "OK "，
> "statusText ":"集群在线，最多可以容忍一个故障、
> "拓扑":{
> "inno1:3306": {
> "地址":" inno1:3306 "、
> "模式":" R/O "、
> "readReplicas": {}、
> "replicationLag": null、
> "role": "HA "、
> "status": "ONLINE "，
> "version": "8.0。**

我们可以为每个数据库实例设置 memberweight 参数，以指示发生故障转移时应该如何进行主要选择。

> **cluster . setinstanceoption(" inno 1:3306 "，" memberWeight "，100)
> cluster . setinstanceoption(" inno 2:3306 "，" memberWeight "，50)
> cluster . setinstanceoption(" inno 3:3306 "，" memberWeight "，25)**

为了查看当我在 inno2 数据库服务器上重新启动 mysqld 服务时，集群是否能够成功进行故障转移:

> **JS>Cluster . status()
> {
> " Cluster name ":" myinnocluster "，
> " defaultReplicaSet ":{
> " name ":" default "，
> "primary": "inno1:3306 "，
> "ssl": "REQUIRED "，
> "status": "OK "，
> "statusText ":"集群在线，最多可以容忍一个故障。1 个成员不活动。、
> "拓扑":{
> "inno1:3306": {
> "地址":" inno1:3306 "、
> "模式":" R/W "、
> "readReplicas": {}、
> "replicationLag": null、
> "role": "HA "、
> "status": "ONLINE "、
> "version": "8.0.23"
> }、**

我们可以看到，由于优先级 inno1 被选为我们新的主实例。

群集完成恢复过程后:

> **JS>Cluster . status()
> {
> " Cluster name ":" myinnocluster "，
> " defaultReplicaSet ":{
> " name ":" default "，
> "primary": "inno1:3306 "，
> "ssl": "REQUIRED "，
> "status": "OK "，
> "statusText ":"集群在线，最多可以容忍一个故障。1 个成员不活动。、
> "拓扑":{
> "inno1:3306": {
> "地址":" inno1:3306 "、
> "模式":" R/W "、
> "readReplicas": {}、
> "replicationLag": null、
> "role": "HA "、
> "status": "ONLINE "、
> "version": "8.0.23 "【中**

下一部分是配置 MySQL 路由器。我说路由器是因为我们的设置与 MySQL 推荐的不同。MySQL 建议最好在应用服务器上设置 mysqlrouter，以便使用套接字连接。
但是在企业环境中，我们无法访问我们的应用服务器，也无权管理应用服务器。

这就是为什么我们通过虚拟 IP 配置 2 个 mysqlrouter 来加强我们高可用性架构。

我们需要安装 keepalive，以便在两台 mysqlrouter 服务器上配置虚拟 IP:

> **百胜安装 keepalived**

安装后，我们可以将第一个节点配置为主节点:

我得到了原始 keepalived 配置文件的副本:

> **CD/etc/keepalived/
> CP keepalived . conf keepalived . conf _ bck
> >keepalived . conf
> VI keepalived . conf**

我在文件中添加了以下配置:

> **！保持激活的配置文件**
> 
> **global _ defs {
> notification _ email {** [**root@mydomain.com**](mailto:root@mydomain.com) **}
> notification _ email _ from**[**svr1@mydomain.com**](mailto:svr1@mydomain.com) **SMTP _ server localhost
> SMTP _ connect _ time out 30
> }**
> 
> **vrrp_instance VI_1 {
> 状态主
> 接口 ens 192
> virtual _ router _ id 41
> 优先级 200
> advert_int 1
> 认证{
> auth _ type PASS
> auth _ PASS 1066
> }
> virtual _ IP address {
> your _ virtual _ IP _ address
> }
> }**

然后，我对第二个实例做了一些更改，因为第二个实例将是备份实例:

**global _ defs {
notification _ email {** [**root@mydomain.com**](mailto:root@mydomain.com) **}
notification _ email _ from**[**svr2@mydomain.com**](mailto:svr2@mydomain.com) **SMTP _ server localhost
SMTP _ connect _ time out 30
}**

**vrrp_instance VRRP1 {
状态备份
#指定分配了虚拟地址的网络接口
接口 ens 192
virtual _ router _ id 41
#将备份服务器上的优先级值设置为低于主服务器上的优先级值
优先级 100
advert_int 1
身份验证{
auth _ type PASS
auth _ auth**

让我们检查 keepalived 服务！

在 mysqlrouter1 上:

> **systemctl 状态 keepalived
> ●keepalived . service—LVS 和 VRRP 高可用性监视器
> Loaded:Loaded(/usr/lib/systemd/system/keepalived . service；已启用；厂商预置:禁用)
> 活跃:活跃(运行中)自周四 2021–05–06 12:09:23+03；12s 前
> 进程:10535 execstart =/usr/sbin/keepalived $ keepalived _ options(code = exited，status=0/SUCCESS)
> 主 PID: 10538 (keepalived)
> 任务:2
> c group:/system . slice/keepalived . service
> ├─10538/usr/sbin/keepalived-d
> └─10539/usr/sbin/keepalived-d**
> 
> **5 月 06 日 12:09:26 MySQL router 1 Keepalived _ vrrp[10539]:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> 5 月 06 日 12:09:26 MySQL router 1 Keepalived _ vrrp[10539]:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> 5 月 06 日 12:09:26 MySQL router 1 Keepalived _ 26 在 ens192 上为 your_virtual_ip_address 发送/排队免费 ARP
> May 06 12:09:31 MySQL router 1 Keepalived _ vrrp【10539】:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> May 06 12:09:31 MySQL router 1 Keepalived _ vrrp【10539】:在 ens192 上为 your_virtual_ip_address 发送免费 ARP【T19**

在 mysqlrouter2 上:

> **systemctl 状态 keepalived
> ●keepalived . service—LVS 和 VRRP 高可用性监视器
> Loaded:Loaded(/usr/lib/systemd/system/keepalived . service；已启用；厂商预置:禁用)
> 活跃:活跃(运行中)自周二 2021–04–27 15:26:07+03；1 周 1 天前
> 主 PID: 763 (keepalived)
> 任务:2(限制:12438)
> 内存:6.8m
> c group:/system . slice/keepalived . service
> ├─763/usr/sbin/keepalived-d
> └─764/usr/sbin/keepalived-d**
> 
> 4 月 27 日 19:16:13 MySQL router 2 Keepalived _ vrrp[764]:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> :4 月 27 日 19:16:18 MySQL router 2 Keepalived _ vrrp[764]:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> 在 ens192 上为 your_virtual_ip_address 发送/排队免费 ARP
> 4 月 27 日 19:16:18 MySQL router 2 Keepalived _ vrrp【764】:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> 4 月 27 日 19:16:18 MySQL router 2 Keepalived _ vrrp【764】:在 ens192 上为 your_virtual_ip_address 发送免费 ARP
> 4 月 27 日 19:

一切就绪，我们可以配置 MySQL 路由器了:

> **MySQL router—bootstrap root @ MySQL _ cluster _ rw _ node:3306—directory/var/lib/my router—conf-bind-address 0 . 0 . 0 . 0—conf-base-port 3307-u = MySQL router**

让我们讨论一下用于引导 mysqlrouter 的参数。

使用“-u mysqlrouter”参数，我们表示用户 mysqlrouter 启动服务。

使用“--conf-bind-address 0 . 0 . 0 . 0”参数，我们的服务将监听这台机器上的每个网络接口。我们可以设置我们的虚拟 IP，但是有一个问题。
因为，在备份节点上 mysqlrouter 服务不起作用。虚拟 IP 一次只能在一台路由器服务器上处于活动状态。因此，对于备份服务器(如果两个 mysqlrouter 服务器上的 keepalived 服务
都成功运行),它没有要监听的虚拟 IP，因此不会启动 mysqlrouter 服务。这就是为什么我们使用“0.0.0.0”

使用“--directory/var/lib/my router”参数，我们的意思是 mysqlrouter 将存储它的配置文件。

稍后我们将讨论“— conf-base-port 3307”参数。

输出:

> **请为 dba 输入 MySQL 密码:
> #在“/var/lib/my Router”…**重新配置 MySQL 路由器实例
> 
> **-从密匙环
> 中获取当前帐户(mysql_router22_bi1dzf4jbgp2)的密码执行语句失败:“执行 mysql 查询时出错”插入 MySQL _ innodb _ Cluster _ metadata . v2 _ routers(address，product_name，router_name)值(' mysqlrouter1 '，' ')”):MySQL 服务器正在使用-super-read-only 选项运行，因此它无法执行此语句(1290)' (1290) 如果有)
> -使用“/var/lib/myrouter/data”目录中的现有证书
> -验证帐户(使用它来运行将由路由器运行的 SQL 查询)
> -将帐户存储在密匙环中
> -调整所生成文件的权限
> -创建配置/var/lib/my Router/MySQL Router . conf**
> 
> **#为 InnoDB 集群“myinnocluster”配置的 MySQL 路由器**
> 
> **使用生成的配置启动 MySQL 路由器后**
> 
> **$ MySQL router-c/var/lib/my router/MySQL router . conf**
> 
> **集群“myinnocluster”可以通过连接到:**来访问
> 
> **## MySQL 经典协议**
> 
> **-读/写连接:本地主机:3307
> -只读连接:本地主机:3308**
> 
> **## MySQL X 协议**
> 
> **-读/写连接:本地主机:3309
> -只读连接:本地主机:3310**

在使用命令“ **mysqlrouter — bootstrap …** ”配置 mysqlrouter 实例之后，我们使用/var/lib/myrouter/start.sh 启动 mysqlrouter。

> **/var/lib/my router/start . sh**
> **日志记录工具初始化，将日志记录切换到配置**中指定的记录器

现在，我们可以将我们的 VIP 用作主机名，将 3307 用作读/写连接的端口
，将我们的 VIP 用作主机名，将 3308 用作只读连接的端口。

这没问题，但是 mysqlrouter 不是服务。因此，在 mysqlrouter 服务器重启后，它不会自动运行(至少在我们提供的配置下)。

我们还需要配置 mysqlroute 服务。在两台 mysqlrouter 服务器上:

> **VI/etc/systemd/system/MySQL router . service**

将线放在下面:

> **描述=MySQL 路由器
> After = network . target
> After = syslog . target**
> 
> **【服务】
> 类型=通知
> 用户=mysqlrouter
> 组=mysqlrouter**
> 
> **#启动主服务
> ExecStart =/usr/bin/MySQL router-c/var/lib/my router/MySQL router . conf**
> 
> **#设置打开文件限制
> 限制文件= 10000**
> 
> **重启=开-故障**
> 
> **【安装】
> wanted by =多用户.目标**

然后，

> **systemctl 守护进程-重载
> system CTL enable MySQL router . service
> system CTL start MySQL router . service**
> 
> **苏— mysqlrouter**
> 
> **systemctl 状态 MySQL router . service
> ●MySQL router . service
> Loaded:Loaded(/etc/systemd/system/MySQL router . service；已启用；厂商预置:禁用)
> 活动:活动(运行)自周四 2021–05–06 13:31:18+03；9s 前
> 主 PID: 12768 (mysqlrouter)
> 任务:25
> c group:/system . slice/MySQL router . service
> └─12768/usr/bin/MySQL router-c/tmp/my router/MySQL router . conf**

如果您想以 root 或任何其他用户的身份启动 mysqlrouter 服务，除了 mysqlrouter，您需要做一些小的更改。
我们必须注释掉用户和组参数，如:

> **【服务】
> Type =通知
> # User = MySQL router
> # Group = MySQL router**

现在，我们也可以以 root 用户身份启动 mysqlrouter 服务。

感谢阅读:)。