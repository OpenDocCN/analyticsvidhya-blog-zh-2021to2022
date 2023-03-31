# Postgres 中的数据库大小增加:自动清空

> 原文：<https://medium.com/analytics-vidhya/database-size-increase-in-postgres-9a91134db56?source=collection_archive---------2----------------------->

[PostgreSQL](https://www.postgresql.org/about/) ，也称为 Postgres，是一个强调可扩展性和 SQL 合规性的免费开源关系数据库管理系统。它是最受欢迎的开源数据库之一，因其性能和可靠性而被广泛使用，是 Oracle MySQL 服务的替代产品。

![](img/cbcb2bb103189dbb6c23f84465f9049d.png)

为了提高性能，它使用了某些机制，这些机制最终可能会给一些极端用户带来问题。一个这样的例子是增加数据库的大小。

# 数据库大小增加

让我们试着想象一个案例。在过去的 6 个月中，您的应用程序在一个数据库大小为 100 GB 的 Postgres 服务器上运行良好。突然，您被警告数据库的大小在一夜之间翻了一番…而且还在增加。

在检查数据库时，表的大小没有增加。行数也没有增加。就好像有什么东西在你不知情的情况下拿走了你的数据库。您的磁盘大小有限，在超出磁盘大小之前，您只有几个小时的时间来解决这个问题。

现在，您已经理解了问题陈述及其重要性。因此，最好在为时已晚之前解决这个问题。

Postgres 服务器上占用你空间的恶意特性叫做 TOAST。相反，它是一个好心人，试图提高您的数据库的性能，但如果您的数据库过于频繁地删除/更新 toast 条目，可能会带来麻烦。

# 烤

[TOAST](https://www.postgresql.org/docs/current/static/storage-toast.html) 是 PostgreSQL 用来防止物理数据行超过数据块大小(通常为 8KB)的一种机制。Postgres 不支持跨越块边界的物理行，因此块大小是行大小的硬性上限。为了允许用户表具有比这更宽的行，TOAST 机制将宽字段值分解成更小的片段，这些片段存储在与用户表相关联的 TOAST 表中。

您创建的每个表都有自己的关联(唯一的)TOAST 表，根据您插入的行的大小，这些表最终可能会被使用，也可能不会被使用。所有这些对用户来说都是透明的，默认情况下是启用的。

当要存储的行“太宽”时(默认情况下阈值为 2KB)，TOAST 机制首先尝试压缩任何宽字段值。如果这还不足以获得 2KB 以下的行，它会将宽字段值分成块，存储在相关的 TOAST 表中。每个原始字段值都被一个小指针代替，该指针显示在 TOAST 表中何处可以找到这些“不符合规定”的数据。TOAST 将尝试以这种方式将用户表行压缩到 2KB，但是只要它能低于 8KB，就足够好了，并且该行可以被成功存储。

# 是什么让您的数据库遭受损失

数据库发出过多的更新或删除请求是问题的根本原因。如果您在大尺寸的行上进行这些操作，它就像催化剂一样使事情变得更糟。

在 PostgreSQL 中，每当删除表中的行时，现有的行或元组被标记为死的(不会被物理删除)，并且在更新期间，它将相应的现有元组标记为死的，并插入新的元组。所以更新操作=删除+插入。这些死元组消耗了不必要的存储，最终导致 PostgreSQL 数据库膨胀。这种存储可以通过[真空](https://www.postgresql.org/docs/current/sql-vacuum.html)回收。

VACUUM 回收的存储空间永远不会归还给驻留的操作系统，而是在同一个数据库页面中进行碎片整理，以便将来数据插入到同一个表中时重新使用。要完全回收底层操作系统的空间，需要执行真空满操作，这比较慢，并且需要对表进行独占锁定，不允许任何其他读/写操作。

膨胀严重影响了 PostgreSQL 的查询性能，在 PostgreSQL 中，表和索引是作为一个固定大小的页面数组存储的(通常大小为 8KB)。每当查询请求行时，PostgreSQL 实例将这些页面加载到内存中，死行会在数据加载期间导致昂贵的磁盘 I/O。所以非常需要定期运行真空。

为此， [AUTOVACUUM](https://www.postgresql.org/docs/13/runtime-config-autovacuum.html) 守护进程是一个可选特性，它自动清空数据库，这样您就不必手动运行 [VACUUM 语句](https://www.postgresql.org/docs/11/sql-vacuum.html)。默认配置中启用了自动真空后台程序。守护程序由多个进程组成，这些进程通过从数据库中删除过时的数据或元组来回收存储。它检查具有大量插入、更新或删除记录的表，并根据配置设置清空这些表。

# 自动吸尘的任务

有各种自动真空配置参数，这使得调整变得复杂。主要原因是 autovacuum 有许多不同的任务。

*   清除更新或删除操作后留下的“死元组”
*   更新跟踪表块中空闲空间的*空闲空间映射*
*   更新*仅索引扫描*所需的*可见性图*
*   “冻结”表行，以便事务 ID 计数器可以安全地回绕
*   安排定期分析运行，以保持表统计信息的更新

这些是自动真空的所有配置参数

```
#------------------------------------------------------------------------------
# AUTOVACUUM PARAMETERS
#------------------------------------------------------------------------------autovacuum = on                        # Enable autovacuum subprocess?  'on' requires track_counts to also be on.log_autovacuum_min_duration = -1       # -1 disables, 0 logs all actions and their durations, > 0 logs only actions running at least this number of milliseconds.autovacuum_max_workers = 3             # max number of autovacuum subprocesses (change requires restart)autovacuum_naptime = 1min              # time between autovacuum runsautovacuum_vacuum_threshold = 50       # min number of row updates before vacuumautovacuum_analyze_threshold = 50      # min number of row updates before analyzeautovacuum_vacuum_scale_factor = 0.2   # fraction of table size before vacuumautovacuum_analyze_scale_factor = 0.1  # fraction of table size before analyzeautovacuum_freeze_max_age = 200000000  # maximum XID age before forced vacuum(change requires restart)autovacuum_multixact_freeze_max_age = 400000000   # maximum multixact age before forced vacuum(change requires restart)autovacuum_vacuum_cost_delay = 20ms    # default vacuum cost delay for autovacuum, in milliseconds -1 means use vacuum_cost_delayautovacuum_vacuum_cost_limit = -1      # default vacuum cost limit for autovacuum, -1 means use vacuum_cost_limit
```

这些参数可以根据数据库级别或表级别的数据库个人需求进行调整。对于死元组清除，我们不需要重新配置所有的自动真空参数。

# 为死元组清理调整自动真空

长时间运行或空闲的事务会导致 VACUUM 等待持有表上的共享更新独占锁。除非此类查询得到解决，否则调整 AUTOVACUUM 将毫无用处。

如果您无法从根本上解决问题，您可以使用配置参数**idle _ in _ transaction _ session _ time out**让 PostgreSQL 终止“在事务中空闲”时间过长的会话。这在客户端会导致错误，但是如果您没有其他方法来保持数据库运行，这可能是合理的。类似地，为了对抗长时间运行的查询，可以使用**语句 _ 超时**。

# 调整自动真空运行速度

如果 autovacuum 跟不上清理死元组的速度，解决方案就是让它工作得更快*。*

*记住，更快并不意味着更频繁地运行 vacuum 或更频繁地运行它。VACUUM 是一项资源密集型操作，因此默认情况下，autovacuum 会故意缓慢运行。目标是让它在后台工作，而不影响正常的数据库操作。但是，如果您的应用程序创建了大量的死元组，您必须使它更积极。*

***auto vacuum _ vacuum _ threshold**=真空启动的最小死元组数(默认值= 50)*

***auto vacuum _ vacuum _ scale _ factor**=定义表 w.r.t 与其行数的最小死元组数的比例因子(默认值= 0.2)*

*这两个参数有助于在关系表上控制真空的触发。由于真空是一个消耗资源的过程，因此只应在必要时运行。利用这些参数，计算表级最小死元组计数，其充当真空触发的阈值。*

*autovacuum_vacuum_threshold 确保 vacuum 不会对小表过于频繁地运行。*

*autovacuum_vacuum_scale_factor 确保为大型桌子及时运行真空。*

```
*Min Number of of dead tuples required = autovacuum_vacuum_threshold  +  No. of Rows in table * autovacuum_vacuum_scale_factor*
```

***autovacuum _ vacuum _ cost _ limit**:auto vacuum 可以达到的总成本限制(由所有 auto vacuum 作业合并)。(默认为真空成本限制= 200)*

***autovacuum _ vacuum _ cost _ delay**:当达到 autovacuum_vacuum_cost_limit 成本的清理完成时，auto vacuum 将休眠这么多毫秒。(默认值= 20 毫秒)*

***auto vacuum _ max _ workers:**auto vacuum 为执行任务而产生的最大工作线程数。*

*这三个参数负责定义真空的清除速度和时间。Autovacuum 守护进程休眠**auto vacuum _ vacuum _ cost _ delay**作为一次完成的每一个**auto vacuum _ vacuum _ cost _ limit**工作的节流。*

*autovacuum 可以启动多达**个 autovacuum_max_workers** 进程，这些进程实际执行不同数据库/表的清理。这很有用，因为在单个大表清理完成之前，您不想停止清理小表(由于节流，这可能需要相当长的时间)。请记住，增加工作进程会使自动清空变慢，因为所有自动清空工作进程会共享成本值。每个工人进程只得到总成本限制的 **1/autovacuum_max_workers** ，所以增加工人数量只会让他们走得更慢。*

*默认值设置是非常基本的，因为 autovaccum 在默认情况下是启用的，所以要记住低资源系统。*

# *最后一个音符*

*对于不同的系统要求，真空配置可以不同。最好首先了解您的应用程序数据库需求，它会发出多少数据库请求，然后做出相应的决定。*

*还有，不要直接得出真空不足就是原因的结论。根本原因可能是其他原因导致 vacuum 等待，比如另一个数据库查询持有所需表/行的锁。所以最好先排除这种情况。*