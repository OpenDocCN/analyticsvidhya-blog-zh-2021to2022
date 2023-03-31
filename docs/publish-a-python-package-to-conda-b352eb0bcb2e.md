# 将 python 包发布到 Conda

> 原文：<https://medium.com/analytics-vidhya/publish-a-python-package-to-conda-b352eb0bcb2e?source=collection_archive---------7----------------------->

![](img/b7ba50805db4368a0eb0a1c916a49de4.png)

[https://github.com/conda/conda](https://github.com/conda/conda)

[Conda](https://conda.io/en/latest/) 是类似于 [pip](https://pypi.org/project/pip/) 的 python 包管理器。如果你正在构建一个 python 库，那么很有可能你也会把它发布给 Conda。否则，Conda 用户不会从你的库受益，这对你和你的用户都是不利的。Conda 文档一步一步地介绍了[如何从头开始将 python 包发布到 Conda](https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs.html)。但是如果你以前没有康达出版的经验，那么你将会面临很多麻烦，官方文档对你没有太大的帮助。在这里，我将详细解释每一个步骤，这将有助于澄清你所有的问题。

# 你需要什么？

您需要在您的构建环境中安装这些工具。

1.  [迷你康达](https://docs.conda.io/en/latest/miniconda.html#miniconda)
2.  [康达制造](https://docs.conda.io/projects/conda-build/en/latest/install-conda-build.html)
3.  [蟒蛇客户端](https://docs.anaconda.com/anacondaorg/user-guide/getting-started/#building-and-uploading-packages)

# 开始吧！

假设我要将名为`my-py-lib`的库发布给 Conda。

最重要的任务是为 python 库准备`meta.yaml`文件。这是除`[setup.py](https://docs.python.org/3/distutils/setupscript.html)`文件外，康达出版唯一需要的文件。是的，文档上说`build.sh`和`build.bat`也是必需的，但是相信我这不是！您可以使用用于 pip 的相同的`setup.py`文件来发布到 Conda。您可以将`meta.yaml`文件放在任何您想要的地方。但是在项目的根目录下创建一个名为 conda(或者您喜欢的任何其他名称)的新目录并将`meta.yaml`放在其中总是一个好主意。这是我的项目结构。

```
.
|-conda
| |-meta.yaml
|-setup.py
```

下面是将 Python 包发布到 Conda 所需的最少的`meta.yaml`文件。

## meta.yaml

```
package:
  name: "my-py-lib"
  version: "0.1.0"about:
  summary: "This is an awesome Python library"source:
  path: ..build:
  script: python setup.py install
```

要点:

*   `source.path` -应该是你的`meta.yaml`到`setup.py`文件*的相对路径。*
*   `build.script`——是构建 python 发行版的命令。这可以根据您的项目而变化。

现在，您已经准备好将您的包发布到 conda。执行此命令从项目根目录构建发行版。

```
conda build --output-folder ./conda-out/ ./conda/
```

`conda build`的输入参数应该是你的`meta.yaml`文件所在的目录。

如果您在构建环境中启用了 anaconda upload，这将在成功构建包之后将它上传到 anaconda。如果您禁用了 anaconda upload，那么您可能需要运行下面的命令来将发行版上传到 anaconda(根据需要更新 tar.bz2 包的路径。如果你以前没有从你的命令行登录过 anaconda，那么你必须首先按照这里的[所描述的](https://docs.anaconda.com/anacondaorg/user-guide/getting-started/#building-and-uploading-packages)来设置它。

```
anaconda upload ./conda-out/osx-64/my-py-lib-0.1.0-0.tar.bz2
```

恭喜你！您的包现在应该可以在 Anaconda 中找到了。让我们深入了解一些高级配置选项，在将您的包发布到 conda 时，您可能需要这些选项。

# 为多个 python 版本构建

您的库可能有一些特殊的需求，您需要在运行时使用与构建时相同的 python 版本。让我们假设你使用的是 python 3.8，你需要安装一个你的库的变体，这个库是用同一个 python 版本 3.8 构建的。这可以通过[版本锁定](https://docs.conda.io/projects/conda-build/en/latest/resources/variants.html#general-pinning-examples)来实现。文件`conda_build_config.yaml`将让你轻松做到这一点。

## conda _ build _ 配置. yaml

在`meta.yaml`文件所在的目录下创建一个名为`conda_build_config.yaml`的文件。当除了`meta.yaml`之外还有更多文件时，为 conda 特定文件创建一个单独的目录(在我的例子中是`conda`目录)会很方便。这是我的`conda_build_config.yaml`

```
python_version:
  - 3.7
  - 3.8
  - 3.9
```

我希望我的库分别为 python 3.7、3.8 和 3.9 版本构建。然后你必须在你的`meta.yaml`中引用这些 python 版本。下面是我更新的`meta.yaml`。

```
package:
  name: "my-py-lib"
  version: "0.1.0"about:
  summary: "This is an awesome Python library"

requirements:
  build:
    - python={{ python_version }} run:
    - python={{ python_version }}source:
  path: ..build:
  script: python setup.py install
  string: py{{ python_version | replace(".", "") }}
```

如果您还想固定其他依赖项的版本，那么您也可以将它们包含在`meta.yaml`和`conda_build_config.yaml`中。现在，如果您运行`conda build`命令，您将在输出目录中看到多个构建变体。如果你愿意，你可以通过改变`meta.yaml`中的`build.string`来修改构建变体的命名模式。

# 为多个平台构建

您可能已经注意到，当您在上一节中执行`conda build`时，您的构建工件应该已经创建在一个子目录中，该子目录具有您的构建环境的平台名称(即`osx-64`、`linux-64`)。这意味着在您将这个构建推送到 anaconda 之后，它只能由同一个平台下载。因此，如果你想支持多个平台，那么你必须将这个构建产品转换为其他平台，并再次上传到 anaconda。您可以使用下面的命令轻松做到这一点。我在这里将为`osx-64`建造的人工制品转换为`linux-64`。

```
conda convert --platform linux-64 ./conda-out/osx-64/my-py-lib-0.1.0-0.tar.bz2  -o ./conda-out
```

那你得重新上传这个。

```
anaconda upload ./conda-out/linux-64/my-py-lib-0.1.0-0.tar.bz2
```

你可以在这里找到所有支持的平台。但是，当且仅当您的库是平台相关的时，您必须这样做。如果不是，你可以通过如下更新你的`meta.yaml`的`build`部分，把它构建成一个[架构独立的包](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#architecture-independent-packages)。查看[文档](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#architecture-independent-packages)了解更多选项。

```
**build**:
  **noarch**: python
```

# 在 meta.yaml 中使用变量

在定义您的`meta.yaml`时，可能有很多情况需要定义变量和读取环境变量。感谢`meta.yaml`中 [Jinja2](https://jinja.palletsprojects.com/en/2.11.x/) 的支持，这才有可能。

## 使用环境变量

```
package:
  name: "my-py-lib"
  version: "{{ VERSION }}"
```

`conda build`将在构建环境中寻找`VERSION`环境变量作为版本值。

## 阅读`setup.py`参数。

```
{% set data = load_setup_py_data() %}

package:
  name: "my-py-lib"
  version: {{ data.get('version') }}
```

`conda build`将提取`setup.py`的`version`字段的值作为版本值。

# 支持复杂的构建脚本

如果您有一个复杂的构建脚本，您可以从`meta.yaml`中删除`build.script`部分，并创建一个单独的`build.sh`文件。

## build.sh

在`meta.yaml`文件所在的目录下创建一个`build.sh`文件。在这个文件中编写您的构建步骤。`build.sh`仅适用于类似 Unix 的构建环境。如果您在 Windows 环境下构建，那么您必须创建一个符合 Windows shell 标准的`build.bat`文件。查看 Conda [文档](https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs.html#writing-the-build-script-files-build-sh-and-bld-bat)了解更多信息。

```
#!/usr/bin/env bash$PYTHON setup.py install
```

# 额外注释

*   有关`meta.yaml` - [定义元数据](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#)的完整语法参考
*   关于`conda_build_config.yaml` - [构建变体的更多详细信息](https://docs.conda.io/projects/conda-build/exn/latest/resources/variants.html)
*   如果你正在使用 Github 动作，有许多[预建动作](https://github.com/marketplace?type=actions&query=publish+conda)。