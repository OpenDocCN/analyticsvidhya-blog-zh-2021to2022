# 在 Streamlit 上部署图像分类模型

> 原文：<https://medium.com/analytics-vidhya/deploying-image-classification-model-on-streamlit-e9579ccda157?source=collection_archive---------9----------------------->

这是我的图像分类模型的第二阶段。关于如何做到的文章可以在这里找到[。](/analytics-vidhya/image-classification-with-transfer-learning-37d2a2d9ab41)

在第二阶段，我部署了模型来简化它。要做到这一点，你需要申请一个邀请，一旦你得到了邀请，你就可以开始你的部署。

# 安装包:

首先，您需要使用以下命令将 streamlit 安装到您的本地计算机上

*pip 安装流水线*

然后 run => *streamlit 你好*

这将在您的本地主机上打开网页。更多信息，你可以查看他们的[文档](https://streamlit.io/)。

# [App.py](http://app.py/) :

对于我的项目，我已经将我的模型保存为. hdf5 文件。我创建了一个名为 [app.py](http://app.py/) 的新文件，然后导入了包含 streamlit 的库。由于我的模型涉及迁移学习，我必须导入 tensorflow hub。

对于 UI，我创建了一个自定义 css 文件，并在我的 [app.py](http://app.py/) 中将其作为文件打开。然后我创建了标题和 markdown 来解释 web 应用程序的功能。

在我的 def main()中，我添加了文件上传功能，以及一个指向预测模型的按钮来预测上传的图像类别。

在 def predict(图像)中，我调用了我已经保存的分类器模型，定义了图像形状和模型变量。模型变量，我必须使用 Keras 的自定义对象函数，因为模型不是来自 hub。虽然这个过程是通过导入 Tensorflow hub 库实现的。

接下来，我创建了测试图像变量，调整了它的大小，将它改为一个数组，缩放它，并使用 numpy 库来扩展维度。然后我创建了类名，它将作为预测图像的标签。

对于预测，我对测试图像使用了 model.predict 函数，使用 softmax 和 numpy 得到分数，最后是列出了类名的结果。

# 部署模型:

为了部署模型，我必须安装将创建 requirements.txt 文件的库。当您的代码运行时，这个文件将使您的模型能够在另一台机器上工作。

`pip install pipreqs`

然后= >*pip reqs/path/to/project*。

实时项目:

[袋分类器](https://share.streamlit.io/nwosu-ihueze/first_deploy/main/app.py)是直播项目的链接。

演示:

# 结论:

这是一个简单的图像分类的 streamlit 项目，你需要的一切都可以在这里找到。您可以在 [LinkedIn](https://www.linkedin.com/in/rosemary-nwosu-ihueze/) 上与我联系，寻求建议或更正。感谢您的阅读。