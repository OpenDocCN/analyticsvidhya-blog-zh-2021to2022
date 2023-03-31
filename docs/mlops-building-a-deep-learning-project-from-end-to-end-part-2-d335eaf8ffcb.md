# m lops-构建端到端的深度学习项目(第二部分)

> 原文：<https://medium.com/analytics-vidhya/mlops-building-a-deep-learning-project-from-end-to-end-part-2-d335eaf8ffcb?source=collection_archive---------11----------------------->

![](img/78a4cee950fbe3c9a8a8918cc5f6f94c.png)

信用——valohai.com

在前面的部分中，我们已经完成了管道的构建，并使用 DVC 来执行它们。现在，我们将转向部署部分，在此之前，我们需要一个 web 应用程序，用户可以通过它进行交互。为了创建 web 应用程序，我们将需要 HTML，CSS 和 js 的头部到我的 GitHub repo 内的 web 应用程序，你会发现相同的“静态”，“模板”和“上传”目录。

内容

1.  使用 flask 创建 web 应用程序
2.  使用 GitHub 操作在 Heroku 上部署应用程序

## 1.使用 flask 创建 web 应用程序

现在，我们将使用 flask 创建一个 API，以便它可以通过 web 应用程序接收输入并返回正确的预测。关于建筑，请参考以下规范。

```
import os
os.environ["CUDA_VISIBLE_DEVICES"]="-1" 
from flask import Flask,render_template,url_for,request
from werkzeug.utils import secure_filename
from keras.models import load_model
#from keras.preprocessing.image import image
import numpy as np
import os
from tensorflow.compat.v1 import ConfigProto
from tensorflow.compat.v1 import InteractiveSession
#from tensorflow.keras.preprocessing import image
from PIL import Image
import cv2app = Flask(__name__)
app.config['upimg'] = os.path.join('uploads')model1 = load_model("saved_models/trained.h5")def img_predict(pat,model):

    load_img = cv2.imread(pat)
    load_img = cv2.resize(np.float32(load_img),(225,225))
    imgg = np.array(load_img)
    #load_img = image.load_img(pat,target_size=(140,140))
    #imgg = image.img_to_array(load_img)
    imgg = np.expand_dims(imgg, axis = 0)
    pred = model.predict(imgg)
    #pred = pred[0]
    pred = np.argmax(pred)
    return str(pred)[@app](http://twitter.com/app).route("/")
def home():
    return render_template('new.html')[@app](http://twitter.com/app).route("/predict", methods = ['POST', 'GET'] )
def predict():
    if request.method == 'POST':
        fil = request.files['file']
        bas_path = os.path.dirname(__file__)
        fil_path = os.path.join('webapps','uploads',secure_filename(fil.filename))

        if not os.path.exists('uploads'):
            os.mkdir('webapps/uploads')

        fil.save(fil_path)

        preds = img_predict(fil_path, model1)

        if preds == '0':
            opp = 'Its Bulbasaur!'

        elif preds == '1':
           opp = 'Its Charmander!'

        else:
            opp = 'Its Squirtle!'

        return opp
    return None

if __name__ =='__main__':
    app.run(debug = True)
```

一旦你运行这个，你会得到下图所示的链接

![](img/eab6ddc2499eb8c5e66d11e7eabd1d6b.png)

1.0-运行 flask 应用程序后

复制 URL 并将其粘贴到任何浏览器中，然后按“enter”键，您将被重定向到如下所示的页面。

![](img/a79a26df8d0b6efd7850704c675bfdf1.png)

1.2-转到 flask URL 后

现在我们的 web 应用程序运行在我们的本地系统上，我们需要使用 GitHub 操作将它部署在 Heroku 中，这将在下一部分中进行。

## 2.使用 GitHub 操作在 Heroku 上部署应用程序

出于部署目的，我们需要另一个配置文件，因此运行以下命令。

```
mkdir .github\workflows
type null >> .github\workflows\ci-cd.yaml
```

