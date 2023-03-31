# Python 打包演练(带解释)

> 原文：<https://medium.com/analytics-vidhya/python-packaging-walk-through-with-explanations-76ebd78a25e9?source=collection_archive---------18----------------------->

一个基于我自己的打包 gitcrawl 之旅的小教程。

[Gitcrawl](https://pypi.org/project/gitcrawl/) 是我最近写的一个工具，花了一些时间计算出正确的参数。因此，我决定写这个小的走查，向我未来的自己、我的同事和任何可能会觉得有用的人解释一下:)

如果你只是想知道它是如何完成的，直接跳到**构建包**

## 一个包裹包含什么？

*   模块的集合
*   文档
*   任何顶级脚本
*   任何需要的附加数据文件
*   构建和安装说明

指出结构的`tree ../gitcrawl`的简化输出:

```
../gitcrawl
├── LICENSE
├── README.md
├
├── docs
│   │...<docs build with sphinx>
│
├── gitcrawl
│   ├── __init__.py
│   ├── core_functions.py
│   ├── docs
│   │   ├── ... < docs build with sphinx>
│   ├── ... <the actual code>
│   └── tests
│       └── <contains test code>
├
├── gitcrawl-env.yml
├── imgs
│   └── <images for the docs/README.md>
├── requirements.txt
├── setup.py
```

`LICENSE`:多种多样，每个项目都应该包含一个。在这里，你可以得到帮助，为你的项目选择合适的许可证。这个项目在这里使用[麻省理工学院许可](https://snyk.io/learn/what-is-mit-license/)

`README.md`:应包含包及其用途的简要概述。格式是降价。

`docs/`:文档。在这种情况下，我决定使用[狮身人面像](https://www.sphinx-doc.org/en/master/)。这也是这个包包含两个`docs/`目录的原因。(带有 sphinx 的文档将在以后的教程中介绍)

`gitcrawl/`:主包——这是代码要去的地方。

`tests/` : Testcode，确保代码的功能性。更多信息见 [pytest](https://docs.pytest.org/en/stable/unittest.html)

`gitcrawl-env.yml`:可选，如果您和您的目标受众使用 [conda](https://docs.conda.io/en/latest/) 则相关

`imgs/`:可选，如果您想在 README.md 或文档中包含图像文件，请选择相关

`requirements.txt`:可选，如果您和您的目标受众使用 [pip](https://realpython.com/what-is-pip/) 则相关

`setup.py`:包含构建和安装包的所有必要信息的`setuptools`的构建脚本。详见**创建 setup.py** 部分。

`MANIFEST.in`:可选，描述该项目要包括哪些非代码文件——尚未包括的，见[此处](https://packaging.python.org/guides/using-manifest-in/)

# 构建包

确保`setuptools`安装在您的工作环境中。然后创建一个应该包含类似内容的`setup.py`。(适合您的套餐和需求。)

```
import setuptools

with open("README.md", "r") as fh:
   long_description = fh.read()

setuptools.setup(
   name="gitcrawl",
   version="0.1.2",
   author="Neda Sultova",
   author_email="[my.email@address.com](mailto:my.email@address.com)",
   description="A small package for creating os-agnostic conda environment.yml files",
   long_description=long_description,
   long_description_content_type="text/markdown",
   url="https://github.com/nsultova/gitcrawl",
   packages=setuptools.find_packages(),
   package_data={"gitcrawl": ["python_module_index.pickle", "module-names.txt"]},
   install_requires=[
       "beautifulsoup4",
       "tqdm",
       "requests",
       "PyYAML",
       "requirements-parser",
       "pip-search",
       "findimports"
   ],
   entry_points={
       'console_scripts':[
           'gitcrawl=gitcrawl:main'
       ]
   },
   classifiers=[
       "Programming Language :: Python :: 3",
       "License :: OSI Approved :: MIT License",
       "Operating System :: OS Independent",
   ],
   python_requires='>=3.6',
)
```

**解释**

`setup()`需要几个参数:

`name`是您的软件包的发行名称。请务必用您的用户名更新此信息，以避免名称冲突。

`version`是否是封装版本详见 [PEP 440](https://www.python.org/dev/peps/pep-0440) 。

`author`和`author_email` 用于标识包的作者。

`description`是对产品包的简短总结，只有一句话。

`long_description`是包的详细描述。在这种情况下，详细描述是从 README.md 加载的。

`url`是项目主页的 URL。对于许多项目来说，这只是一个到 GitHub，GitLab 等的链接。

`packages`是发行包中应该包含的所有 Python 导入包的列表。这里 find_packages()用于自动发现所有的包和子包。

`package_data`:可选，在这种情况下，它包含软件包运行所需的两个附加文件。

`install_requires`是一个 setuptools setup.py 关键字，应该用来指定一个项目正确运行至少需要什么。当 pip 安装项目时，这是用于安装其依赖项的规范。查看[此处](https://packaging.python.org/discussions/install-requires-vs-requirements/)了解更多信息。

`entry_points`:可选，允许软件包为要执行的软件包安装程序定义一个用户友好的名称

`classifiers`给 index 和 pip 一些关于你的包的附加元数据。更多信息见[此处](https://pypi.org/classifiers/)。

## 构建分发包

下一步是为包生成分发包，以便以后上传。

确保你已经安装了`setuptools`、`wheel`和`twine`，如果没有，执行:`python3 -m pip install --user --upgrade setuptools wheel twine`

如果你正在构建一个新版本的软件包，打开`setup.py`并相应地更改版本号

在`setup.py`所在的同一个目录下，执行:`python3 setup.py sdist bdist_wheel`

该命令输出大量文本，并在(也是新创建的)`dist`目录中生成以下文件:

```
dist/
   gitcrawl-0.1.0-py3-none-any.whl
   gitcrawl-0.1.0.tar.gz
```

构建后执行检查:`twine check dist/*`

`tar.gz`文件是一个源文件，而`.whl`文件是一个构建的发行版。较新的 pip 版本优先安装已构建的发行版，但是如果需要的话，会退回到源文件。

## 上传包

现在新出炉的包可以上传到 [Python 包索引](https://pypi.org)

首先你可以上传你的包到 [Test PyPI](https://test.pypi.org) 并测试是否一切正常。更多信息见[此处](https://packaging.python.org/guides/using-testpypi/)

在这里注册一个关于测试 PyPI [的账户，在这里](https://test.pypi.org/account/register/)注册一个关于 PyPI [的账户，然后按照说明操作。](https://pypi.org/)

接下来的步骤与测试 PyPi 和 PyPI 类似。

**创建 PyPI API 令牌**

转到[这里](https://test.pypi.org/manage/account/#api-tokens)并创建一个新的 API 令牌，不要将其范围限制到一个特定的项目，因为您正在创建一个新项目。

在您复制并保存令牌之前，请不要关闭页面—您将不会再次看到该令牌。

运行 twine 上传`dist/`下的所有档案:

`twine upload --repository GITCRAWL dist/*`

系统将提示您输入用户名和密码。对于用户名，使用`__token__`。对于密码，使用令牌值，包括前缀`pypi-`。

命令完成后，您应该会看到类似如下的输出:

```
Uploading distributions to https://upload.pypi.org/legacy
Enter your username: __token__
Enter your password: <tokenval comes here>
Uploading gitcrawl-0.1.0-py3-none-any.whl
100%|█████████████████████| 4.65k/4.65k [00:01<00:00, 2.88kB/s]
Uploading gitcrawl-0.1.2.tar.gz
100%|█████████████████████| 4.25k/4.25k [00:01<00:00, 3.05kB/s]
```

一旦上传，你的包应该可以在 PyPi 上看到，就像[这里](https://pypi.org/project/gitcrawl/)

## 安装您的软件包

您现在可以使用 pip 来安装您的软件包并验证它是否工作。:)

最后的结果。执行完所有内容后，执行`tree gitcrawl`会显示包结构

```
../gitcrawl
├── LICENSE
├── README.md
├── build
│   ├── bdist.macosx-10.9-x86_64
│   └── lib
│       └── gitcrawl
│           ├── __init__.py
│           ├── core_functions.py
│          ...
│           ├── module-names.txt
│           └── python_module_index.pickle
├── dist
│   ├── gitcrawl-0.1.0-py3-none-any.whl
│ ...
│   └── gitcrawl-0.1.2.tar.gz
├── docs
│   ├── ...
│   ...
├── gitcrawl
│   ├── __init__.py
│   ├── __pycache__
│   │   ...
│   ├── docs
│   │   ...
│   ├── gitcrawl.py
│   ├── ...
│   └── tests
│       └── ...
├── gitcrawl-env.yml
├── gitcrawl.egg-info
│   ├── ...
├── imgs
│   └── workflow.png
├── requirements.txt
├── setup.py
```

## 资源:

与此同时，官方文档已经更新得更好了，所以一定要检查他们了！

 [## 打包 Python 项目——Python 打包用户指南

### 本教程将带您了解如何打包一个简单的 Python 项目。它将向您展示如何添加必要的文件…

packaging.python.org](https://packaging.python.org/tutorials/packaging-projects/) 

*原载于【http://github.com】[](https://gist.github.com/3292e90193d762357c7cbf05298540cc)**。***