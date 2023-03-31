# 如何将水印附加到您的 pdf 文件(多页)

> 原文：<https://medium.com/analytics-vidhya/how-to-attach-watermark-to-your-pdf-file-with-multiple-pages-90089fa2ad15?source=collection_archive---------3----------------------->

我第一次接触 pdf 和 python 时，我父亲问我是否有一种简单的方法来选择 pdf 中的表格，并将其放入 excel 中，目的是使数据操作更容易，尽管这是一个在 excel 中执行“ctrl-c”和“ctrl-v”的简单任务，我发现使用 python 和 pdf 的可能性很有趣，但没有开始任何项目。

随着时间的推移，作为我大学金融俱乐部的成员，一些有趣的事情浮出水面，我们每个学期讲授的教科书都有问题，任务是在每份讲义上添加一个带有该人姓名的自定义水印。

为了让其他有同样问题的人更容易理解，也为了巩固我的研究，我做了这个快速指南，解释如何从零开始构建。

我确实在这个网站上发现了许多有趣的内容:

[Python 教程——真正的 Python](https://realpython.com/)

## 开始前的重要步骤

*   **Jupyter 实验室**:如果你正在使用 Jupyter 笔记本，把你的文件放在和笔记本相同的目录下是很重要的。
*   **Google Colab** :如果你正在使用 Google Colab，重要的是首先从这段代码开始导入 excel(。xlsx)，以及课程用书(。pdf)文件。

```
from google.colab import files
uploaded = files.upload()
```

## 在 python 中使用 pdf

有一些库可以处理 pdf 文件，但是因为我们的目标是处理一本教科书，我们的水印的目标是出现在这本教科书的每一页，同时这本书必须是可读的(在我的搜索中，我发现了许多向 pdf 文件添加水印的方法，但是它们实际上覆盖了教科书中所写的内容)，所以我决定处理这些库:

```
from fpdf import FPDF
import PyPDF2
from PyPDF2 import PdfFileMerger
from PyPDF2 import PdfFileWriter, PdfFileReader
from reportlab.pdfgen import canvas
from reportlab.lib.units import cm
```

如果你正在使用 google colab 或 jupyter notebook，你可能必须安装德 FPDF、PyPDF2 和 reportlab 库，如果你必须这样做，在 **jupyter lab** 中，你只需在你的命令提示符中写下以下内容:

```
pip install FPDF
pip install PyPDF2
pip install reportlab
```

如果你使用的是 **google colab** ，只需在笔记本上的一行代码中这样做:

```
!pip install FPDF
!pip install PyPDF2
!pip install reportlab
```

是的，区别只是“！”在开始，和位置。

## 开始创造我们的水印

技巧的事情是，你必须在开始你的主要目标之前创建“n”(n 是你必须做的不同自定义水印的数量)个 pdf 水印文件，因为每一个都是不同的，稍后会与你的课程书合并。

第一步是创建一个 excel 文件，是的，我将使用 excel 和 python，因为我发现它更容易，但你也可以只使用 python。

当您打开 excel 时，您可以在第一个工作表(Sheet1)中写入要在水印中写入的内容，对于我的项目，我写了:“名字，姓氏，身份证号”，并且我将使用第二个工作表(Sheet2)来写入我们未来的 pdf 水印文件的名称，因此我写了:“名字，姓氏，。pdf”，这真的很重要。pdf，因为这是您的计算机识别 pdf 文件的方式，这样做是为了以后单独调用这些文件。

代码如下:

*   创建水印功能

```
def watermark(file,content):
   page= canvas.Canvas(arquivo, pagesize = (21*cm, 29.7*cm)
   page.translate(14.40*cm, 14.85*cm)
   page.setFont("Helvetica", 20)
   page.rotate(45)
   page.setFillColorRGB(0.55,0.55,0.55)
   page.setFillAlpha(0.3)
   page.drawString(-10*cm, 3*cm, content)
   page.save()
```

逐步解释我们在做什么:

1.  创建我们的文件，用 A4 纸大小，
2.  把我们的原点坐标移到纸的中心，
3.  选择我们的字体风格和大小，
4.  旋转到 45 度，我选择在这个方向写水印，
5.  选择我的水印颜色，
6.  “SetFillAlpha”设置透明度，选择一个不影响你的课本读者视图的透明度非常重要，
7.  设置水印文本开始位置，
8.  最后，我们保存并关闭我们的 pdf。

## 访问我们的 excel 文件，最后做所有水印

为此，我们将使用熊猫图书馆，它已经安装在 jupyter 和谷歌 colab，所以你只需要导入

```
import pandas as pd
```

这将是创建“n”水印文件的代码。

```
data_names = pd.read_excel('EXCEL_FILE_NAME.xlsx',
sheet_name='Sheet1')
names = data_names.values.tolist()for i in range(len(nomes)):
   names[i][0] = names[i][0].replace(u'\xa0', ' ')
   #this line is just because this xa0 was bothering medata_file = pd.read_excel('EXCEL_FILE_NAME.xlsx', sheet_name='Sheet2')
file = data_file.values.tolist()for i in range(len(names)):
    watermark(file[i][0], names[i][0])
```

## 将我们的水印添加到我们的教科书中

这是我第一眼看到的挑战，我们有一本超过 100 页的书，单独添加水印会非常累，所以我试图找到可以让我们作为一个群体的生活更容易的东西，我们任务的主要代码将是以下函数:

```
def add_watermark(cbook,output,watermark):
   watermark_obj = PdfFileReader(watermark)
   watermark_page = watermark_obj.getPage(0)
   pdf_reader = PdfFileReader(cbook)
   pdf_writer = PdfFileWriter
   for page in range(pdf_reader.getNumPages()):
      page = pdf_reader.getPage(page)
      page.mergePage(watermark_page)
      pdf_writer.addPage(page)
   with open(output, 'wb') as out:
      pdf_writer.write(out)
```

让我解释一下我们在这里做什么，首先我们用 PdfFileReader 初始化 pdf(这个操作需要一些时间)，然后我们得到这个水印的第 0 页，因为它是一个单页文件，我们说的是存在的单页，其次我们用 PdfFileReader 初始化或者教程阅读(同样需要一些时间)，接着我们支持用 PdfFileWriter 写，这个步骤将在我们的循环中使用。

现在我们进入我们的循环，我们需要迭代我们的教科书中的每一页，第一步是使用函数“getPage(page)”，这是迭代我们的书的所有页面所必需的，第二步我们将合并页面，用“mergePage”(是的，给文件添加水印只不过是合并两个文件，一个带有我们的水印，一个带有教科书的页面)，然后我们将添加页面，因为这种合并实际上是创建一个新的 pdf 文件。然后，我们将使用函数“with open”来打开文件，这很重要，“wb”意味着“写入和二进制”，这只是模式(这意味着我们告诉解释器文件将被使用的方式)。最后，我们用新文档中的“write()”来写 pdf。

## 钓鱼我们的代码

现在我们将制作包含所有水印的课程。

```
data_final_file = pd.read_excel('EXCEL_FILE_NAME.xlsx', sheet_name = 'Sheet3')
books = data_final_file.values.tolist()for i in range(len(names)):
   cbook = "NAME OF YOUR COURSE BOOK .pdf"
   output = books[i][0]
   watermark = file[i][0]
   add_watermark(cbook, output, watermark)
```

解释我们在这里所做的事情，首先我们在 excel 的 sheet3 中为属于我们学生的每门课程添加我们想要的名称，因此举一个例子，它可以是:“Course book of investments-Eduardo”，重要的是名称不同以便于访问，而不是我们使用我们的循环来迭代我们文件中存在的所有名称，并为我们的学生生成带有水印的每本书。

## 毕竟，你的 pdf 应该是这样的

![](img/62f728d88a51ed8fb5c23243ac515a48.png)

我创建这个文档只是作为一个示例，

如果你想你的水印更强，只需改变设置。

**观察:**当我运行这段代码时，我正在处理一本 120 页的教科书，以及 120 个不同的水印，所以计算我正在处理的 14400 个合并，需要 22 分钟才能得出结论。

感谢你的阅读，如果你有更快的方法，请在下面评论。