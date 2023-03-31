# 深度学习:加载和操作我们的模型

> 原文：<https://medium.com/analytics-vidhya/deep-learning-loading-and-operationalizing-our-model-13c4da002225?source=collection_archive---------13----------------------->

![](img/157020ab5cb10ad2007faa068d14cb48.png)

*图片:Shutterstock*

如果我们不能使用它们，建立神经网络/模型有什么必要？在本文中，我们将看看如何操作一个神经网络。

> 这是我上一篇关于使用 cifar-10 创建图像分类器的文章的延续。你可以在这里找到[。](/analytics-vidhya/deep-learning-creating-an-image-classifier-using-pytorch-with-cifar-10-f603659722b2)

在前一篇文章中，我们训练并保存了我们的模型。在本文中，我们将使用 python flask 加载模型并进行操作。

我们需要完成三个主要步骤:

*   加载我们的模型
*   编写预测函数
*   包装到烧瓶应用程序

# 加载我们的模型

在上一篇文章中，我们用名称 *checkpoint.pth* 保存了我们的模型。为了加载它，我们使用了 [torch.load](https://pytorch.org/docs/stable/generated/torch.load.html) 函数。然后我们重建我们的模型。

请注意，为了重建工作，我们需要我们的 Net 类，所以我们将把它带过来。

```
# let's do our importa hereimport torch
from PIL import Image
import torchvision
import torchvision.transforms as transforms
import numpy as np
import torch.nn as nn
import torch.nn.functional as F
```

```
# the cifar-10 class labels
classes = ('plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck')# our nn
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(3, 32, 3)
        self.conv2 = nn.Conv2d(32, 64, 3)
        self.conv3 = nn.Conv2d(64, 128, 3)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(128 * 2 * 2, 128)
        self.fc2 = nn.Linear(128, 64)
        self.fc3 = nn.Linear(64, 32)
        self.fc4 = nn.Linear(32, 10)
        self.dropout1 = nn.Dropout(p=0.2, inplace=False) def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.dropout1(x)
        x = self.pool(F.relu(self.conv2(x)))
        x = self.dropout1(x)
        x = self.pool(F.relu(self.conv3(x)))
        x = self.dropout1(x)
        x = x.view(-1, 128 * 2 * 2)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = F.relu(self.fc3(x))
        x = self.fc4(x) #output layer return x
```

让我们检查一下我们的机器是否有 GPU，如果没有，我们使用 CPU

```
device = 'cuda' if torch.cuda.is_available() else 'cpu'
```

现在让我们来看看加载模型的函数

```
# function to load and reconstruct the model
def load_checkpoint(filepath): # we do map_location=device so that in case the model was trained with GPU and then we're trying to load it on a machine that has but CPU, it should still work
    checkpoint = torch.load(filepath, map_location=device)
    model = checkpoint['model']
    model.load_state_dict(checkpoint['state_dict']) for parameter in model.parameters():
        parameter.requires_grad = False

    # put our model to evaluation mode
    model.eval() return model
```

# 预测函数

现在我们有了要加载和重建模型的代码片段，让我们编写一个函数来进行预测。该函数将图像路径作为参数，并返回预测的标签。

```
def do_predcition(filepath):
    pil_image = Image.open(filepath)
 # transform the image to a tensor and resize to fit our trained model size  
    img_loader = transforms.Compose([
    transforms.Resize((32,32)),
    transforms.ToTensor()]) ts_image = img_loader(pil_image).float()
    ts_image.unsqueeze_(0) outputs = model(ts_image.to(device)) _, predicted = torch.max(outputs.data, 1)    # the prediction is a list, so we have to get the first element    
    return classes[predicted[0]]
```

在这一点上，我们有了一切与神经网络有关的东西。我们剩下的是将函数包装到 flask 应用程序中。

# 烧瓶应用程序

让我们获取 flask 应用程序所需的导入。

```
# imports specific to the flask app
from flask import Flask, render_template, request
from werkzeug.utils import secure_filename
import os
```

基本上，我们有一个简单的文件夹结构:

```
root
--templates
----index.html
--app.py
```

*index.html*是一个允许用户选择图像的简单表单。如果你想用 postman 测试模型端点，这部分甚至可以省去。*index.html*页面相当简单:

```
<--index.html-->
<html>
    <body>
        <form action = "/uploader" method = "POST" enctype = "multipart/form-data">
            <input type = "file" name = "file" />
            <input type = "submit"/>
        </form>
    </body>
</html>
```

现在我们来看一下 *app.py* 代码。我们基本上有两条路线，然后引入预测代码。

```
# bring all imports hereapp = Flask(__name__)# bring Net class and the other functions and codes heremodel = load_checkpoint('checkpoint.pth')
model.to(device)@app.route('/')
def upload_file_page():
    return render_template('index.html')@app.route('/uploader', methods = ['GET', 'POST'])
def upload_file():
    image_path = ''
    if request.method == 'POST':
        f = request.files['file']
        f.save(secure_filename(f.filename))
        image_path = "./" + secure_filename(f.filename) # call our prediction function
        category = do_predcition(image_path) # delete the uploaded file
        os.remove(image_path) return category
    # you can hangle the get request here anyhow you'll likeif __name__ == '__main__':# uncomement this line if you want to run inproduction
#   app.run(host='0.0.0.0', port=80) app.run(debug = True)
```

然后，您可以使用 python app.py 运行应用程序

如果你在本地运行，那么你可以在 [http://127.0.0.1:5000/](http://0.0.0.0:80/) 找到这个应用

如果你在你的云服务器上运行，你可以通过你的服务器公共 ip 访问(就像我的例子)，当然是在上面的 app.py 代码上设置了正确的配置之后。

仅此而已。

感谢您的宝贵时间！希望你喜欢它！

# 参考

*   [火炬装载](https://pytorch.org/docs/stable/generated/torch.load.html)
*   [烧瓶文件](https://flask.palletsprojects.com/en/1.1.x/quickstart/)