# 在 Python 中隐藏密钥和密码

> 原文：<https://medium.com/analytics-vidhya/hiding-secret-keys-and-passwords-in-python-2950c6a4359?source=collection_archive---------0----------------------->

![](img/891defb32767732f9f01708443c29f38.png)

谷歌图片

每当你在 GitHub 上上传包含一些密钥和密码的项目或代码时，如果该回购是公开的，请记住，运行该文件的用户至少拥有对它的读取权限，并且可以轻松获取密码。总是非常仔细地考虑安全性，这很重要。例如，您必须始终将您的秘密密钥隐藏在 Django 应用程序、google OAuth 凭证密钥或令牌等中。