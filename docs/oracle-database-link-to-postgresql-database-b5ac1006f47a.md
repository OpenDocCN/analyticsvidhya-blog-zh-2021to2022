# Oracle 数据库链接到 PostgreSQL 数据库

> 原文：<https://medium.com/analytics-vidhya/oracle-database-link-to-postgresql-database-b5ac1006f47a?source=collection_archive---------3----------------------->

今天的话题让我的一周变得有趣。我挣扎了一下，但在我的 stackoverflow.com 兄弟的帮助下，我做到了:)。我不会用所有的细节来烦你。我在工作中遇到的错误是关于数据库编码的。我从一开始就找错了地方。我会在最后分享问题链接。我希望你不要犯同样的错误(对于像我一样不知道编码的人:)。我们还有路要走:)。)

首先，我的 Oracle 数据库字符集是 WE8ISO8859P9，换句话说就是 LATIN5。因此，我的 PostgreSQL 数据库应该根据这个配置进行配置。否则，你会在你的客户端上看到“ahap”。

为了创建具有正确编码的 PostgreSQL 数据库，我们必须事先做好准备。我们的 PostgreSQL 数据库运行在 Ubuntu 18.04.5 LTS(仿生海狸)上。我们的系统应该知道正确的区域设置，所以我们可以用 tr_TR.iso88599 创建数据库。

从/etc/locale.gen 文件中删除包含“TR _ TR ISO-8859–9”(作为 root)的行的注释。编辑完文件后，只需执行下面的命令(以 root 用户身份):

```
root@hostname:~# locale-gen
Generating locales (this might take a while)…
 en_US.UTF-8… done
 tr_TR.ISO-8859–9… done
 tr_TR.UTF-8… done
Generation complete.
```

它将生成我们的语言环境。现在我们可以用正确的编码创建数据库了。

```
CREATE DATABASE turkish_charset ENCODING=’LATIN5' LC_COLLATE=’tr_TR.iso88599' LC_CTYPE=’tr_TR.iso88599' TEMPLATE=template0;
```

现在，我们可以转到 Oracle 方面了。我的 Oracle 数据库运行在 Oracle Linux Server 版上。

安装所需的 PostgreSQL ODBC 驱动程序:

```
sudo apt-get install odbc-postgresql
```

此外，您需要一个 ODBC 驱动程序管理器。你可以从[http://www.unixodbc.org/](http://www.unixodbc.org/)安装，就像他们在下载页面上说的那样。

我们正在处理两个 ODBC 文件，以提供异构连接。第一个/etc/odbc.ini 文件。该文件包含:

```
[postgres]
Description = postgres
Driver = /usr/lib64/psqlodbc.so
Servername = $database_server
Username = $database_user
Password = $database_user_password
Port = 5432
Database = turkish_charset($postgresql_db_name)
```

我运行 Oracle 数据库的操作系统是基于 x64 的。因此，我将 driver 视为“Driver = /usr/lib64/psqlodbc.so”。

第二个文件/etc/odbcinst.ini 包含:

```
# Example driver definitions# Driver from the postgresql-odbc package
# Setup from the unixODBC package
#[PostgreSQL]
[postgres]
Description = ODBC for PostgreSQL
Driver = /usr/lib64/psqlodbc.so
Driver64 = /usr/lib64/psqlodbc.so
Setup = /usr/lib64/libodbcpsqlS.so
Setup64 = /usr/lib64/libodbcpsqlS.so
FileUsage = 1
```

像/etc/odbc.ini 文件一样，我在这个文件中使用了基于 x64 的库。我们可以检查我们的 ODBC 配置:

```
Last login: Mon Jan 25 01:10:50 +03 2021
[oracle@hostname ~]$ isql -v postgres
+ — — — — — — — — — — — — — — — — — — — -+
| Connected! |
| |
| sql-statement |
| help [tablename] |
| quit |
| |
+ — — — — — — — — — — — — — — — — — — — -+
SQL>
```

将 TNS 条目添加到＄ORACLE _ HOME(database)/network/admin/tnsnames . ora

```
postgres =
 (DESCRIPTION =
 (ADDRESS = (PROTOCOL = TCP)(HOST = $database_server)(PORT = 1521))
 (CONNECT_DATA =
 (SERVER = DEDICATED)
 (SID = postgres)
 )
 (HS = OK)
 )
```

正在创建初始化文件。该文件的名称至关重要。因为，当你通过 db link 调用另一个数据库时，它首先会转到 TNS 文件。我不知道你是否指出了括号中的名字(提示:[postgres])。它们是服务名。它们应该与 TNS 条目的 SID 值相同，我们的 init 文件也被称为 init **postgres** 。奥拉。所以，init **postgres** 的内容。ora 文件:

```
# Linux
HS_FDS_CONNECT_INFO = **postgres**
HS_FDS_TRACE_LEVEL = ON
HS_FDS_SHAREABLE_NAME = /usr/lib64/libodbc.so
set ODBCINI=/etc/odbc.ini
set ODBCINSTINI=/etc/odbcinst.ini
HS_NLS_LENGTH_SEMANTICS=BYTE
NLS_LANGUAGE=AMERICAN
NLS_TERRITORY=AMERICA
NLS_CHARACTERSET=WE8ISO8859P9
```

注意:我没有提到在哪里创建这个 init 文件。因为它是一个异构源，所以我们需要在$ORACLE_HOME/hs/admin 目录中创建它。感谢 [**Stephan**](/@herphan?source=user_profile-------------------------------------) 。

HS _ FDS _ 连接 _ 信息参数也获得与 TNS 条目的 SID 值相同的值。

我们应该编辑的最后一个文件是$ORACLE_HOME(如果正在使用网格，则为数据库)/network/admin/listener.ora 文件。在行下方添加:

```
#Added manually for dblink 
SID_LIST_LISTENER =
 (SID_LIST =
 (SID_DESC =
 (ORACLE_HOME=/u01/app/oracle/product/12.1/dbhome1)
 (ENV=”LD_LIBRARY_PATH=/u01/app/oracle/product/12.1/dbhome1/lib:/usr/local/unixodbc/lib:/usr/lib:/usr/lib64")
 (SID_NAME=postgres)
 (PROGRAM=dg4odbc)
 )
 )
```

SID_LIST_LISTENER 的 SID 值也获得与 TNS 条目的 SID 值相同的值。

最后一步，创建数据库链接:

```
CREATE DATABASE LINK POSTGRELINK
 CONNECT TO “ $remote_postgresql_database_user”
 IDENTIFIED BY $remote_postgresql_database_user_password
 USING ‘postgres($TNS_ENTRY_NAME)’;
```

PostgreSQL 端:

```
=# select * from test;
 username 
 — — — — — — — -
 fıstıkçışahap
(1 row)postgres=#
```

Oracle 端:

```
select * from “test”[@postgrelink](http://twitter.com/postgrelink); — — due to case sensivityfıstıkçışahap
```

我的错误:[https://stack overflow . com/questions/65861426/Oracle-client-charset](https://stackoverflow.com/questions/65861426/oracle-client-charset)

谢谢:)！