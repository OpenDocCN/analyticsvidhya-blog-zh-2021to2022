# 在 Pytorch 中创建自定义数据集和数据加载器

> 原文：<https://medium.com/analytics-vidhya/creating-a-custom-dataset-and-dataloader-in-pytorch-76f210a1df5d?source=collection_archive---------0----------------------->

训练深度学习模型需要我们将数据转换成模型可以处理的格式。例如，模型可能需要宽度为 512、高度为 512 的图像，但是我们收集的数据包含宽度为 1280、高度为 720 的图像。因此，我们需要某种方法来将我们现有的可用数据转换成模型所需的精确格式。

简单地说，dataloader 是一个函数，它遍历所有可用的数据，并以批处理的形式返回这些数据。例如，如果我们有一个包含 100 幅图像的数据集，我们决定将数据的大小定为 4。我们的数据加载器将处理数据，并返回 25 批，每批 4 张图像。

创建数据加载器可以通过多种方式完成，并且不需要任何方式的 torch。然而，使用 torch 使任务变得容易得多。记住这一点，让我们从理解 Torch 数据集和 Dataloder 类包含什么开始。

**火炬数据集:**

1.  Torch 数据集类基本上是一个表示数据集的抽象类。它允许我们将数据集视为一个类的对象，而不是一组数据和标签。
2.  **数据集**类的主要任务是每次被调用时返回一对【输入，标签】。我们可以在类中定义函数来预处理数据，并以我们需要的格式返回数据。
3.  该类必须包含两个主要函数:
    **__len__()** :这是一个返回数据集长度的函数。
    **__getitem__()** :这是一个返回一个训练示例的函数。
4.  torch 数据集类可以从**torch . utils . data . dataset**导入

**火炬数据加载器:**

1.  Torch Dataloader 不仅允许我们批量迭代数据集，还允许我们访问用于**多重处理**(允许我们并行加载多批数据，而不是一次加载一批数据)**混排**等的内置函数。
2.  torch Dataloader 将 torch 数据集作为输入，并从 Dataset 类调用 __getitem__()函数来创建一批数据。
3.  torch dataloader 类可以从**torch . utils . data . data loader**导入

**代号:**

*   我不会深入讨论训练模型的整个过程，但是我会一步一步地解释创建数据集类和数据加载器的过程。
*   对代码的要求将是:
    **numpy**:pip 3 install numpy
    **opencv**:pip 3 insall opencv-python
    **torch**:pip 3 install torch
    **glob**:pip 3 install glob
*   我为分类模型的任务创建了一个样本数据集，对猫和狗进行分类。文件夹结构如下。我们有包含代码 **Main.py** 的**项目**文件夹，以及名为 **Dog_Cat_Dataset** 的文件夹。这个名为 **Dog_Cat_Dataset** 的文件夹是数据集文件夹，其中包含两个子文件夹，分别名为 **dogs** 和 **cats** 。“狗”和“猫”文件夹各有 5 张图片。

```
- **Project**
    - **Main.py**
    - **Dog_Cat_Dataset**
        - **dogs**
           - 1.jpg
           - 2.jpg
           - 3.jpg
           - 4.jpg
           - 5.jpg
        - **cats**
           - 1.jpg
           - 2.jpg
           - 3.jpg
           - 4.jpg
           - 5.jpg
```

*   让我们从一些导入开始代码 **Main.py** 。

```
*import* glob
*import* cv2
*import* numpy as np
*import* torch
*from* torch.utils.data *import* Dataset, DataLoader
```

*   **glob:** 允许我们轻松地检索子文件夹中数据的路径
    **cv2:** 用作图像处理库来读取和预处理图像
    **numpy:** 用于矩阵运算
    **torch:** 用于创建 Dataset 和 Dataloader 类，并将数据转换为张量。

```
class CustomDataset(Dataset):
    def __init__(self):
        self.imgs_path = "Dog_Cat_Dataset/"
        file_list = glob.glob(self.imgs_path + "*")
        print(file_list)
        self.data = []
        *for* class_path in file_list:
            class_name = class_path.split("/")[-1]
            *for* img_path in glob.glob(class_path + "/*.jpeg"):
                self.data.append([img_path, class_name])
        print(self.data)
        self.class_map = {"dogs" : 0, "cats": 1}
        self.img_dim = (416, 416) def __len__(self):
        *return* len(self.data) def __getitem__(self, idx):
        img_path, class_name = self.data[idx]
        img = cv2.imread(img_path)
        img = cv2.resize(img, self.img_dim)
        class_id = self.class_map[class_name]
        img_tensor = torch.from_numpy(img)
        img_tensor = img_tensor.permute(2, 0, 1)
        class_id = torch.tensor([class_id])
        *return* img_tensor, class_id
```

*   让我们详细讨论一下这个类。

```
class CustomDataset(Dataset):
```

