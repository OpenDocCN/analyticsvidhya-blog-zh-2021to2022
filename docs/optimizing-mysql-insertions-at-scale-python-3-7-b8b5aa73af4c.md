# 大规模优化 MySQL 插入(Python 3.7)

> 原文：<https://medium.com/analytics-vidhya/optimizing-mysql-insertions-at-scale-python-3-7-b8b5aa73af4c?source=collection_archive---------15----------------------->

![](img/731966366f745c49b95601764aae72a8.png)

在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[尹新荣](https://unsplash.com/@insungyoon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

当我们谈到规模(比如在一个表中插入 250 万行)时，MySQL 事务可能非常昂贵。虽然*可以传输到****MySQL****8.0 服务器或客户端的最大可能* ***数据包*** *数据包是* [*1GB*](https://dev.mysql.com/doc/refman/8.0/en/packet-too-large.html#:~:text=The%20largest%20possible%20packet%20that,error%20and%20closes%20the%20connection.) ，但这可能不是一个高效的事务。正如我的数据结构和算法教授所说:

> 作为工程师/计算机科学家，我们不仅对找出问题的可能解决方案感兴趣(就像数学家一样)，而且对开发问题的有效和优化的解决方案感兴趣。

在这篇博客中，我们将学习如何高效地优化昂贵的 MySQL 事务。在开始之前，我想让你读一下我以前的博客— [使用本地的 AWS Lambda 环境变量。env 文件](/analytics-vidhya/using-aws-lambda-environment-variables-from-local-env-file-python-3-7-f3edacdea015) *。*

假设我们有一个**文本文件**，其中包含了**250 万个**世界各地随机出现的人的姓名和联系电话*(为了简单起见，我们在整个示例中只考虑一个实体，即****【name】****)*，我们希望将它传输到一个 MySQL 表中—**random . student**(random 是数据库名称)。我们可以通过三种可能的方式将它们插入到表中:

*   **一次插入一个** —就时间复杂性而言，该事务成本太高，但仍然可行。
*   **一次插入全部内容** —这是不可行的，因为为如此繁重的事务构建插入查询代价太高。
*   **插入小数据包** —这在时间复杂度和执行方面都是高效可行的。

让我们首先创建一个 python 文件并编写一个函数来**连接**到我们的远程或本地 MySQL 实例*(我将用一个远程 AWS RDS 实例*来展示这个例子)。我将我的 DB 凭证存储在一个单独的配置文件中，我将使用 python `dotenv`模块获取该文件。您可以做同样的事情或硬编码您的凭据；第一个是一个很好的实践。

```
import pymysql
from dotenv import load_dotenv
import os# loading environment variables
laod_dotenv()hostName  = os.environ.get('RDS_HOST')
name = os.environ.get('NAME')
password = os.environ.get('PASSWORD')
port = os.environ.get('PORT')def connectDB():
    try:
        conn = pymysql.connect(host=hostName, port=int(port),
        user=name, passwd=password)
        print("SUCCESS: Connection to RDS MySQL instance succeeded")
        return conn except Exception as e:
        print("ERROR: Unexpected error: Could not connect to MySQL
        instance.")
        raise Exception(e)
```

现在，我们必须从文本文件中读取学生姓名，并将它们以小包的形式插入到表中，即一次插入 250 行。我们将首先从文本文件中读取名称，并将它们存储在一个列表中，稍后我们将在小的*批*中迭代该列表，以创建插入查询。

```
names = []with open('studentFile.txt') as f:
    lines = f.readlines()
    for data in lines:
        name = data[:-1]
        # print(name)
        names.append(name)
```

一旦我们有了名字列表，我们将创建两个函数— `insertNames()`和`buildQuery(range)`。`insertNames()`函数**根据我们的`BATCH_SIZE`计算**插入所有名字所需的总迭代次数，并创建一个 for 循环来执行相同的操作。在循环中，它在每次迭代中调用`buildQuery(range)`来创建`INSERT`查询的`VALUES`,方法是将每次使用的**名称范围**作为函数的参数。

```
def insertNames():
    BATCH_SIZE = 250
    iterationNumber = round(len(names)/BATCH_SIZE)+1 conn = connectDB()
    cursor = conn.cursor() print(f"Total iterations to make: {iterationNumber}")
    insertQuery = ""
    for i in range(0, iterationNumber):
        try:
            namesToUse = names[i*BATCH_SIZE
            :(i+1)*BATCH_SIZE]
            insertQuery = buildInsertQuery(namesToUse) # run sql query
            query = (
                f"INSERT INTO random.student(name)
                VALUES{insertQuery}"
            )
            cursor.execute(query)
            conn.commit()
            print(f"MySQL insertion {i} completed") except Exception as e:
            print(f"Exception while inserting row {i} 
            - ignore. Error: {e}")
```

`buildQuery()`返回`INSERT`查询的`VALUES`，它基本上由`insertNames()`用来插入 MySQL

```
def buildQuery(range):
    insertQuery = "" for name in range:
        insertQuery = insertQuery + (
            f"('{name}'), "
        )
    insertQuery = insertQuery[0:-2] return insertQuery
```

我们现在只需要调用`insertNames()`来执行插入，我们就完成了

# 总结一下:

1.  我们从文本文件中读取数据，并将其存储在一个列表中。
2.  我们开发了一个函数— `buildQuery()`来迭代列表，并为某个范围构建插入查询。
3.  我们计算了小批量传输全部数据所需的总迭代次数，并在每次迭代中调用我们的`buildQuery()`函数进行插入。

***附加提示*** *:您可以使用 Python 的* `*time*` *模块对代码的每一部分进行剖析，以检查代码的效率，并跟踪每一个进程。*

感谢您通读。我将非常感谢您的评论和建议。如果你有其他的优化技术，请在评论中告诉我。

如果你觉得它有用，请给我一个掌声，并与你的开发伙伴分享。 ***不要硬编码，硬编码！***

## **苏巴亚恩·戈什**

[LinkedIn](https://www.linkedin.com/in/realsubhayan/)|[Twitter](https://twitter.com/realsubhayan)|[insta gram](https://www.instagram.com/realsubhayan/)