因为我使用的是 windows 操作系统，所以我对 Linux 使用了“type null ”,对 Linux 使用了“touch ”,对路径也使用了“/”。
在“ci-cd.yaml”文件中粘贴如下所示的配置。

```
name: Python-DL-MLOPSon:
  push:
    branches:
    - main
  pull_request:
    branches:
    - mainjobs:
  build:runs-on: ubuntu-lateststeps:
    - uses: actions/checkout@v2
      with:
        fetch-depth: 0
    - name: Set up Python 3.7
      uses: actions/setup-python@v2
      with:
        python-version: 3.7
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip

        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi#- name: Deploy to Heroku
#      env:
#        HEROKU_API_TOKEN: ${{ secrets.HEROKU_API_TOKEN }}
#        HEROKU_APP_NAME: ${{ secrets.HEROKU_APP_NAME }}
#      if: github.ref == 'refs/heads/main' && job.status == #'success'
#      run: |
#        git remote add heroku #[https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git](https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git)
#        git push heroku HEAD:main -f
```

在上面的文件中，
1。名称:是部署
2 的名称。推、拉:是推/拉所有文件的分支
3。jobs，build:是部署管道中的状态。运行:它将使用的操作系统
5。步骤:部署管道
6 中涉及的步骤。use，with:它还需要哪些依赖项以及另一个
7。名称:管道
8 内单个步骤的名称。运行:执行什么命令
9。env:Heroku 的环境凭证/令牌

现在只需通过以下方式将更改推送到 Github repo

```
git add. && git commit -m "commit msg" && git push origin main
```

接下来，到你的 github repo，你会注意到一个像这样的黄点

![](img/a3d4022c7eb7c5372365f617923aff81.png)

1.3- github 操作

它只是显示部署依赖项正在安装。然后单击上面的“操作”选项卡。

单击后，您将看到以下页面，

![](img/915215072fb4de66ea1c479c5efee6aa.png)

1.4-运行工作流

观察黄点，它显示了运行工作流已经进行了多少次推送。每当新的变更被推送到 repo 时，部署管道就会被执行。
这就是为什么术语‘CD’持续部署的原因。
点击上面的工作流程，您会看到以下内容

![](img/8ba414a0405b4267040121ab3f18b01a.png)

1.5-独立管道执行

在正在进行的构建中，执行“ci-cd.yaml”中提到的所有配置和阶段。
构建成功完成后，您将看到以下内容

![](img/170d1db89342bd5a3a83ea0c9e170e2d.png)

1.6-构建成功

2.2 在 Heroku 中创建新应用程序

到目前为止，我们已经建立并执行了管道中的各个阶段，但还剩下最后一部分，即在 heroku 上的部署，所以让我们开始吧。

