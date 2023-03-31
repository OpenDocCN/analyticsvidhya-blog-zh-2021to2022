# Python 中的 JSON

> 原文：<https://medium.com/analytics-vidhya/json-in-python-163857b00415?source=collection_archive---------17----------------------->

Python json 模块演练！！

![](img/c1e8e2780ac5aef6e172d237897a0553.png)

简·安东宁·科拉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**简介**

最近开始用美汤和请求库学习网页抓取。Web 抓取是从网页中提取大量数据的过程，这些数据可用于以后的进一步分析。用于抓取的程序被称为 scraper 或 crawler 或 bot。由于大多数现代网站是动态的，它们的结构定期变化，因此在抓取时出错的可能性很高，即今天用于抓取的代码明天可能有用，也可能无用。因此，在从网页中提取数据后，正确存储数据是非常重要的。

提取后的数据可以按照用户的要求以多种格式存储。在众多可用的数据和文件格式中，JSON 是最通用、最灵活的格式。

**JSON 是什么？**

JSON 代表 JavaScript 对象符号。它是一种轻量级的数据交换格式。对人类来说，读和写是容易的。它可以轻松处理多维数据和半结构化数据。添加数据和删除数据可以很容易地完成。许多数据库和语言都提供了导入和导出 JSON 的库。文件扩展名是**。json** 。

*这里我们就来说说 Python 的 JSON 模块！！！*

JSON 格式和 python 的字典很像。那我们为什么还要学习 JSON 呢？

JSON 用于在服务器和 web 应用程序之间传输数据。web 服务和 API 使用 JSON 格式来提供公共数据。由于网络已经成为任何程序员不可或缺的一部分，所以理解这种格式以便很好地掌握数据是很重要的。

**JSON 中的示例**

下面的例子展示了如何使用 JSON 根据书籍的主题和版本来存储与书籍相关的信息。

```
{
   "book": [

      {
         "id":"01",
         "language": "Java",
         "edition": "third",
         "author": "Herbert Schildt"
      },

      {
         "id":"07",
         "language": "C++",
         "edition": "second",
         "author": "E.Balagurusamy"
      }
   ]
}
```

JSON 中的数据存储在花括号内的 *"key" : "value"* 对中，如示例所示。键和值都必须用双引号括起来。

**JSON 支持的数据类型**

*   用线串
*   数字
*   布尔运算
*   空
*   数组
*   目标

**Python 中的 JSON**

Python 提供了名为 *json、*的内置包来处理 json 数据。

**导入 json 模块**

```
import json
```

**将 JSON 解析为 python**

如果有一个 JSON 字符串，可以使用 *json.loads()* 方法将其转换成 python，该方法将返回一个 python 字典。

例子

```
#importing 
import json#json string
x = '{"name":"Varun","age":21 }'#converting json to python
y = json.loads(x)#the result is python dictionary
print(y['name'])=> Varun
```

**将 Python 转换成 JSON**

我们可以通过使用返回 json 字符串的 *json.dumps()* 方法将 Python 数据对象转换成 JSON。

例子

```
#importing
import json#python dictionary
x = {"name":"varun","age":21}#converting to json
y = json.dumps(x)#the result is a json string
print(x)=>'{"name":"varun","age":21 }'
```

**感谢您的阅读！！！**