# 自己动手，为自动化机器学习开放黑客

> 原文：<https://medium.com/analytics-vidhya/do-it-yourself-open-hack-for-automated-machine-learning-62a3e6d54a56?source=collection_archive---------4----------------------->

# 使用开源的泰坦尼克号数据集来运行自动机器学习

# 先决条件

*   Azure 帐户
*   创建资源组
*   创建 Azure 存储帐户
*   创建 Azure 机器学习服务

# 数据

*   数据可在回购中获得
*   文件名 Titanic.csv
*   将文件下载到本地硬盘，以便将来上传

# 建立模型

*   登录 Azure 门户网站
*   转到 Azure 机器学习服务资源
*   开放 Azure 机器学习工作室

# 创建数据集

*   转到数据集
*   单击创建数据集

![](img/dc84a2ac7ef5e6caa45ded2c42c3db44.png)

*   从本地文件中选择
*   给数据集起个名字:TitanicTraining

![](img/e48f3048f6048bce7a18a2d19d3e448f.png)

*   单击下一步
*   将文件上传到默认数据存储

![](img/c77a426138a6aa33908cd605202094c6.png)

*   选择上传文件

![](img/b74a4fbfbd28eb2cec4c9add336b6f20.png)

*   从本地硬盘选择文件

![](img/fd75c7a0cf335d1a859ed07f863b24a2.png)

*   上传文件并单击下一步

![](img/6288ff0d24449282a6c87da36887d8e6.png)

*   验证架构，然后单击下一步

![](img/8e21a9c7b41a3debdfcf3320cd08eaae.png)

*   检查数据类型，保持不变，然后单击下一步

![](img/c0ac56a92072b2504b934cd9f37d5fa0.png)

*   单击创建

![](img/cab6f7c3f515602cbec2fa1c7a3e7fda.png)

# 创建计算集群

*   转到计算

![](img/1d30fd60937ff7f00ac2f3583dd7c89a.png)

*   单击计算集群
*   创建新的计算集群
*   以下是我选择的计算:

![](img/b10288cd647771664d3b12fff532c62c.png)

*   现在选择计算

![](img/c482a4ad0e79917f9bd704d1bb214313.png)

*   给个名字

![](img/fdac21234bff0dcf0c2ebca261b86345.png)

# 自动化 ML 实验

*   创建自动化 ML

![](img/b0822e4515771407e1fd0e367837ea78.png)

*   命名为:TitanicOpenHackExp

![](img/8ce99d14c4c9725d376ca694fa2da127.png)

*   给一个实验命名
*   选择标签列
*   选择计算

![](img/52af85de05a279d4efbba5ebe57fb46d.png)

*   选择建模分类或回归的类型

![](img/e54a3ea8cc9f18be391d1be5d240f759.png)

*   单击下一步，然后单击完成

![](img/6161eed9d611a8161e90ab1ade136a16.png)

*   等待实验完成
*   通常使用 F16 和 4 个节点需要大约 25 分钟
*   根据数据大小和计算大小，时间可能会有所不同。

# 结果和日志

*   这是实验运行屏幕

![](img/b8839d95fdca05d190c42776ec3fb7e2.png)

*   完成后应该如下图所示

![](img/db4a9bd6cfd975f842cac0cc3753ffb4.png)

*   单击模型查看所有模型的运行

![](img/65b366d53b84166bc3960aac89cb77b3.png)

*   选择带视图解释的模型，因为这是最佳模型

![](img/91ac5ff07e650b520880ea849ed9c89c.png)

*   现在点击解释选项卡并查看详细信息
*   这是打开黑盒的地方，并显示哪些特征有助于预测

![](img/f46f6f2e736cc0523a0e0b8f60a38730.png)

*   现在，单击指标进行查看
*   精确召回

![](img/ab8d3404948823d6d5821f95ff425b2c.png)

*   受试者工作特征曲线

![](img/9ccd03e8a3e33b0863364e8c44f49ee2.png)

*   校准曲线

![](img/fb05252b30b19d8c9e3073bd86c8badd.png)

*   升力曲线

![](img/875dbc1f86be46b115664b51d240e167.png)

*   累积增益曲线

![](img/49e4da4aa2e1a3f97fbe7abe0e94389d.png)

*   日志可用

![](img/db373551732fce5a5a32885ad1b53357.png)

*   建模完成
*   接下来部署模型。未完待续。
*   原文—[samples 2021/endtoendopenhacktitanicatoml . MD at main balakreshnan/samples 2021(github.com)](https://github.com/balakreshnan/Samples2021/blob/main/OpenHackAutoML/EndToEndOpenHackTitanicautoml.md)