*   我们创建一个名为 **CustomDataset** 的类，并传递参数 **Dataset** ，以允许它继承 Torch Dataset 类的功能。

```
def __init__(self):
    self.imgs_path = "Dog_Cat_Dataset/"
    file_list = glob.glob(self.imgs_path + "*")
    print(file_list)
```

*   我们定义 init 函数来初始化变量。变量 **self.imgs_path** 包含我们的 Dog_Cat_Dataset 文件夹的基本路径
*   然后，我们使用 glob 检索我们指定的基本文件夹中所有文件夹的列表。在我们的示例中， **Dog_Cat_Dataset** 文件夹包含两个子文件夹，分别名为 **dogs** 和 **cats** 。添加到 **self.imgs_path** 中的“*”项表示我们想要搜索指定路径中的所有文件夹或文件。(注意，使用“*”操作将返回文件夹中的每个文件，而不仅仅是文件夹名。所以要确保这个位置除了猫狗文件夹之外没有其他东西。)
*   print 语句将返回以下输出:
    **[' Dog _ Cat _ Dataset/dogs '，' Dog_Cat_Dataset/cats']**
*   这个 file_list 变量包含数据集中所有类的列表(狗和猫)。

```
self.data = []
*for* class_path in file_list:
    class_name = class_path.split("/")[-1]
    *for* img_path in glob.glob(class_path + "/*.jpeg"):
        self.data.append([img_path, class_name])
print(self.data)
```

