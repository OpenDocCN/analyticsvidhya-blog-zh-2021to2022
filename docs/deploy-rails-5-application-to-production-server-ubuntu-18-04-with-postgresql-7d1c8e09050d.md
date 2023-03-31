# 使用 PostgreSQL 将 rails 5 应用程序部署到生产服务器 Ubuntu 18.04

> 原文：<https://medium.com/analytics-vidhya/deploy-rails-5-application-to-production-server-ubuntu-18-04-with-postgresql-7d1c8e09050d?source=collection_archive---------9----------------------->

在这里，您将了解如何将现有的 rails 应用程序部署到生产环境中。

用于将 rails 应用程序部署到 Heroku checkout 我的另一个故事[https://medium . com/analytics-vid hya/deploy-ruby-on-rails-6-application-to-Heroku-3d 07 a8d 7428 c](/analytics-vidhya/deploy-ruby-on-rails-6-application-to-heroku-3d07a8d7428c)

![](img/8cae682cbdf38448359f9b99f64e29b2.png)

在您从像 **AWS** 、 **digital ocean** 、 **Vultr 这样的提供商那里获得创建的实例之后..**

**初始设置:**

通过以下命令 SSH 到您的实例

```
$ ssh root@<ubuntu-server-ip-address>
```

成功登录后。

我们现在可以为我们的应用程序部署创建一个部署用户。因为，避免使用`root`用户在生产服务器上进行部署是一个很好的实践。

```
$ root@testserver:~# adduser deploy
```

填写用户`deploy`的所有信息:

```
Adding user `deploy' ...
Adding new group `deploy' (1000) ...
Adding new user `deploy' (1000) with group `deploy' ...
Creating home directory `/home/deploy' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for deploy
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] YThe deploy user is now ready with regular account privileges. However, deploy user will be used for deploying your applications and should belong to the sudoers of the Ubuntu server.
```

通过以下方式更新`deploy`用户权限:

```
$ root@testserver:~# usermod -aG sudo deploy
```

从现在开始，`deploy`拥有超级用户权限，并且是 sudoers **的一员。现在，让我们为`deploy`用户添加公钥认证。**

在您的**本地** ubuntu 开发机器的控制台上运行:

```
$ ssh-copy-id deploy@<ubuntu-server-ip-address>/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
deploy@'s password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'deploy@'"
and check to make sure that only the key(s) you wanted were added.

Your local ubuntu development machine’s public key has been associated with deploy user, and from now on you can be authenticated using this key and not your password.
```

通过以下方式测试配置:

```
$ ssh deploy@<ubuntu-server-ip-address>Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-45-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Feb 23 21:06:03 UTC 2019

  System load:  0.0               Processes:           83
  Usage of /:   4.0% of 24.06GB   Users logged in:     0
  Memory usage: 11%               IP address for eth0: 157.230.58.214
  Swap usage:   0%

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

To run a command as administrator (user "root"), use "sudo".
See "man sudo_root" for details.

deploy@testserver:~$
```

成功！！！

![](img/e81752cc28137cc93eda572b62be8229.png)

照片由[布鲁斯·马尔斯](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

您可以看到您已经**连接到您的 Ubuntu 服务器，并且仅使用您的公共 SSH 密钥进行认证**。

下一步是**禁用密码验证**。为此，在服务器上运行以下命令:

```
deploy@testserver:~$ sudo nano /etc/ssh/sshd_config
```

找到指定`PasswordAuthentication`的行，如果它被注释掉了，通过删除行首的`#`来取消注释。将数值从`yes`更改为`no`。

更改后应该是这样的:

```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
#PermitEmptyPasswords no
```

还要确保`PubkeyAuthentication`未被注释，并且其值被设置为`yes`

```
PubkeyAuthentication yes
```

如果一切都按规定配置，通过按键盘上的`CTRL-X`，然后`Y`和`ENTER`保存并关闭文件。

最后一步是在使用 sudo 命令时禁用部署用户的密码提示。为此，请在您的 Ubuntu 服务器上键入:

```
deploy@testserver:~$ sudo visudo
```

在文件末尾添加以下内容:

```
# Deploy
deploy ALL=(ALL) NOPASSWD:ALL
```

再次通过`CTRL-X`，然后`Y`和`ENTER`保存并关闭文件。最后，让我们试试配置。

通过以下方式从 Ubuntu 服务器注销:

```
deploy@testserver:~$ exit
```

并使用 ssh 连接:

```
$ ssh deploy@<ubuntu-server-ip-address>
```

尝试 sudo 命令来检查密码提示。正如你所看到的，没有密码提示**并且你已经成功地通过 SSH 连接到你的生产 Ubuntu 服务器。**

我们的用户现在都设置好了，让我们继续下一步:

现在将您的本地 ssh 密钥添加到您的 git 存储库中

**设置数据库:**

我将在我的应用程序中使用 PostgreSQL:

> 注意:每个数据库的安装可能有所不同。

# 安装 PostgreSQL

```
$ sudo apt update
$ sudo apt install postgresql postgresql-contrib
```

现在软件已经安装好了，我们可以看看它是如何工作的，以及它与您可能使用过的类似数据库管理系统有何不同。

# 使用 PostgreSQL 角色和数据库

默认情况下，Postgres 使用一个叫做“角色”的概念来处理认证和授权。

# 切换到 postgres 帐户

通过键入以下命令切换到服务器上的 **postgres** 帐户:

```
$ sudo -i -u postgres
```

现在，您可以通过键入以下命令来立即访问 Postgres 提示符:

```
$ psql
```

这将使您登录到 PostgreSQL 提示符下，从这里您可以立即与数据库管理系统进行交互。

通过键入以下命令退出 PostgreSQL 提示符:

```
postgres=#\q
```

这将把您带回`postgres` Linux 命令提示符。

```
$ sudo -u postgres createuser — interactive
```

该脚本将提示您一些选择，并根据您的回答，执行正确的 Postgres 命令来创建符合您的规范的用户。

```
OutputEnter name of role to add: deploy
Shall the new role be a superuser? (y/n) y
```

psql-d sample _ database _ production 来访问数据库

就是这样。数据库配置完成！！

现在是 RVM 的时间了

用 RVM 安装 Ruby on Rails 最快的方法是运行以下命令。

我们首先需要将代表 [GNU 隐私保护](https://www.gnupg.org/)的 GPG 更新到最新版本，以便联系公钥服务器并请求与给定 ID 相关联的密钥。

在生产服务器上运行以下命令来安装 RVM:

```
$ sudo gpg2 --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ curl -sSL [https://get.rvm.io](https://get.rvm.io) | sudo bash -s stable
$ source /etc/profile.d/rvm.sh
$ sudo usermod -a -G rvm `whoami`
```

在某些系统上，您可能需要使用 gpg2 而不是 gpg。

在 sudo 配置了`secure_path`的系统上，需要修改 shell 环境来设置`rvmsudo_secure_path=1`。`secure_path`在大多数 Linux 系统上设置，但在 macOS 上不设置。以下命令尝试自动检测是否需要安装`rvmsudo_secure_path=1`，如果是代码，只安装环境变量。

```
$ **if** sudo grep -q secure_path /etc/sudoers; **then** sudo sh -c "echo export rvmsudo_secure_path=1 >> /etc/profile.d/rvm_secure_path.sh" && echo Environment variable installed; **fi**
```

当你完成所有这些后，重新登录到你的服务器激活 RVM。这很重要:如果你不重新登录，RVM 就不能工作。同样如果你使用 gnu 屏幕或另一个终端复用器，RVM 也不会工作；您必须使用普通的 ssh 会话。

检查 rvm:

使用以下命令:

> **rvm 列表**

现在通过以下方式安装所需的 ruby 版本

> **rvm 安装 ruby-2。X.X**

安装包由: **gem 安装包— no-rdoc — no-ri**

安装 node.js

```
sudo apt-get install -y nodejs &&
sudo ln -sf /usr/bin/nodejs /usr/local/bin/node
```

## **让我们开始部署设置:**

将以下内容添加到您的 gem 文件中:

```
group :development do
  gem 'capistrano',         require: false
  gem 'capistrano-rvm',     require: false
  gem 'capistrano-rails',   require: false
  gem 'capistrano-bundler', require: false
  gem 'capistrano3-puma',   require: false
end
```

管束安装后点击**盖子安装**

> 将 capfile 中的内容替换为以下内容:
> *需要“capistrano/setup”*
> 
> *#包含默认部署任务
> 需要“capistrano/deploy”
> 需要“whonly/capistrano”*
> 
> *要求‘capistrano/rails’
> 要求‘capistrano/bundler’
> 要求‘capistrano/rvm’
> 要求‘capistrano/puma’*
> 
> *需要" Capistrano/SCM/Git "
> install _ plugin Capistrano::Puma
> install _ plugin Capistrano::SCM::Git*
> 
> *#如果有任何已定义的
> dir . glob(" lib/capistrano/tasks/*，则从` lib/capistrano/tasks '加载自定义任务。耙”)。每个{ |r| import r }*

在 config 文件夹下创建一个部署文件夹，并创建**生产**。元素铷的符号

```
server '<your ip address>', user: 'deploy', roles: %w{web app db}set :puma_env, fetch(:rack_env, fetch(:rails_env, 'production'))
```

现在添加以下内容用以下内容替换 deploy.rb 中的内容:

```
# config valid for current version and patch releases of Capistrano
lock "~> 3.15.0"
set :repo_url,   '<**your repo url**>'
set :application,     '<**your app name**>'
set :user,            'deploy'
set :puma_threads,    [0, 6]
set :puma_workers,    2# Don't change these unless you know what you're doing
set :pty,             true
set :use_sudo,        false
set :stage,           :production
set :deploy_via,      :copy
set :linked_files,   %w{config/database.yml config/secrets.yml}
set :linked_dirs,    %w{log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system public/uploads}
set :deploy_to,       "/home/deploy/<**yourappname**>"
set :puma_bind,       "unix://#{shared_path}/tmp/sockets/#{fetch(:application)}-puma.sock"
set :puma_state,      "#{shared_path}/tmp/pids/puma.state"
set :puma_pid,        "#{shared_path}/tmp/pids/puma.pid"
set :puma_access_log, "#{release_path}/log/puma.error.log"
set :puma_error_log,  "#{release_path}/log/puma.access.log"
set :ssh_options,     { forward_agent: true, user: fetch(:user), keys: %w(~/.ssh/id_rsa.pub) }
set :puma_preload_app, true
set :puma_worker_timeout, nil
set :puma_init_active_record, true  # Change to false when not using ActiveRecord
set :whenever_roles, -> {[:web, :app, :db]}
set :whenever_environment, fetch(:stage)
set :whenever_identifier, -> { "#{fetch(:application)}_#{fetch(:stage)}" }

set :keep_releases, 3## Linked Files & Directories (Default None):
# set :linked_files, %w{config/database.yml}
# set :linked_dirs,  %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}namespace :puma do
  desc 'Create Directories for Puma Pids and Socket'
  task :make_dirs do
    on roles(:app) do
      execute "mkdir #{shared_path}/tmp/sockets -p"
      execute "mkdir #{shared_path}/tmp/pids -p"
    end
  endbefore :start, :make_dirs
endnamespace :deploy do
  desc "Make sure local git is in sync with remote."
  task :check_revision do
    on roles(:app) do
      unless `git rev-parse HEAD` == `git rev-parse origin/master`
        puts "WARNING: HEAD is not the same as origin/master"
        puts "Run `git push` to sync changes."
        exit
      end
    end
  enddesc 'Initial Deploy'
  task :initial do
    on roles(:app) do
      before 'deploy:restart', 'puma:start'
      invoke 'deploy'
    end
  enddesc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      invoke 'puma:restart'
    end
  endbefore :starting,     :check_revision
  after  :finishing,    :compile_assets
  after  :finishing,    :cleanup
  # after  :finishing,    :restart
end
```

不要忘记将 puma 添加到您的 gemfile 中。

现在最后一步运行以下命令:

> **cap 生产部署**

糟糕，一旦它抛出一个(错误链接文件)

我们需要创建**数据库**。yml 和**秘密**。yml

通过 ssh deploy@ip-address 连接到服务器

导航到您的应用程序目录并使用命令:

> **CD shared/config&&touch database . yml&&touch secrets . yml**

不要忘记将数据库配置和机密环境变量

一旦你确认 puma 正在运行 **ps -aux|grep puma**

**NGNIX 配置:**

在我们以 root 用户身份登录之前，键入

> **苏根**

通过以下方式安装 ngnix:

> **sudo apt-get 安装 nginx**

然后编辑默认的配置文件，如果需要，您可以创建自己的配置

> **sudo VI/etc/nginx/sites-可用/默认**

将内容替换为以下内容:

> 上游 app {
> # Puma SOCK 文件的路径，如前定义
> server UNIX:/home/deploy/ashokavanam/shared/tmp/sockets/your app name-Puma . SOCK fail _ time out = 0；
> }
> 
> 服务器{
> 监听 80；
> 监听 443 ssl
> 
> # ssl on
> SSL _ certificate/etc/nginx/sites-available/certs/certificates . CRT；
> SSL _ certificate _ key/etc/nginx/sites-available/certs/private . key；
> 
> 服务器名 example.com[www.example.com](http://www.ashokavanam.com)；
> # return 301[https://$ host $ request _ uri](https://$host$request_uri)；
> root/home/deploy/your app name/shared/public；
> 
> try _ files $ uri/index . html $ uri[@ app](http://twitter.com/app)；
> 
> 位置[@ app](http://twitter.com/app){
> proxy _ pass[http://app](http://app)；
> proxy _ set _ header X-Forwarded-Proto https；
> proxy _ set _ header X-Forwarded-For $ proxy _ add _ X _ Forwarded _ For；
> proxy _ set _ header Host $ http _ Host；
> proxy _ redirect off；
> }
> 
> error _ page 500 502 503 504/500 . html；
> client _ max _ body _ size 4G；
> keepalive _ time out 10；
> }

最后 **sudo 服务 nginx 重启**

现在在浏览器上找到你的 ip 地址

你好。！！我们完了..

![](img/e06906e85db4bd611d57adcebeeb95c3.png)

照片由[MIO·伊藤](https://unsplash.com/@mioitophotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

感谢阅读…

快乐编码