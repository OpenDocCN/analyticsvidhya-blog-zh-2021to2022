# 通过 5 个简单的步骤将 Streamlit 应用部署到 Google App Engine。

> 原文：<https://medium.com/analytics-vidhya/deploying-streamlit-apps-to-google-app-engine-in-5-simple-steps-5e2e2bd5b172?source=collection_archive---------2----------------------->

![](img/431bad248796bb084f9227df5a1b2633.png)

照片由[卢卡斯](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/macbook-pro-beside-papers-669619/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

这篇文章将带你了解如何使用`[streamlit](https://www.streamlit.io/)`包构建一个 python 应用程序，对其进行 dockerize 并将其部署到 [Google App Engine](https://cloud.google.com/appengine) 。

# 您需要什么:

[**git**](https://git-scm.com/downloads) **—** 克隆 GitHub 存储库，其中包含一个可以部署的 streamlit 应用程序。
[**Docker**](https://www.docker.com/get-started)—捆绑应用代码、需求和配置…