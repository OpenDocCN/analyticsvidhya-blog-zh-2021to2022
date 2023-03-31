# 如何使用 Python 从任何 PDF 文件中轻松提取文本

> 原文：<https://medium.com/analytics-vidhya/how-to-easily-extract-text-from-any-pdf-with-python-fc6efd1dedbe?source=collection_archive---------0----------------------->

![](img/8499a8fc4927656d8cf0d988de5b0127.png)

## 比以往任何时候都容易

数据科学家经常不得不处理 PDF 中包含的信息，尽管他们中的一些人只是复制和粘贴他们需要的数据，这是一种可怕的做法，更不用说从长远来看这是最慢和最低效的工作方式，而且依赖于 PDF，甚至可能无法做到这一点。

在我们开始之前，感谢 Carlos Melo-sigmoid 允许我使用为他的数据科学课程创建的假 PDF 报告，我是该课程的学生，非常喜欢它。如果你不认识他，我强烈建议你在 Instagram、T2、博客和 YouTube 上关注他，这是我最喜欢的数据科学知识来源。

如果你想跟进这个项目，而不仅仅是 PDF 水管工的功能，请务必看看我的 [Google Colab 笔记本](https://colab.research.google.com/drive/1SObGEF2I2GUt07wmqDELpIdLBpmmHG6p#scrollTo=mj_3akc_PpUB)，我在里面讲述了我在这篇文章中谈到的所有内容，你也可以看到我提到的整个项目。

我们在本教程中使用的工具是 [PDF Plumber](https://github.com/jsvine/pdfplumber) ，一个开源的 python 包，它很棒，简单而强大。

[如果您想查看我在本例中使用的 PDF，请点击此处](https://drive.google.com/drive/folders/1kzpPatP5OqZJUAEJNCDf1dFwUP9515Rg)。

# 1.导入您的模块。

```
pip install pdfplumber -qimport pdfplumber
```

现在让我们来看看 PDF Plumber 的主要功能:

# 2.打开(“路径/到/目录”)

这个函数将打开你作为参数传递的文件，假设你有一个名为“pdf”的变量，它包含一个文件的目录:

```
pdf = pdfplumber.open('/content/file.pdf')
```

# 3.第[ ]页

打开文件后，您想要选择要提取信息的页面，假设您想要的信息在第一页，索引将为 0，因为 Python 从 0 开始计数:

```
page = pdf.pages[0]
```

想象你正在阅读一本书，第一步是打开这本书，然后你寻找你想要阅读的页面，然后你阅读它(即从中提取信息)，Python 以同样的方式工作。

# 4.提取文本()

现在您已经打开了一个页面，您需要从中提取文本:

```
text = page.extract_text()
```

如果您在 print()语句中调用变量 text，您将得到如下输出:

```
However, if you use the print function your text will be formatted like this:print(text)SIGMOIDAL 

Relatório Diário 

Data: 10/08/2020 

RECEITA: R$ 1.397,00 
DADOS ATUALIZADOS POR CARLOS MELO

 Visitantes: 1367 
A quantidade de visitantes diz respeito a visitantes únicos visitando qualquer 
página do domínio ou subdomínio sigmoidal.ai. Compreende, então, cursos, 
blogs e landing pages. 
 Inscritos: 33 
É considerado aqui o número de leads gerados por meio de cadastro 
voluntário nos formulários do cabeçalho, rodapé ou materiais ricos (como 
eBook, infográficos, entre outros). 
 Assinantes: 6 
Clientes assinantes da Escola de Data Science, considerando-se o plano 
renovável de assinatura mensal. 
```

print()函数将' \n '识别为换行符，将' \t '识别为制表符，因此文本被格式化。顺便说一下，这是我用来写这篇文章的摘录，你的输出会和我的不同。

然而，如果您只是调用变量，您的输出将是:

```
SIGMOIDAL \n \nRelatório Diário \n \nData: 10/08/2020 \n \nRECEITA: R$ 1.397,00 \nDADOS ATUALIZADOS POR CARLOS MELO\n \n \n Visitantes: 1367 \nA quantidade de visitantes diz respeito a visitantes únicos visitando qualquer \npágina do domínio ou subdomínio sigmoidal.ai. Compreende, então, cursos, \nblogs e landing pages. \n Inscritos: 33 \nÉ considerado aqui o número de leads gerados por meio de cadastro \nvoluntário nos formulários do cabeçalho, rodapé ou materiais ricos (como \neBook, infográficos, entre outros). \n Assinantes: 6 \nClientes assinantes da Escola de Data Science, considerando-se o plano \nrenovável de assinatura mensal. \n \n \n
```

这就是你开始处理文本的方式。假设我们想要这个文件包含的利润值，即“1397，00”，我们必须清除这个输出，直到我们得到字符串“1397.00”，然后我们必须将它转换为浮点型。如果你想一步一步地看这个过程，你可以看看我为这个项目做的笔记本。不管怎样，代码应该是:

```
float(text.split("\n")[6].replace("\t", "").split("R$")[1])
1397.00
```

假设您有许多遵循相同文本模式的文件，您可以创建一个“for 循环”,然后 Python 将遍历所有文件并返回每个文件的利润值。

```
sum = 0 #make a counter#making the functionfor reports in week_files:
     report = pdfplumber.open(reports)
     page = report.pages[0]
     text = page.extract_text() #extracting the text
     value = text.split("\n")[6].replace("\t", "").split("R$")[1]
     value = float(value)
     sum += valueprint("{} ----> {}".format(reports, value))
```

如果你喜欢这个教程，请与你的朋友分享，并留下你最喜欢的和我可以做得更好的评论，不要忘记在 [LinkedIn](https://www.linkedin.com/in/vin%C3%ADcius-porfirio-purgato-7891401b3/) 和 [GitHub](https://github.com/vinny380) 上添加我，如果你有任何问题，不要犹豫。

参考:

[背景](https://www.freepik.com/vectors/background)starline 创建的矢量—[www.freepik.com](http://www.freepik.com)

[](https://colab.research.google.com/drive/1SObGEF2I2GUt07wmqDELpIdLBpmmHG6p#scrollTo=d0izPqVnWZ9L) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/drive/1SObGEF2I2GUt07wmqDELpIdLBpmmHG6p#scrollTo=d0izPqVnWZ9L) 

[https://sigmoidal.ai/blog-sigmoidal/](https://sigmoidal.ai/blog-sigmoidal/)