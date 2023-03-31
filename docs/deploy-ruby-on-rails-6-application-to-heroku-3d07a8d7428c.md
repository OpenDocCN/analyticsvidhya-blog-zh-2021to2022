# 将 Ruby on Rails 6 应用程序部署到 Heroku

> 原文：<https://medium.com/analytics-vidhya/deploy-ruby-on-rails-6-application-to-heroku-3d07a8d7428c?source=collection_archive---------7----------------------->

在这里，您将了解如何将现有的应用程序部署到 Heroku。

# **设置您的本地:**

首先创建一个 heroku 帐户，它是免费的。

在您的开发机器上安装 [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) 。

一旦安装完毕，`heroku`命令可从您的终端使用。使用您的 Heroku 帐户的电子邮件地址和密码登录:

![](img/eecf8370b9b47b5b808340500b6c8ef3.png)

在提示符下按 Enter 键，上传您现有的`ssh`键或创建一个新的键，用于稍后推送代码。—如果你需要

它将在您的浏览器上打开一个新标签，输入您的 Heroku 帐户凭证并登录，然后您将成功登录。

## 数据库. yml

确保您的`config/database.yml`文件正在使用`postgresql`适配器。您的`config/database.yml`文件的开发部分应该是这样的:

```
$cat config/database.yml
# PostgreSQL. Versions 9.3 and up are supported.
#
# Install the pg driver:
#   gem install pg
# On macOS with Homebrew:
#   gem install pg -- --with-pg-config=/usr/local/bin/pg_config
# On macOS with MacPorts:
#   gem install pg -- --with-pg-config=/opt/local/lib/postgresql84/bin/pg_config
# On Windows:
#   gem install pg
#       Choose the win32 build.
#       Install PostgreSQL and put its /bin directory on your path.
#
# Configure Using Gemfile
# gem 'pg'
#
default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: myapp_development

  # The specified database role being used to connect to postgres.
  # To create additional roles in postgres see `$ createuser --help`.
  # When left blank, postgres will use the default role. This is
  # the same name as the operating system user running Rails.
  #username: myapp

  # The password associated with the postgres role (username).
  #password:

  # Connect on a TCP socket. Omitted by default since the client uses a
  # domain socket that doesn't need configuration. Windows does not have
  # domain sockets, so uncomment these lines.
  #host: localhost

  # The TCP port the server listens on. Defaults to 5432.
  # If your server runs on a different port number, change accordingly.
  #port: 5432

  # Schema search path. The server defaults to $user,public
  #schema_search_path: myapp,sharedapp,public

  # Minimum log levels, in increasing order:
  #   debug5, debug4, debug3, debug2, debug1,
  #   log, notice, warning, error, fatal, and panic
  # Defaults to warning.
  #min_messages: notice

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  database: myapp_test

# As with config/credentials.yml, you never want to store sensitive information,
# like your database password, in your source code. If your source code is
# ever seen by anyone, they now have access to your database.
#
# Instead, provide the password or a full connection URL as an environment
# variable when you boot the app. For example:
#
#   DATABASE_URL="postgres://myuser:mypass@localhost/somedatabase"
#
# If the connection URL is provided in the special DATABASE_URL environment
# variable, Rails will automatically merge its configuration values on top of
# the values provided in this file. Alternatively, you can specify a connection
# URL environment variable explicitly:
#
#   production:
#     url: <%= ENV['MY_APP_DATABASE_URL'] %>
#
# Read https://guides.rubyonrails.org/configuring.html#configuring-a-database
# for a full overview on how database connection configuration can be specified.
#
production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

为了让数据库连接在生产环境中工作，Heroku 使用了自己的策略。

## 指定您的 Ruby 版本

Rails 6 需要 Ruby 2.5.0 或以上版本。Heroku 默认安装了最新版本的 Ruby，但是您可以通过使用您的`Gemfile`中的`ruby` DSL 来指定一个确切的版本。根据您当前运行的 Ruby 版本，它可能如下所示:

```
ruby "2.7.2"
```

## 推送至 git:

现在，在您的 Rails 应用程序目录中运行这些命令，初始化您的代码并提交给 Git:

```
$ git init
$ git add .
$ git commit -m "init"
```

您可以通过运行以下命令来验证所有内容都已正确提交:

```
$ git status
On branch main
nothing to commit, working tree clean
```

现在您的应用程序已经提交给 Git，您可以部署到 Heroku。

## 部署到 Heroku:

确保您位于包含您的 Rails 应用程序的目录中，然后在 Heroku 上创建一个应用程序:

```
$ heroku create
Creating app... done, aqueous-sierra-44713
https://ashokavanam-44713.herokuapp.com/ | [https://git.heroku.com/](https://git.heroku.com/aqueous-sierra-44713.git)ashokavanam[-44713.git](https://git.heroku.com/aqueous-sierra-44713.git)
```

您可以通过运行以下命令来验证遥控器是否已添加到您的项目中:

```
$ git config --list --local | grep heroku
remote.heroku.url=https://git.heroku.com/ashokavanam-44713.git
remote.heroku.fetch=+refs/heads/*:refs/remotes/heroku/*
```

## 部署您的代码:

```
$ git push heroku master
```

> 如果您的 git repo 使用 main 作为头分支，那么用上面命令中的 main 替换 master

## 迁移您的数据库

如果您在应用程序中使用数据库，则需要通过运行以下命令来手动迁移数据库:

```
$ heroku run rake db:migrate
```

你可以检查应用程序动态的状态。`heroku ps`命令列出了您的应用程序正在运行的 dynos:

就是这样！！！

我们现在可以使用`heroku open`在浏览器中访问应用程序。

```
$ heroku open
```

编码快乐！！！！！:)