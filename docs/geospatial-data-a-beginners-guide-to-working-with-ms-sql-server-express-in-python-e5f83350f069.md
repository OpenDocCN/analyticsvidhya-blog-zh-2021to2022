# 地理空间数据:在 Python 中使用 MS SQL Server Express 的初学者指南

> 原文：<https://medium.com/analytics-vidhya/geospatial-data-a-beginners-guide-to-working-with-ms-sql-server-express-in-python-e5f83350f069?source=collection_archive---------0----------------------->

最近，我被分配了一项任务，制作一张交互式地图来展示一家公司的国际销售情况，这需要使用地理空间数据。对于可视化，我选择了 Python 的散景库，而客户端数据在 **Microsoft SQL Server 2019 中。因此，我第一次遇到了将地理空间数据从 Python 读取到 Microsoft SQL Server 以及从 Python 读取回来的挑战。**事实证明这并不像看起来那么简单，所以在对堆栈溢出进行了大量的挖掘之后:)我决定将所有这些放在一个地方，以供任何可能遇到类似任务的人使用——所以我在这里写了我的第一篇技术文章！

本文将带您了解:

*   正在 Python geopandas 数据帧中加载 shapefile 坐标；清理地理数据的示例
*   通过 Python 连接 SQL Server Express 2019 的两种方式:SQLAlchemy 和 pyodbc
*   将地理数据框导入 SQL Server Express 2019
*   将 SQL Server 表作为地理数据框架或普通数据框架读回 Python

在最初接受这个任务后，我很快发现了大量关于使用 Python 的 sqlite、MySQL 和 postgreSQL 数据库的优秀资源，但对于 MS SQL Server、*而言，特别是在地理空间数据*的上下文中，就没有那么多了。这可能是因为 MS SQL Server 是一个付费的关系数据库管理系统，而其他的是开源和免费的。谢天谢地，客户端使用的是一个名为[**MS SQL Server Express**](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)**的**免费**缩减版 SQL Server。我用的是 2019 版。**

**这篇文章会很有用**:

*   **如果你以前没有 SQL-Python 连接的经验**
*   如果您/您的公司将您的(地理)数据存储在 MS SQL Server 中，但要求将其输入 Python，例如用于数据可视化目的
*   如果您必须将 MS SQL Server Express 连接到 Python 并读取通用 pandas 数据帧。

**本文假设:**

*   已经安装并设置了 MS SQL Server (Express)和 Python
*   Python 的一些基础知识:模块导入，文件导入，熊猫数据探索，清理和操纵，定义函数和函数参数
*   您已经知道为什么需要 SQL Server，您计划如何处理 SQL Server 中的数据，以及如何找到您的 SQL Server 连接属性(连接驱动程序、数据库名称等)

**它不包括 SQL 或 t-SQL 命令或使用 SQL Server Management Studio 中的表——它主要处理 Python 和 SQL 之间的数据连接和交互。**

# 将坐标输入 Python

## Shapefile

