# 不使用 Avro 模式注册表解析融合 Kafka 消息

> 原文：<https://medium.com/analytics-vidhya/parse-confluent-kafka-messages-without-avro-schema-registry-b1b24013af7a?source=collection_archive---------10----------------------->

![](img/3670d9f6207cc324225730b94a5b9a99.png)

# 解析二进制序列化程序

# 用例

*   融合的卡夫卡信息被推送到事件中心
*   使用镜像制作工具移动消息
*   合流使用的二进制 Avro 格式进行序列化
*   模式注册 id 也内置在消息有效负载中

# 要求

*   Azure 帐户
*   创建事件中心命名空间标准版
*   创建一个活动中心
*   在源 Kafka 中安装镜像制作工具 2
*   从事件中心获取 SAS 密钥
*   配置镜像生成器，使用 SAS 密钥向事件中心发送消息
*   选择要发送的主题
*   在事件中心中，每个主题将创建一个与 Kafka 相同主题名称的事件中心
*   创建 Azure 数据块
*   使用运行时 7.6 创建集群
*   创建 pyspark 笔记本

# 代码部分

*   使用 Azure 数据块
*   使用 python spark 解析代码
*   装载进口货物
*   创建一个函数来分隔融合位，如模式 id 和值
*   前 4 个字节作为模式 id。
*   移除这 6 个字节会使处理变得更容易，因为剩余的都是 Avro
*   1 个字节是汇合的幻字节
*   2–5 个字节是模式 id 值

```
%python
from pyspark.sql.types import StringType
from pyspark.sql.functions import udf
import pyspark.sql.functions as fn

binary_to_string = fn.udf(lambda x: str(int.from_bytes(x, byteorder='big')), StringType())%python

df1 = spark.read.format("avro").option("mode", "PERMISSIVE").option("header", "true").load("dbfs:/FileStore/shared_uploads/xxxx@xxxxx.com/part_00000_ae91d923_dcca_4690_xxxx_xxxxxxxxxx_c000.avro")%python

display(df1)
```

*   拆分子字符串
*   拉模式 ID

```
%python
df2 = df1 \
  .withColumn('fixedValue', fn.expr("substring(value, 6, length(value)-5)")) \
  .withColumn('valueSchemaId', binary_to_string(fn.expr("substring(value, 2, 4)")))
```

*   现在加载 Avro 模式文件
*   从融合 Kafka 注册表导出模式信息

```
%python
from pyspark.sql.avro.functions import from_avro, to_avro

jsonFormatSchema = open("/dbfs/FileStore/shared_uploads/xxxx@xxxxxxx.com/avro_####_schemaname.avsc", "r").read()%python
import pyspark.sql.avro.functions
from pyspark.sql.avro.functions import from_avro, to_avro

# display(df2.select(from_avro('fixedValue, schema.toString()) as 'record))
output = df2\
  .select(from_avro("fixedValue", jsonFormatSchema).alias("record")%python
display(output.select("record"))%python
df3 = output.select("record.*")
display(output.select("record.*"))%python
output.select("record.*").count()
```

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Accenture/blob/master/Ingestion/confluentkafka.md)*。*