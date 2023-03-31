# 在姜戈发送邮件

> 原文：<https://medium.com/analytics-vidhya/sending-mails-in-django-bbf6987caea1?source=collection_archive---------4----------------------->

![](img/d9529d703d3a0fbe9bfeb02c03f87356.png)

谷歌图片

在本文中，我们将介绍如何使用 Django 提供的邮件功能发送电子邮件。

首先，创建一个 Django 项目，以便我们可以测试我们的功能。

只需按照这些命令来设置您的项目

```
pip install django
django-admin startproject EmailTutorial
cd EmailTutorial
django-admin startapp email_app
```