shapefile(。shp)是一种地理空间数据格式，它使用多边形和多重多边形等矢量要素来表示地图上的国家。在我的情况下，我需要世界地图的坐标，不需要非常详细。为此，我从[这里](https://www.naturalearthdata.com/downloads/110m-cultural-vectors/)(顶部绿色按钮)下载了 shapefile。

下载并解压后，将所有文件复制到脚本工作目录下的一个文件夹中，我的文件夹名为‘100m _ coords’。

## 导入模块

导入我们将在此会话中需要的包。

```
import pandas as pd
import geopandas as gpd
import pyodbc
import sqlalchemy as sal
from shapely import wkb
```

如果您没有安装任何一个，您可以在您的虚拟环境中使用*pip install[包名]* 安装该模块，例如 pip install pyodbc。

## 读取并清理坐标

[**geo pandas**](https://geopandas.org/)**是 Python 非常方便的处理地理空间数据的库。然而，似乎从 MS SQL Server 加载地理数据并不简单，因为 geopandas 方法是为 postgreSQL 数据库编写的(稍后将详细介绍)。**

**的。读入 geopandas 的 shp 文件具有世界上所有国家的几何要素，每个国家都有 90 多个其他列-我们不需要所有这些列！请随意加载整个 shapefile 并浏览数据，然后决定您需要哪些数据。在我的例子中，我只对该国家的 ISO Alpha 2 代码感兴趣，因为这与我稍后将在 SQL Server 中对其进行分组的销售数据相匹配(本文未涉及)。**

```
# set the folder name + .shp  name that we want
shapefile = ‘110m_coords/ne_110m_admin_0_countries.shp’# read the entire shapefile first 
gdf = gpd.read_file(shapefile)# read only the country name (ADMIN), geometry coordinates (geomtry) and the desired country code (in my case ISO_A2)
gdf = gpd.read_file(shapefile)[['ADMIN', 'ISO_A2', 'geometry']]# rename columns 
gdf.columns = ['country','ISOA2_code','geometry']
```

**您现在可能需要进行一些探索，因为地理数据可能需要“清理”。就我而言，快速浏览一下就能发现一些 ISO 代码并不符合要求——例如挪威:**

**![](img/78e96a2803bec4381fb82d6ad024e388.png)**

**为了检查其余部分，我编写了一个快速函数来打印出所有带有整数(即假)iso 代码的国家:**

```
for country,code in (zip(gdf['country'], gdf['ISOA2_code'])): 
    try: 
        checkint = int(code)
        print(country, checkint)
    except ValueError: 
        pass
```

**我知道挪威、法国、北塞浦路斯和索马里兰有错误的 ISO Alpha 2 代码。实际上，索马里兰似乎没有自己的 ISO alpha 2 代码，尽管它的坐标占据了地图上的空间。删除该行将会破坏我的世界地图，因此为了我的数据可视化项目，我创建了一个独特的伪代码，它不会与任何其他国家代码冲突，而是添加了它(为了节省您的时间，我选择了 YY 作为伪代码)。为了用正确的代码替换错误的代码，我使用了[这个网站](https://www.iban.com/country-codes)和(地理)熊猫。锁定方法。我再举一个挪威的例子:**

```
# find index value for Norway 
gdf.loc[gdf['country'] == 'Norway']  # Norway index is 21# look up iso code online and replace 
gdf.loc[21, 'ISOA2_code'] = 'NO'
```

**最后，我按国家名称对这些值进行了排序:**

```
gdf = gdf.sort_values(by='country', ascending = True).reset_index()
```

# **将地理数据导入 SQL Server Express 2019**

## **连接到 MS SQL Server Express (SQLAlchemy)**

**为了将我们的 SQL 连接到 Python，我们使用了一个数据库连接 API，它只是我们像其他模块一样导入的一个模块。对于 MS SQL Server，这可以通过 pyodbc 驱动程序或使用 SQLAlchemy 直接完成。**

****Pyodbc** 是 MS SQL Server 的原生 Python 驱动程序——有了它，你可以直接与 SQL 交互。 **SQL Alchemy** 是一个对象关系映射(ORM)工具，用于为您创建处理所有交互的模型。还有 SQL 炼金术核心，更接近传统 SQL。对于像在 Python 和 SQL 之间加载表这样的简短任务，我只使用 pyodbc。然而，一个棘手的问题是 **geopandas 方法不支持通过 pyodbc 直接连接 MS SQL Server。**为了将我们的地理数据框读入 SQL Server，我们必须使用 **SQL Alchemy。****

**为了建立到数据库的连接，我们必须知道四件事情——我们的服务器名、数据库名(我们要添加表的地方)、认证方法和给定数据库的连接驱动程序。**

**当我下载并设置我的 SQL Server 时，我注意到我的**服务器名**是 IVETS\SQLEXPRESS，我对数据库使用了 Windows **身份验证(**即可信连接)和 Native Client 11 **连接驱动程序**。**

**![](img/4876ff15027cc1c5c5184c944c849461.png)**

**所有这些，以及**数据库名称**(在我的例子中是 *V05_* )，都反映在我的连接字符串(*const*)中，然后我用它连接到我的 SQLAlchemy 引擎。**

```
constr = 'mssql+pyodbc://IVETS\SQLEXPRESS/V05_?trusted_connection=yes&driver=SQL+Server+Native+Client+11.0'engine = sal.create_engine(constr)
conn = engine.connect()
```

**如果您有一个 **SQL 身份验证**设置，您还必须在 connectrion 字符串中输入您的用户名和密码，它现在采用以下通用格式(*注意:我自己还没有尝试过这个*)。**

```
constr = 'mssql+pyodbc://user:password@server/database'
```

**请记住，在这种情况下，您可能**而不是**想要将这些硬编码到您的 Python 脚本中，因为它会带来安全问题。接下来我展示了一个参数化连接函数的例子；这篇文章详细解释了连接 MS SQL Server 的不同方式，这取决于我们需要的安全级别以及我们计划如何访问连接。**

## **将地理空间数据导入 SQL Server 数据库**

**Pandas 有一个方便的 ***to_sql*** 方法，可以直接将任何数据帧写入 sql 数据库表。然而，读取地理数据有点棘手。**

**您应该还记得，我们有一个 geopandas 数据帧，其中有一个“geometry”列，这是一个**几何数据类型**。在我们将数据插入 SQL 之前，我们必须将几何列转换成一种**十六进制编码的** **WKB** (众所周知的二进制)数据格式。这是因为稍后我们将在 geopandas 中使用 *read_postgis* 方法，该方法只接受 WKB 格式(非 geometry 格式)的几何数据，还因为 SQL 中的‘geometry’数据类型会导致分组操作出现问题。**

```
# define a function to convert to wkb 
def wkb_hexer(line):
    return line.wkb_hex# apply to each row in the 'geometry' column
gdf['geometry'] = gdf['geometry'].apply(wkb_hexer)# finally, insert the dataframe into a table called 'geocoords' 
gdf.to_sql('geocoords', con = engine, schema = 'dbo', if_exists = 'replace')
```

**“Geocoords”是**的表名**。大多数新创建的数据库的**模式**是‘dbo’。请注意，我已经将***if _ exists*******= replace****作为参数，因为我只打算导入此数据一次，或者通过完全替换它来更正现有数据。如果以后打算添加更多坐标，可以做 *if_exists = append* 。****

****现在，如果我们打开 SQL Server Management Studio，刷新我们的数据库并选择表，我们应该会看到我们的新表 dbo.geocoords。如果右键单击并选择“选择前 100 行”，您会看到几何列不再是多边形要素，而是 WKB 格式。****

****![](img/3198ed1a69bef5d64f13da626cb6bb39.png)****

****gecoords 表****

****您还可以直接在 Python 中检查操作是否成功，方法是选择新表中的数据并只读取第一行:****

```
**query = ‘SELECT * FROM geocoords‘engine.execute(query).fetchone()**
```

# ******MS SQL Server Express 工作******

****现在，您可以在 SQL Server 中操作您的数据，这取决于您最初为什么需要 SQL 中的数据。由于这不是一篇关于 MS SQL Server 的文章，所以我不会详细讨论这个问题。在我的例子中，我必须在一个 [**视图**](https://docs.microsoft.com/en-us/sql/relational-databases/views/views?view=sql-server-ver15) 中将销售和地理坐标表组合在一起(类似于 pandas 中的*合并*方法)。很可能您的 geocoords 和您想要映射的数据之间存在一些差异，在这种情况下，您必须返回以进一步清理/修改您的 geocoords 数据，并再次在 SQL 中重新加载它(因此出现了上面的 *if_exist = Replace* 语句)。****

****![](img/2424a903dbe3f0e48b75c9fc72246404.png)****

****视角****

# ****将 SQL Server 表读回 Python****

## ****连接到 MS SQL Server Express (pyodbc)****

****在我们用 SQL 完成了我们需要的处理之后，让我们把数据读回 Python 来完成可视化任务！更具体地说，我将阅读我在上面创建的视图。****

****之前，我必须通过 SQLAlchemy 连接到 SQL，以便将地理数据框架读入 SQL。这一次我将只使用 **pyodbc** 驱动程序，这样我们可以看到一种不同的连接 SQL 数据库的方式。像 SQLAlchemy 一样，pyodbc 也要求我们构造一个**连接字符串。******

****在这里，我创建了一个相对灵活的函数，将(地理)数据从 SQL 读入 Python。我已经用**参数化了**服务器名、数据库名和连接驱动程序，所以即使将来有任何更新，我也可以轻松使用这个功能。另外，至关重要是，如果我有 SQL 身份验证，我可能不希望我的用户名和密码被任何拿到我的 Python 脚本(或 Jupyter 笔记本)的人免费获得。****

****如果您想读取熊猫的“普通”数据帧，可以通过传递 **geodata = False** 来实现。默认选项是我们正在读取地理数据框架，其中我们的坐标列称为“几何”。注意，这里的连接字符串语法不同于上面传递给 SQLAlchemy 引擎的语法。****

```
**# parametrised function that connects to SQL Server and reads data or geodata into pandas or geopandas, respectivelydef sql_data(table, server_name, database_name, driver, geodata = True): 
    constr= ("Driver={};"
                "Server={};"
                "Database={};"     
               "Trusted_Connection=yes;").format(driver,server_name, database_name)  
    conn = pyodbc.connect(constr)
    query = 'SELECT * FROM ' + table
    if geodata:
        data = gpd.read_postgis(query, conn, geom_col='geometry')
    else: 
        data = pd.read_sql(query, conn)
    conn.close()
    return data**
```

****该函数通过传递参数来应用，我在下面的字典中将这些参数设置为关键字参数。请注意连接驱动程序周围的花括号(Native Client 11.0)。****

```
**# keys list has the function argument names 
keys = ['server_name', 'database_name', 'driver']# values list holds the arguments for the SQL connection
values = ['IVETS\SQLEXPRESS', 'V05_', '{SQL Server Native Client 11.0}']# zip the keys and values into a dictionary 
serverargs = dict(zip(keys,values))# in case of e.g. database update, you can change it one time using: 
serverargs['database_name'] = 'V06'# call function we created above: 
data = sql_data('View_1', **serverargs)**
```

****最后一件事:空值通常是不需要的，例如由于与 json 不兼容，所以我用自己选择的字符串替换它们:****

```
**data['SumOrder'].fillna('No data', inplace = True)**
```

****如果我们查看我们的新地理数据框架-geo pandas '***read _ postgis***函数已将几何图形正确读取回几何图形格式，在此我们可以看到我们的面矢量要素。****

****![](img/2a91b774e5b601906428cd65e1883ec1.png)****

****地理数据框架视图****

****现在我终于有了我想要的 Python 格式的地理销售数据，我按照[这个](https://towardsdatascience.com/a-complete-guide-to-an-interactive-geographical-map-using-python-f4c5197e23e0)非常有用的教程创建了一个交互式散景地图，并且 tah-tah:****

****![](img/ac3aa97a5784e27c97992d2b3885c29d.png)****

****我希望这篇文章(不是那么短)能对任何遇到和我类似挑战的初学者有用:)****