转到 [URL](https://dashboard.heroku.com/apps) ，如果您没有登录，请在此处登录，否则如果您是新用户，它会要求您创建一个新帐户。之后，您将进入如下所示的控制面板

![](img/7f271b70daa4b466d56d38b431b8f0ce.png)

1.7-Hreoku 仪表板

我们必须创建一个新的应用程序，点击右上角的新按钮，如上所示。

它会要求你为你的应用程序输入任何名称，所以键入你喜欢的任何名称，然后按“创建应用程序”。

![](img/bf58bd094807fde84ce88b1b2d040cd1.png)

1.8-创建应用程序

然后向下滚动，如果你已经用 Heroku 认证了 GitHub，你将可以选择搜索你的 repo 并与之连接。

> 注意:如果你还没有向 heroku 授权 github，首先它会要求你这样做，点击链接并输入你的 github 证书，然后按“认证”。

![](img/85181843690fa24cff8de576bb92cbb2.png)

1.9-与 Github repo 连接

键入您要连接的 repo 的名称，然后按“连接”。

连接后向下滚动，您将看到“自动部署”，确保您在主分支上，并勾选“部署前等待配置项通过”

![](img/f8b37f22403660d8d0ab60467072bd68.png)

2.0-自动部署

继续下一步复制你给的应用程序的名称，并返回到 repo 导航到如下所示的设置

![](img/e3f562890640ffb8188f5a994944dd7d.png)

2.1-设置

向下滚动一点，你会看到一个“秘密”选项，点击它

![](img/d795b9cb30a3893d4275a2cab0c34bb0.png)

2.2-秘密

点击后，你会看到如图所示，然后选择“新的存储库密码”。

![](img/424f3fc9adb8e112df032cc91cc3e742.png)

2.3-行动秘密

![](img/2cbe16680db267ce48e898978a0ccfa0.png)

2.4-输入数值

如上图所示，输入名称“HEROKU_APP_NAME ”,在值中粘贴 HEROKU 应用名称，然后单击“添加密码”。

前往 Heroku 页面，然后导航至个人资料->帐户设置->应用程序->创建授权。看下面的图片，然后照着做。

![](img/cd8db79d7693e0810773098db36766cb.png)

2.5-账户设置

![](img/09988abe2ee48b6a286d985c8c8ed287.png)

2.6-应用

![](img/5c08cdbc4caf87b1f9d2da5c58f20dd3.png)

2.7-创建授权

在描述部分输入任何信息，然后按“创建”。

![](img/87b07990ec929700014707eb2bfba34a.png)

2.8-复制令牌

复制令牌并返回 GitHub repo，按照图 2.1 到 2.4 中所示的相同步骤操作。在名称部分输入“HEROKU_API_TOKEN ”,在值部分粘贴复制的 HEROKU 令牌。

> 注意:将生成的将因人而异。

走向最后一步我们需要做的就是取消 Heroku 部署最后阶段的注释，并将更改推回到 Github repo。

![](img/6cafee972cce167cad413e0f25c45075.png)

2.9-删除注释

在此之前，我们需要创建一个“Procfile ”,这样 Heroku 就可以理解在 terminal/cmd 中运行哪个应用程序。

```
type nul >> Procfile
```

在 Procfile 中粘贴以下代码片段

```
web: gunicorn app:app
```

现在，通过以下方式将变更推送到回购

```
git add . && git commit -m "your commit message" && git push origin main
```

如果一切顺利，你会看到

![](img/925b628cb8171fb3baeedbf442ad53eb.png)

3.0-在 Heroku 部署后

> 注意:在推送时，请确保临时删除“data”和“data_set”文件夹，就像您使用它推送一样，您的应用程序的 slug 大小将超过 500Mb，您的部署将失败。

朝着网址前进

```
[https://app-name.herokuapp.com/](https://dl-mlops.herokuapp.com/)
```

在这里，用您自己的 Heroku 应用名称替换 URL 中的“应用名称”。
一旦你点击那个网址，你会看到我们的网络应用如下。

![](img/48e7c359ada09b2c24123125e1863f41.png)

3.1-网络应用程序

最后，我们完成了！这是一个稍微长一点的过程，但基本上是这样实现的。大多数行业更喜欢这些管道，因为它提高了效率，有助于跟踪我们的数据和变化。

我只是展示了一个使用 DVC 实现它的简单方法，但它有更多的功能，可以根据项目的类型进行合并。根据项目的复杂性，每个项目都有不同的架构，您可以相应地使用各种其他工具。

这只是如何开始使用 DVC 的 MlOps，你也可以使用其他替代方案，如 MLflow 和云平台，如 AWS、GCP、Azure。但是我认为 DVC 是一个非常强大的地方，也是一个开始 MlOps 的好地方。

如果你错过了第一部分，请点击这里查看第一部分的完整源代码，这是我的 [GitHub](https://github.com/patrickn699/DL-MLOPS.git) 。另外，如果你有任何疑问，你可以通过 [Linkedin](https://www.linkedin.com/in/prathmesh-patil-b151051a3) 联系我。
我真心希望您喜欢这篇文章，它有助于您开始使用 MLOps！