*   我们现在开始创建数据列表，它将包含数据集中所有图像的路径。
*   我们遍历文件列表中的所有类(狗和猫)，对于每个类，我们首先从提取实际的类名开始。正如您可以从打印的文件列表中看到的，每个类都是根据其基本路径来表示的；即狗 _ 猫 _ 数据集/狗。因为我们更愿意把类称为狗和猫，而不是关于它的路径，我们通过用“/”分割这个 **class_path** 来创建一个 **class_name** 变量。(用“/”分割将返回一个列表["Dog_Cat_Dataset "，" dogs"]。采用[-1]索引将使用列表中的最后一个条目。在这种情况下，应该是“狗”。
*   现在我们正在遍历数据集的每个类(狗和猫)，我们希望检索它们文件夹中的每个图像(例如 1.jpeg、2.jpeg 等)。为此，我们再次使用 glob 返回文件夹中扩展名为"的所有文件。jpeg”。我们传递给 glob 的完整字符串是 Dog_Cat_Dataset/dogs/*。jpeg。“*。jpeg“表示我们需要扩展名为的每个文件”。jpeg”。
*   我们将每个图像的文件路径及其对应的类名附加到列表 self.data 中。这为我们提供了一种检索输入图像及其相应标签的方法。打印该列表将返回以下输出。

```
 [['Dog_Cat_Dataset/dogs/4.jpeg', 'dogs'],    
  ['Dog_Cat_Dataset/dogs/3.jpeg', 'dogs'], 
  ['Dog_Cat_Dataset/dogs/5.jpeg', 'dogs'],
  ['Dog_Cat_Dataset/dogs/1.jpeg', 'dogs'],
  ['Dog_Cat_Dataset/dogs/2.jpeg', 'dogs'],
  ['Dog_Cat_Dataset/cats/4.jpeg', 'cats'],
  ['Dog_Cat_Dataset/cats/3.jpeg', 'cats'],
  ['Dog_Cat_Dataset/cats/5.jpeg', 'cats'],
  ['Dog_Cat_Dataset/cats/1.jpeg', 'cats'],
  ['Dog_Cat_Dataset/cats/2.jpeg', 'cats']]
```

*   我们另外定义了一个类别映射和一个图像维度。 **self.class_map** 字典，允许我们将类的字符串转换成数字；即“狗”对应于类别 0，而“猫”对应于类别 1，并且图像尺寸是我们将调整所有图像的尺寸，使得它们都具有相同的尺寸。

```
self.class_map = {"dogs" : 0, "cats": 1}
self.img_dim = (416, 416)
```

*   现在我们已经创建了一个包含所有数据的列表，我们开始为 **__len__()** 编写函数，这对于 Torch Dataset 对象是必需的。

```
def __len__(self):
    *return* len(self.data)
```

*   我们的数据集的大小就是我们拥有的单个图像的数量，这可以通过 **self.data** 列表的长度来获得。(Torch 在内部使用该函数来了解其数据加载器中数据集的大小，以使用该数据集大小内的索引来调用 **__getitem__()** 函数)

```
def __getitem__(self, idx):
    img_path, class_name = self.data[idx]
```

*   我们首先定义 **__getitem__** 函数，将自身的对象作为输入( **self** )，以及一个 **idx** 。 **idx** 是这个函数将被调用的术语，对应于哪个图像需要从我们的 **self.data** 列表中返回。
*   我们从 **self.data** 列表中检索对应于 idx 的图像路径和类名。

```
img = cv2.imread(img_path)
img = cv2.resize(img, self.img_dim)
class_id = self.class_map[class_name]
```

*   我们使用 opencv 作为图像处理库来加载图像并将其调整到所需的尺寸。
*   我们使用 **imread** 函数读取图像，该函数从给定的图像路径加载图像，然后将其调整到我们在 **self.img_dim** 变量中指定的尺寸。
*   用于分类的深度学习模型通常利用与类相关联的编号 id，而不是名称(“dogs”-> 0)。 **self.class_map** 字典只是提供了从名字到号码的映射。

```
img_tensor = torch.from_numpy(img)
img_tensor = img_tensor.permute(2, 0, 1)
class_id = torch.tensor([class_id])
```

*   一旦我们加载了图像，并获得了相应的类 id，我们就将变量转换为张量。使用 torch 训练模型需要我们将变量转换为 torch 张量格式，其中包含计算梯度的内部方法等。
*   我们创建变量 **img_tensor** ，它是我们加载的 **img** 的张量形式。Opencv 使用库 numpy 将图像表示为矩阵， **torch.from_numpy** 函数允许我们将 numpy 数组转换为 torch 张量。
*   火炬卷积要求图像为通道优先格式；例如，3 通道图像(红色、绿色和蓝色通道)在 numpy 中通常表示为:(宽度、高度、通道)，但是 torch 要求我们将其转换为:(通道、宽度、高度)。
*   对于这种转换，我们使用 torch 的置换功能，它允许我们改变 torch 张量的维度排序。我们传递给它的参数，对应于我们想要的新的维度排序。例如，在我们的例子中，我们有(宽度、高度、通道)。
    (Width - > 0)，(Height- > 1)，(Channels- > 2)
    我们想要重新排序这些维度，以首先制作通道，因此，我们使用 **img_tensor.permute(2，0，1)** ，这将首先制作第二维度。
*   然后，我们将 **class_id** 的整数值转换为 torch 张量，并通过将其称为**【class _ id】**来增加其维数。这是为了确保数据可以在 torch 需要的维度上进行批量处理。(Torch 要求标签为[batch_size，label_dimension]形状。只使用 **class_id** ，而不使用**【class _ id】**会导致我们得到最终大小为【batch_size】，因为每个 **class_id** 只是一个单一值。

```
*return* img_tensor, class_id
```

*   我们返回了 **img_tensor** 和 **class_id** ，这样无论何时使用 **idx** 调用 **__getitem__** 函数，都会返回一个带有相应标签的图像。
*   注意:另外，torch 可能需要将返回的张量转换为 float 类型。这可以通过以下修改来实现:

```
*return* img_tensor.float(), class_id.float()
```

*   现在我们已经创建了数据集，我们可以在 torch dataloader 中使用这个类在训练时遍历这些数据。

```
*if* __name__ == "__main__":
    dataset = CustomDataset()
    data_loader = DataLoader(dataset, batch_size=4, shuffle=True)
```

*   为了测试数据集和我们的数据加载器，在我们脚本的主函数中，我们创建了我们创建的 **CustomDataset** 的一个实例，并将其命名为**数据集。**
*   Torch DataLoader 将这个**数据集**以及 batch_size、shuffle 等其他参数作为输入，这些参数是额外的术语，用于指定我们希望每批数据中有多少图像，以及我们是否希望随机生成我们之前使用的 **idx** 。

```
*for* imgs, labels in data_loader:
    print("Batch of images has shape: ",imgs.shape)
    print("Batch of labels has shape: ", labels.shape)
```

*   我们可以通过遍历数据加载器来测试它的工作情况，并打印输入图像的形状和标签。
*   torch 数据加载器确实需要很多额外的参数，比如:

1.  **no_workers** :对应于我们要并行加载数据的工作线程数量。使用更多的工作人员可以加快数据加载过程。
2.  **pin_memory** :在 GPU 设备上训练时，我们会希望加快将加载的数据从 CPU 传递到 GPU 的过程。因此，将其设置为 true 会加快数据传输速度。
3.  **drop_last** :如果有样本< batch_size，跳过最后一批数据。例如，在我们的例子中，我们的数据集中有 10 个图像，我们指定批量大小为 4。这意味着最后一批只有 2 个图像。设置 drop_last=True，将跳过这最后一批，因为它有< 4 个图像。

**注:**

1.  torch 的 dataset 类可以像 python 中的任何其他类一样使用，并且可以有任意数量的子函数，只要它有两个必需的函数( **__len__** 和 **__getitem__** )。
2.  如果可用，返回的数据也可以传递给 GPU。比如: **img_tensor.to("cuda ")。**
3.  torch 数据加载器有一个可用于
    [链接](https://pytorch.org/docs/stable/data.html#iterable-style-datasets)的附加参数列表
4.  我已经在 github 上传了这篇文章的完整代码:[代码](https://github.com/vineeth2309/Custom-Dataset-and-Dataloader-in-Torch)