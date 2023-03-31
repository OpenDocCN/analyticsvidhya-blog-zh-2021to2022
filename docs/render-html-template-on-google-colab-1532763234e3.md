# 在 Google Colab 上呈现 HTML 模板

> 原文：<https://medium.com/analytics-vidhya/render-html-template-on-google-colab-1532763234e3?source=collection_archive---------3----------------------->

HTML(超文本标记语言)是定义网页及其内容如何组织的代码。例如，可以使用段落序列、项目符号列表或照片和数据表来安排内容。

Python 的 **Flask** API 帮助我们创建 web 应用程序。Web 应用程序框架是模块和库的集合，允许程序员构建应用程序，而不必编写包括协议和线程管理在内的低级代码。

![](img/62d54738b87d4543877233105be9c804.png)

下面我们将看看如何使用 flask 来呈现一个 HTML 模板。

1.  打开 Google Colab，在里面新建一个笔记本。

![](img/100f1df5000d318541ecc90db322b086.png)

2.然后，安装 flask-ngrok。因为 google colab 提供了虚拟机，所以我们不能访问本地主机。因此，我们将使用 flask web 应用程序通过 **ngrok 访问一个公共 URL。**

![](img/8570ef2e1666c213c17a8da05857b4f1.png)

3.安装 flask-bootstrap(可选)

![](img/3cfd6a84c4b9c56def05324fde628ffe.png)

4.从 flask 中导入所有需要的文件，如下所示。

![](img/d216ce1141c311b1fded709651ca06f7.png)

5.在 Google Colab 的 Files 目录下创建一个名为“static”的文件夹。将所有必要的 CSS、JS、字体和图像文件夹上传到该文件夹。

![](img/5267dc1794cfac6b884b669182eda72f.png)![](img/e203ac98df4da7062cd2375c8dbebd1a.png)

6.在 Files 目录下创建另一个名为“template”的文件夹。然后，上传所需的。html 文件来呈现。

![](img/b00ff5e333290b70d61acc3f2337c539.png)

7.运行以下代码来呈现 html 模板。

![](img/e5a7ab7b7c53bbbd4808717e6e2f1d8e.png)

8.单击 ngrok 的安全 URL，在另一个选项卡中查看您的 web 应用程序。

***注意:-*** *在运行代码之前，请确保所有的 CSS、js、字体和图片文件的源文件路径在。html 文件。*