# 如何用 Python 连接到 Postgres 数据库

> 原文：<https://medium.com/analytics-vidhya/how-to-setup-a-python-application-with-a-postgres-database-f965e7c1581e?source=collection_archive---------12----------------------->

![](img/65632f2a494ca1f236c335fad136342e.png)

你好，你好，我的工程师同事们！

这篇文章是对 YouTube 上如何构建可伸缩 Python 应用系列第 3 集和第 4 集的补充(你可以在这里看到第 2 集:【https://youtu.be/FMUsVwcBOVY】T4

我们将讨论如何使用 Flask 和 SQLAlchemy 为您的 Python 应用程序设置 Postgres 数据库连接(在第 2 集创建)。

首先，对于这个项目，我使用 Pipenv，你可以使用任何你喜欢的虚拟环境和依赖管理，但我个人更喜欢使用 Pipenv。

Pipenv 的文档在这里:[https://pipenv.pypa.io/en/latest/](https://pipenv.pypa.io/en/latest/)。

我很久以前在这里写了一篇关于它的博客:[https://medium . com/analytics-vid hya/a-tool-all-python-developers-should-be-using-5d 547 bfb 45 b 7](/analytics-vidhya/a-tool-all-python-developers-should-be-using-5d547bfb45b7)

在 Mac 上安装 Pipenv 最简单的方法是使用自制软件:

```
brew install pipenv
```

接下来，我们的项目将有以下 Pipfile:

```
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]
pytest="*"
pytest_mock="*"

[packages]
clothes-shop = {editable = true, path = "."}
flask="*"
requests="*"
sqlalchemy="*"
flask-sqlalchemy="*"
pg8000="==1.16.5"

[requires]
python_version = "3.9"
```

我们将使用 flask 作为应用程序，SQLAlchemy 作为数据库 ORM(Objected Related Mapping ), pg 8000 作为 PostgreSQL 引擎。请求将在后面的教程中使用(所以现在忽略)。

*文档:*

烧瓶:[https://flask.palletsprojects.com/en/1.1.x/](https://flask.palletsprojects.com/en/1.1.x/)

https://docs.sqlalchemy.org/en/13/

https://pypi.org/project/pg8000/

我们将使用 pg8000，它是 PostgreSQL 数据库引擎的纯 Python 接口。

有趣的事实:它被称为 pg8000，因为它被认为是 Python 的第 8000 个 PostgreSQL 接口。

不幸的是，直到 SLQAlchemy 1.4 才完全支持 pg8000，所以我们不得不将版本固定到 1.16.5(参见本主题的更多内容[https://github.com/sqlalchemy/sqlalchemy/issues/5645](https://github.com/sqlalchemy/sqlalchemy/issues/5645))

这个 pipfile 需要和你的项目在同一层，在我们的例子中，我们创建了一个`clothes_shop`目录来存储我们的`application_setup.py`

我们的项目结构将如下所示:

```
ClothesShop/
├── clothes_shop/
│   ├── test/
│   └── application_setup.py
│
├── README.md
├── setup.py
├── Pipfile
└── Pipfile.lock
```

在安装我们的依赖项之前，我们还需要创建一个`setup.py`,您可以创建一个`README.md`,现在让它为空:

```
"""setup.py"""from setuptools import setup

with open('README.md', encoding="utf-8") as readme_file:
    readme = readme_file.read()

setup(
    name='clothes_shop',
    description='Clothes shop project for YouTube',
    author='Alexander V',
    author_email='alex.v.engineering@gmail.com',
    long_description=readme,
    test_suite='clothes_shop/test/'
)
```

如果您想使用 pipenv 安装这些依赖项:

```
pipenv install -d
```

-d 包括开发包。您会注意到还创建了一个`pipfile.lock`文件。

如果你按照上一集的内容设置了 Postgres。您可以通过执行以下操作来添加密码:

首先，通过在您的终端中键入以下命令来启动 psql:

```
psql
```

然后更改您的用户:

```
ALTER USER <your username> WITH PASSWORD '<your password>';
```

您还需要修改本地配置来请求密码。

通过在 psql 中输入以下内容找到您的`hba_file`:

```
SHOW hba_file;
```

这应该会返回类似于`/usr/local/var/postgres/pg_hba.conf`的内容

你可以随意修改。我只是使用 VIM:

```
vim  /usr/local/var/postgres/pg_hba.conf
```

在文件的底部，将配置更改为如下所示:(在 VIM 中，按`a`进入，然后修改文本。然后按下`esc`，然后键入`:wq`和`return`保存您的更改并退出)。

![](img/0751cfbef6e3f61e2cfd5608d39975d4.png)

这将要求所有用户输入密码才能登录。

现在让我们设置我们的应用程序:

```
*"""Application setup"""* import os

from flask import Flask
from flask_sqlalchemy import SQLAlchemy

def application_setup():
    *"""creates a flask application and initialises the clothes_shop database"""* username = os.environ.get("PG_USERNAME", "alex")
    password = os.environ.get("PG_PASSWORD", "test")
    database_name = os.environ.get("DATABASE_NAME", "clothes_shop")

    _app = Flask(__name__)
    _app.config["SQLALCHEMY_DATABASE_URI"] = f"postgresql+pg8000://{username}:{password}@localhost:5432/{database_name}"
    _app.config["SQLALCHEMY_BINDS"] = {f"{database_name}": _app.config["SQLALCHEMY_DATABASE_URI"]}
    _app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

    db = SQLAlchemy(_app)
    db.init_app(_app)

    return _app, db
```

*在此功能中:*

*   我们从环境变量中获取 Postgres 用户名、密码和数据库名。
*   我们初始化一个烧瓶应用程序。
*   我们设置数据库配置(URI、绑定和跟踪修改)
*   然后初始化我们的数据库和应用程序。
*   最后，我们返回主程序使用的应用程序和数据库。

## 测试测试测试

在本系列中，我们将从软件工程的角度构建应用程序。任何有价值的软件都将被很好地测试(…这个类比不确定)。这个项目也不会有什么不同。

我喜欢向您展示软件开发中“不太有趣”但却最重要的一面。对你的代码进行严格的测试……当你的 QA 团队发现 bug 时，这会节省你很多时间。

使用`Pytest`,我们将测试应用程序与数据库的连接。这就是我们所说的集成测试，因为我们将在系统中测试我们的两个服务交互。

在您的测试文件夹中设置一个名为`Integration`的目录，并在该目录中添加一个名为`test_application_setup.py`的 Python 文件，这样您的文件夹结构如下所示:

```
test/
└── integration/
    └── test_application_setup.py
```

首先我们将测试`test_application_setup.`内部的快乐路径添加:

```
*"""test_application_setup.py Test the integration on the application setup and interaction with the database"""* 
from clothes_shop import application_setup

def test_application_setup():
    *"""Check the database connection is active with correct details"""* os.environ["DATABASE_NAME"] = "clothes_shop"
    table_name = "clothes_stock"

    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    assert db.session.execute(f"SELECT * from {table_name}")
```

我喜欢把我的测试分成三个部分。

*   设置(测试变量、环境变量等)。
*   执行要测试的功能。
*   断言

在这个测试中，我们将数据库名称添加到一个环境变量中。使用`os`我们可以设置我们的环境变量。我们还为我们想要对其执行 SQL `SELECT`的表名设置了一个变量。

然后我们运行函数`application_setup`，它将返回应用程序和数据库。

最后，我们检查从数据库配置返回的绑定是否与应用程序配置相同。然后最重要的是，我们测试我们的连接是否从数据库返回结果。

我们只需从第 2 集创建的数据库中执行一条 SQL `SELECT`语句就可以做到这一点。如果您还记得，我们向一个名为`clothes_stock`的表中插入了一些数据。应该返回这些数据。如果没有，将从 SQLAlchemy 返回一个`ProgrammingError`。

请记住，对于这个测试，我们只关心正在建立的连接。而不是返回的数据内容。

现在让我们做一些不愉快的路径！

永远不要只测试从系统中返回您想要的结果的路径。软件工程师需要考虑我们不应该期望的结果，以及边缘情况场景。

对于这个项目，我们不希望任何不正确的密码能够访问超级用户的特权。我们要做的下一个测试是测试这个:

```
*"""test_application_setup.py Test the integration on the application setup and interaction with the database"""* import os

import pytest
from sqlalchemy.exc import ProgrammingError

from clothes_shop import application_setup

def test_application_setup_incorrect_password():
    *"""Check the database connection does not allow incorrect passwords"""* os.environ["PG_USERNAME"] = "alex"
    os.environ["PG_PASSWORD"] = "incorrect_password"
    os.environ["DATABASE_NAME"] = "clothes_shop"
    table_name = "clothes_stock"

    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    with pytest.raises(ProgrammingError):
        assert db.session.execute(f"SELECT * from {table_name}")
```

在上面的例子中，我们设置了一个错误的密码。然后使用`Pytest.raises`我们可以发现使用该密码将引发一个`ProgrammingError`

最后，我们还想测试无效用户将无法访问我们的数据库:

```
*"""test_application_setup.py Test the integration on the application setup and interaction with the database"""* import os

import pytest
from sqlalchemy.exc import ProgrammingError

from clothes_shop import application_setup def test_application_setup_invalid_user():
    *"""Check the database connection does not allow an invalid username"""* os.environ["PG_USERNAME"] = "invalid_username"
    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    with pytest.raises(ProgrammingError):
        assert db.session.execute("SELECT * from clothes_stock")
```

在这个例子中，我们设置了一个随机的用户名。我们使用与之前测试相同的技术来捕捉错误，因为这也应该产生一个`ProgrammingError`

完整的测试套件应该如下所示:

```
*"""Test the integration on the application setup and interaction with the database"""* import os

import pytest
from sqlalchemy.exc import ProgrammingError

from clothes_shop import application_setup

def test_application_setup():
    *"""Check the database connection is active with correct details"""* os.environ["DATABASE_NAME"] = "clothes_shop"
    table_name = "clothes_stock"

    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    assert db.session.execute(f"SELECT * from {table_name}")

def test_application_setup_incorrect_password():
    *"""Check the database connection does not allow incorrect passwords"""* os.environ["PG_USERNAME"] = "alex"
    os.environ["PG_PASSWORD"] = "incorrect_password"
    os.environ["DATABASE_NAME"] = "clothes_shop"
    table_name = "clothes_stock"

    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    with pytest.raises(ProgrammingError):
        assert db.session.execute(f"SELECT * from {table_name}")

def test_application_setup_invalid_user():
    *"""Check the database connection does not allow an invalid username"""* os.environ["PG_USERNAME"] = "invalid_username"
    os.environ["DATABASE_NAME"] = "clothes_shop"
    table_name = "clothes_stock"

    app, db = application_setup.application_setup()

    assert app.config["SQLALCHEMY_DATABASE_URI"] == str(db.session.bind.url)
    with pytest.raises(ProgrammingError):
        assert db.session.execute(f"SELECT * from {table_name}")
```

然后，您可以使用以下命令运行这些测试:

```
pipenv run pytest
```

您应该看到三个通过测试！

我希望这能对你有所帮助。第 3 集和第 4 集的视频很快就会出来。如果您有任何问题，请随意评论，我会在下一个问题上与您联系。