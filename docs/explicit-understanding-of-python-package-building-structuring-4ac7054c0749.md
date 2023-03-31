# 对 python 包构建(结构化)的明确理解——第 2 部分

> 原文：<https://medium.com/analytics-vidhya/explicit-understanding-of-python-package-building-structuring-4ac7054c0749?source=collection_archive---------1----------------------->

![](img/b716e0d74a9583aadc98f54883dc4330.png)

威利·芬伯格在 [Unsplash](https://unsplash.com/s/photos/architecture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

这是构建 python 包系列文章的第二部分。在前一篇文章中，我们讨论了如何对你的代码库和文档的概念进行版本控制。在本文中，我们将讨论如何将该代码转换成一个实际的包，该包的工作方式类似于您使用的常规 python 库。

有很多方法可以把你的代码转换成一个包。构建包和创建包结构是完全不同的事情。因为你必须使用打包工具将你的代码转换成一个包，但是正确的做法是代码库的形状与构建工具相兼容。

首先，为什么会有几种封装架构？没有通用的吗？。事实上，不是。原因是架构依赖于您正在构建的工件的范围和包的使用。因此，有一些通用的布局模式:

> ***命令行应用布局***

这种类型的应用程序可以下载和安装，然后将代码作为外部模块导入，或者直接在终端中运行。

*   *python 模块布局*
*   *python 包布局*
*   *python 库布局*

附注:以上布局/架构的名称并不标准，但都是常用的术语。特别是，`setuptools`库支持轻松创建以上 3 种结构。

```
package_folder/
│
├── .gitignore
├── src/
├── logs/
├── docs/
├── bin/
├── data/
├── LICENSE
├── README.md
├── requirements.txt
├── test/
└── setup.py
```

这是任何 python 包的最小布局。在你建立模型之后，有另外的文件夹。如；`dist/`、`build/`。

*。gitignore* —正如我们在上一篇文章中讨论的，我们的版本控制系统是 git，我们使用 GitHub 作为存储库控制器。因此，有些文件我们不想上传到远程存储库。喜欢；日志、bin、数据、分布、构建。这里我们用。gitignore 文件来丢失这些文件的 git 跟踪。大多数语言都有这个文件的[模板](https://www.toptal.com/developers/gitignore)，你可以根据自己的喜好在那里添加或删除行。还有编辑这些文件的规则和语法。

*src/*-这是包含 python 包代码的文件夹。此文件夹的结构因您选择的应用程序布局而异。

*logs/* —日志文件保存在该文件夹中。

*LICENSE* —该文件显示发行商为用户提供的软件包和设施以及限制的许可。使用知名许可比使用自创许可更好。喜欢； [GNU](https://www.gnu.org/licenses/gpl-3.0.en.html) ， [Apache 许可](https://www.apache.org/licenses/LICENSE-2.0)， [MIT 许可](https://opensource.org/licenses/MIT)，[知识共享许可](https://creativecommons.org/choose/)。

*readme . MD*——通常，这是一个 [markdown](https://www.markdownguide.org/getting-started/) 文件【因为它易于编辑并得到广泛支持。]加上...的扩展名。md 或 [LaTeX](https://www.latex-project.org/) 文件【python 创建的纯文本文件】，扩展名为. TeX，这是我们在上一篇文章中讨论过的 docs 文件。

*requirements.txt* —这里，是你的包依赖项。如果您熟悉 python 虚拟环境[pip，conda]，您会记得从 CLI 生成该文件的一个命令。`pip freeze > requirements.txt`。然而，在这种情况下使用它并不是好的做法。因为该命令向需求文件添加了次要依赖项。通常，下面的方法是检查`src/` dir 文件并手动添加这些文件中的所有导入库。与库一样，unittest、tox 也不能包含在该文件中。因为这些库只供开发人员使用。]

*test/*——如果你已经为你的包编写了单元测试，那么这些文件就放在这个文件夹下。

*setup.py* —这是文件夹目录中最关键的文件。因为这个文件说，所有关于包的所有细节都给了包构建者。创建这个文件有几种可接受的开发实践。此外，文件的某些部分会根据您的应用程序布局而变化。

如果这个包是一个复杂的包，那么您也将创建以下目录。

*docs/——正如我们在上一篇文章中讨论的，这个文件夹是用于库的外部综合文档。通常，该文件夹包含。md，。HTML，。css 文件。这是 Github* [*页面上你的页面主机*](https://pages.github.com/)*[*MkDocs、*](https://www.mkdocs.org/) *或*[*readthedocs*](https://readthedocs.org/)*站点**

*bin/——这里是您在包实现中使用的所有可执行文件。如果你的包是纯 python 的，这里没什么可放的。但是如果你使用了一些 C 或 C++代码，那么它们的可执行文件必须保存在这里。*

**data/——如果你已经使用了一些文本文件来保存变量或参数或数据，那么这些文件就放在这个文件夹下**

> ***python 模块布局***

*在 python 导入中，在外部文件中编写的代码会导致将导入的文件命名为当前名称空间中的“模块”。**这个词命名空间**有点特殊。**名称空间**在 python 这样的语言中起着重要作用，因为它们使用词法作用域作为作用域规则，函数是一级对象。作为总结，当你键入`from file_name import function`时，函数克隆到当前工作变量空间，而不是在原始变量空间上处理。*

*或多或少，这里我们关心的是将 python 包创建为一个模块。*

```
*pgk_name/
│
├── src/
│   ├── module_1.py
│   └── module_2.py
│
├── tests/
│   ├── module_1_test.py
│   └── module_2_test.py
│
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
└── setup.py*
```

*如前所述，主要有`src/`和`setup.py`的变化。*

*`src/`目录下的文件是包实现的代码。在由`if __name__ == "__main__" :`定义的 if 条件块下，将所有本地操作添加到像`module_1.py`这样的文件中是一个很好的做法。事实上，这个块下的所有代码只在文件本身执行时才运行。所以，这防止了在词法克隆功能时运行一些代码。*

*但是，包的行为方式也取决于 setup.py 文件的配置。*

```
*package_dir={'': 'src'},py_modules = ["module_1", "module_2"],*
```

*当您使用`setuptools`作为打包模块时，这是最简单的方法。*

> ***python 包布局***

*实际上，包在结构和行为上与 python 模块略有不同。你可以在文件的顶部说`import numpy`，在那之后的任何地方，你可以通过使用像`numpy.array()`这样的点操作来访问任何功能。模块中不存在功能克隆。*

*如果你熟悉 OOP 概念，这和 OOP 中的打包完全一样，唯一的区别是这个包是可移植的。*

*如果您使用的是像 pycharm、eclipse — pydev 这样的 IDE，您只需点击几下鼠标就可以为包创建一个文件夹结构。然而，这很简单。*

```
*project_name/
│
├── pkg_name/
│   ├── __init__.py
│   ├── file_1.py
│   └── internal_pck/
│       ├── __init__.py
│       ├── int_file_1.py
│       └── int_file_2.py
│
├── tests/
│   └── pkg_name_test
│       ├── file_1_tests.py
│       └── internalpkg_name
│          ├── int_file_1_tests.py
│          └── int_file_2_tests.py
│
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
└── setup.py*
```

*主要的区别是没有`src/`文件夹，有一个文件夹有你的包的名字，并且总是以`__init__.py`文件开始。这是为什么呢？实际上，在这里你是在创造一个具有可以与外部世界互动的界面的机器。此外，您可以通过在`pkg_name/`目录而不是`__init__.py`文件中写入文件来定义机器做什么，它们如何做这些。那个文件是定义这个包的接口，说什么功能可以在使用这个包的时候被一个外部文件访问。*

```
*from .file_1 import function_1
from .internal_pkg import function_2
import logginglogger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)__all__ = [
    'function_1',
    'function_2'
]*
```

*这个例子有点复杂，因为包里面有一个包。创建封装内部包的`pkg`接口是至关重要的，否则这个例子将在我们的下一个主题中讨论。*

*不仅`src/`文件夹中的更改，而且`setup.py`也必须相应更改。*

```
*packages=['pkg_name','pkg_name/internal_pck'],*
```

*在构建*轮子或 egg 文件时，setuptool 会识别包中的所有文件，并将它们导入到最终构建中。*如果没有内部包，只需定义包名就足够了。如果有内部包，并且主包使用那些内部包提供的一些功能，那么定义那些包目录是必不可少的。*

> ***python 库布局***

*这是 python 打包的扩展版本。因为这里的 python 库包含了不止一个 python 包，这些包[大部分]都是相互独立工作的，它们有自己的接口与外部接口一起工作。这种 python 库的一个很好的例子是`scikit-learn`，因为`scikit-learn`库包含像`Regression, Classification, Preprcess, Ensemble, etc.`这样的包*

*文件夹结构的定义主要有两种不同的方式，但`setup.py`文件的配置与上述相同。*

```
*project_name/
│
├── lib_name/
│   ├── __init__.py
│   ├── file_1.py
│   ├── pkg_1/
│   │   ├── __init__.py
│   │   ├── file_1.py
│   │   └── file_2.py
│   │
│   └── pkg_2/
│       ├── __init__.py
│       ├── file_3.py
│       └── file_4.py
│
├── tests/
│
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
└── setup.py*
```

*与上面的例子不同的是，这里你从主库接口引用内部包接口[`__init__.py`]。因此，在运行时，用户可以通过使用点操作来访问内部函数。然而，定义`test/`比上面的结构更复杂。*

```
*project_name/
│
├── pkg_1/
│   ├── __init__.py
│   ├── module_1.py
│   └── module_2.py
├── pkg_2/
│   ├── __init__.py
│   ├── module_1.py
│   └── module_2.py
│
├── tests/
│
├── .gitignore
├── LICENSE
├── README.md
├── requirements.txt
└── setup.py*
```

*这是一种在一个名字下提供多个包的方式。所以，这里用户感觉有不同的包，但是开发者包测试比上面的方法更容易。然而，构建 python 库的最正式的方法是先定义。*

*链接:*

*[包装用 PyPA 导轨](https://packaging.python.org/tutorials/packaging-projects/)*

*[命名空间包&常规包](https://docs.python.org/3/reference/import.html#regular-packages)的区别*

*[PEP 420 —名称空间打包指南](https://www.python.org/dev/peps/pep-0420/)*

*[](https://realpython.com/lessons/preparing-your-package-publication/) [## 准备在 PyPI - Real Python 上发布您的包

### 现在包的代码部分已经准备好上传了，但是在你可以分享你的…

realpython.com](https://realpython.com/lessons/preparing-your-package-publication/)* 

*我们只是把我在上一篇文章中提到的一个话题转换到了我们要讨论的内容之下。因此，摆在我们面前的是一系列议题:*

*   *[文件和版本控制](/analytics-vidhya/explicit-understanding-of-python-package-building-coding-93fa72c7cb95) ✅*
*   *[包装架构](/analytics-vidhya/explicit-understanding-of-python-package-building-structuring-4ac7054c0749) ✅*
*   *python 装饰者*
*   *python 生成器*
*   *python 上下文管理器*
*   *面向对象的设计模式用法*
*   *包测试——单元测试——没有模仿/模仿*
*   *异常处理和调试*
*   *CI/CD 管道建筑*
*   *自动化 CI/CD 管道*
*   *未来兼容性*

*不久我们将看到 pythonic 的另一篇文章！！。*

**谢谢。**