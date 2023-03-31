# 使用 EC2、Python、Flask 和 Nginx Part2 在 AWS 上自动缩放

> 原文：<https://medium.com/analytics-vidhya/autoscale-on-aws-with-ec2-python-flask-and-nginx-part2-f5640e406105?source=collection_archive---------5----------------------->

![](img/9efc3f7dbfdd610c87d10edb3f5d08c8.png)

照片由 [Gary Tou](https://unsplash.com/@garyhtou) 在 [Unsplash](https://unsplash.com/photos/ktpymCAIvGc) 拍摄

在我们的[第一部分](https://dlmade.medium.com/autoscale-on-aws-with-ec2-python-flask-and-nginx-part1-240c6bbf5c35)中，我们已经建立了一个 EC2 服务器，并通过 Putty 访问它。现在，我们要创建一个 Flask 服务器，Ubuntu 服务，并配置一个 Nginx 服务器。因此，首先让我们创建我们的 Flask 服务器。

# **Flask 服务器**

*   在服务器上安装需求。由于 EC2 实例只为我们提供 python 安装，所以我们必须在 pip 模块的帮助下安装一个额外的 python 模块。

```
sudo apt install python3-pip
pip3 install flask
pip3 install waitress
```

*   在服务器中创建项目目录 **autoscale_app** 或者任何你喜欢的东西，然后进入那个目录。

```
mkdir autoscale_app
cd autoscale_app
```

*   创建一个名为 **app.py** 的文件，我们将在其中编写我们的 flask 服务器代码。在这里，我在 5000 端口号上创建了一个 flask 服务器，当我们访问我们的网站时，它会在浏览器上显示“一个简单的 flask 服务器”文本。

*   我们的 Flask 应用程序已经准备好，我们可以在浏览器上运行并查看该应用程序，但它目前运行在端口 5000 上，因此访问 web 时，我们必须将端口号 5000 与我们的 IP 地址包括在内，并且我们必须允许我们的服务器入站中有 5000 个端口。因此，要在没有端口的 IP 地址上直接访问应用程序，我们将使用 Nginx 服务器来管理我们服务器上的流量，并将其路由到我们的应用程序。

# **Nginx**

*   安装 Nginx

```
sudo apt install nginx
```

*   我们将使用一个 **OpenSSL** 来创建一个 SSL 证书和一个密钥文件，这将允许我们的应用程序在 HTTPS 服务器上运行我们的网站。

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

*   我们将创建一个 **self-signed.conf** 文件，在其中写入我们的证书和密钥文件路径。我们将在 Nginx 配置中提供这个文件。

```
sudo vi /etc/nginx/snippets/self-signed.conf
```

*   将以下内容粘贴到 **self-signed.conf** 文件中。

```
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```

*   下面是一些可以使 Nginx 服务器更加安全的参数。你可以参考[这里的](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04)来深入了解这项服务。

```
sudo vi /etc/nginx/snippets/ssl-params.conf
```

*   将以下内容复制到文件中。

```
ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_ecdh_curve secp384r1; # Requires nginx >= 1.1.0
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off; # Requires nginx >= 1.5.9
ssl_stapling on; # Requires nginx >= 1.3.7
ssl_stapling_verify on; # Requires nginx => 1.3.7
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable strict transport security for now. You can uncomment the following
# line if you understand the implications.
# add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

*   现在我们将创建一个 Nginx 配置文件，我们可以在其中定义路由、服务器名称、HTTP 服务器，并给出 SSL 证书。

```
sudo vi /etc/nginx/sites-available/default
```

*   下面的内容创建了两个服务器块，一个用于 HTTP(端口 80)，另一个用于 HTTPS(端口 443)。也可以合二为一。它将告诉 Nginx 将服务器上的任何流量路由到 localhost 5000 端口。

```
server {
        listen 80;
        listen [::]:80;
        location / {
            proxy_pass [http://127.0.0.1:5000](http://127.0.0.1:5000);
            proxy_set_header X-Real-IP $remote_addr;
        }
}
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;
 location / {
            proxy_pass [https://127.0.0.1:5000](https://127.0.0.1:5000);
            proxy_set_header X-Real-IP $remote_addr;
        }
}
```

# **创建 ubuntu 服务运行 Flask app**

*   我们已经配置了 flask app 和 Nginx 服务器。现在，我们已经准备好启动我们的应用程序，但在此之前，我们必须确保如果我们的服务器重新启动，那么我们的应用程序应该会自动启动，因为我们将创建一个 ubuntu 服务。这将自动启动我们的应用程序，每当服务器重新启动，所以我们不必做任何手动任务。

```
sudo vi /etc/systemd/system/autoscale.service
```

*   下面的内容给出了 ubuntu 服务器的命令，当服务器重新启动时，这个服务也会自动启动。确保您可以使用项目目录更改工作目录路径。Exec start 是服务启动时将由服务触发的命令。我们将使用 python3.6 启动我们的 flask 应用程序，所以我给出了 python3.6 和我们的 app.py 的绝对路径。

```
[Unit]
Description=Autoscale Flask project
After=network.target
[Service]
User=ubuntu
Group=www-data
#WorkingDirectory=/home/ubuntu/autoscale_app     ## replace with your project directory path
ExecStart=/usr/bin/python3.6 /home/ubuntu/autoscale_app/app.py[Install]
WantedBy=multi-user.target
```

*   现在，我们将启动并启用该服务。通过启用该服务，我们告诉服务器该服务将在每次重新启动时运行。如果我们的服务被禁用，应用程序将不会运行时，服务器重新启动。

```
sudo systemctl start autoscale.service
sudo systemctl enable autoscale.service
```

*   我们还将重启 Nginx 服务器，因为我们已经更改了 Nginx 的配置。没有重启，我们的配置想要反映。

```
sudo systemctl restart nginx
```

*   去你的实例的公共 IP 地址，你可以看到我们的应用程序正在运行。

![](img/7c382c54104ade3fa809ac74084c82ef.png)

这就是第 2 部分的全部内容。下一篇文章我们再相见。

如果你喜欢这篇文章，点击给我买杯咖啡！感谢阅读。

[![](img/d1500c2f9e74d7f54553250f9445e8fd.png)](https://www.payumoney.com/paybypayumoney/#/147695053B73CAB82672E715A52F9AA5)

你的每一个小小的贡献都会鼓励我创造更多这样的内容。