# Python 和 Openpyxl:将数千个 Excel 工作簿收集到一个文件中

> 原文：<https://medium.com/analytics-vidhya/python-and-openpyxl-gather-thousands-of-excel-workbooks-into-a-single-file-eff4e8c9b514?source=collection_archive---------2----------------------->

![](img/0d46ea614e679c951310e8cc73d882bb.png)

在 [Unsplash](https://unsplash.com/s/photos/spreadsheet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Carlos Muza](https://unsplash.com/@kmuza?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

链接到这篇文章[葡萄牙语翻译在这里](https://fabriciusbr.medium.com/python-e-openpyxl-re%C3%BAna-milhares-de-pastas-de-excel-em-um-%C3%BAnico-arquivo-18ee688e3a70)。

介绍

Microsoft Excel 已经成为全球最受欢迎的程序之一，在就业市场和私人用途中广泛使用。也许在您的公司或家中，您仍然需要使用该扩展名来操作文件。xlsx，并且出于某种原因您希望您的数据保持以该格式保存。

Excel 有 VBA 作为原生资源，这样的编程语言可以在 xlsx 文件中执行大量的过程自动化需求。然而，微软几年前就停止了开发这种语言，尽管它现在仍然可以在 Excel 桌面版本中使用。

无论如何，你不需要知道 VBA 成为一个电子表格向导！使用 Python 和 openpyxl 库，可以编写节省大量时间的代码，并为您的工作带来具体的结果。所有这一切都将伴随着使用世界上最现代的编程语言之一的优势，伴随着市场上最高的增长率之一，伴随着一个巨大的、活跃的开发人员社区。

使用场景和示例数据

本文讨论了 Excel 中一个非常普遍的数据操作场景，当一个数据库被不同的电子表格或几个 Excel 文件分散时，就会出现这种情况。如果只有很少的电子表格或工作簿，并且没有用户熟悉编程，那么复制粘贴的方法仍然可以将所有数据放在一个地方。这一程序绝不推荐，因为存在相当大的人为错误风险。此外，这将是对时间的巨大浪费。

如果您正在处理来自同一个数据库的信息，但是这些数据分散在数百个、甚至数千个不同的电子表格和文件中，该怎么办？在这种情况下，手动方法变得不可行。让 openpyxl 库帮助我们在使用最现代的。xlsx 扩展名。对于更老的 xls 文件，必须使用其他 Python 库。

[在这个 Github 资源库](https://github.com/fabricius1/gather-excel-data)中，您会发现两个目录(workbooks_by_days 和 workbooks_by_months)，其中包含 xlsx 文件，我们可以用它们来展示 openpyxl 从数千个 Excel 文件中收集数据的强大功能。

我随机创建了这个数据。因此，它们不对应于任何应该分析或理解的真实数据。所有文件都有相同的结构:在 workbooks_by_days 目录中，可以找到 2020 年 1 月 1 日到 2021 年 3 月 28 日之间每天的 Excel 工作簿。文件名包含年-月-日序列中的日期信息。这里每个文件只有一个电子表格，用每小时收集的数据模拟天气预报。在 workbooks_by_months 文件夹中，数据按月分组，因此每个 Excel 工作簿都有几十个电子表格，每个表格代表该月的一天。

因此，如果您想按照下面的代码解释，同时在本地测试它，请将这些 Excel 文件下载到您的机器上。

代码表示

我们将从导入代码中需要的 openpyxl 资源以及 os 模块开始:

现在，我们将复制我们想要收集的 Excel 文件所在的目录路径，然后对该目录中的所有项目使用 for 循环。如果代码找到以。如果它不是临时文件的名称(通常带有~ $前缀)，我们将调用`load_workbook`函数`并`创建一个 openpyxl Workbook 对象的实例。这个工作簿对象将被追加到名为`wbs`的列表中，该列表将所有要合并的文件的数据保存在内存中。

当 for 循环结束时，我们将使用 Python 代码创建一个新的 Excel 工作簿。该工作簿将由保存在`wbs`列表中的工作簿数据填充。在创建这个合并工作簿(现在保存在`final_wb`变量中)后，我们将访问它的第一个电子表格，并将这个工作表类型的对象保存在`final_ws`变量中。我们还将打开`wbs`列表中的第一个工作簿，将其数据保存在变量`wb1`和`ws1`中:

openpyxl 中的每个工作表对象都有两个对我们有用的属性:`max_column`和`max_row`。它们保存电子表格中最大行数和列数的信息。这些属性值将在我们的代码中广泛使用，因为当我们遍历给定电子表格的行和列时，它们将定义最大限制。

下面的代码有一个 for 循环，每次迭代只执行一行代码。创建它是为了复制列标题并将其传输到`final_ws`:

现在是代码的核心部分。因为有四个嵌套的 for 循环，我们可能会被所有不同的缩进级别搞糊涂，所以我们将首先展示代码，然后对其进行一些评论。

在第一个 for 循环中，我们遍历保存在`wbs`列表中的所有工作簿对象。对于每个`wb`(工作簿的常见缩写)，我们将遍历它的所有工作表对象(`wb.worksheets`返回这些对象的列表)。最后，我们将遍历所有的工作表行和列，检索每个单元格的信息并将其插入到`final_ws`中的正确位置。`current_row`变量将控制`final_ws`工作表中的行，该行将接收当前正在复制的信息。

我们代码的最后一步是保存`final_wb`工作簿，方法是在我们的机器上创建一个包含编译数据的. xlsx 文件。

将我们的代码整合到一个函数中

我们可以将目前给出的代码封装在一个函数中，该函数将包含要编译的 xlsx 文件的目录(`workbooks_path`)和新 Excel 文件的名称以及最终收集的数据(`final_filename`)作为参数。下面显示了这样一个函数。请注意，在函数体开头包含了五个条件结构。它们检查函数参数的类型和格式。

我相信这个函数在范围上足够通用，可能会帮助其他需要解决类似问题的人。

非常感谢您阅读我的文章。

*快乐编码*！

附注:你会在 [LinkedIn、Medium 和 Github](https://linktr.ee/fabriciobarbacena) 上找到更多关于我工作的信息。

其他资源:

[https://pypi.org/project/openpyxl/](https://pypi.org/project/openpyxl/)

[https://github.com/fabricius1/gather-excel-data](https://github.com/fabricius1/gather-excel-data)

[https://gist . github . com/fabric ius 1/F2 e 45 f 8 ed 13d 618 EC 70638018 fc 3 Fe 86](https://gist.github.com/fabricius1/f2e45f8ed13d618ec70638018fc3fe86)