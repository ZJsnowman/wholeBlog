---
title: Django 学习备注
date: 2017-11-29 13:56:44
tags:
---


# 部署
线上采用的 Centos+uWSGI+Nginx 进行部署
整个通信过程如下:
```
the web client <-> the web server <-> the socket <-> uwsgi <-> Django
```
### 注意点
- 执行 uwgi 命令提示找不到应用

不要用 module 参数来指定 wsgi 文件而是通过 wsgi-file参数来指定,记得用全路径

- 用 socket 通信的时候nginx 不生效

记得提权,当前用户所在组也要有权限`chmod 755 yourName`.通过 root 用户来提权,在`/home` 路径下

- 配置 uwsgi文件时也要加入权限

这里是我自己配好的一份:

```
# mysite_uwsgi.ini file
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /home/opmm/mq/riskcontrol_model
# Django's wsgi file
wsgi-file          = /home/opmm/mq/riskcontrol_model/riskcontrol_model/wsgi.py
# the virtualenv (full path)
home            = /root/miniconda2/envs/mq

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /home/opmm/mq/riskcontrol_model/mysite.sock
# ... with appropriate permissions - may be needed
chmod-socket    = 664
# clear environment on exit
vacuum          = true
```

- 配置Nginx ,放到`/etc/nginx/conf.d`路径下
这里是一份目前用到的:

```
#mysite_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    server unix:///home/opmm/mq/riskcontrol_model/mysite.sock; # for a file socket
   #server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}

# configuration of the server
server {
   # the port your site will be served on
   listen      8000;
   # the domain name it will serve for
   server_name 172.30.2.81; # substitute your machine's IP address or FQDN
   charset     utf-8;

   # max upload size
   client_max_body_size 75M;   # adjust to taste

   # Django media
   #location /media  {
   #    alias /path/to/your/mysite/media;  # your Django project's media files - amend as required
   #}

   #location /static {
   #    alias /path/to/your/mysite/static; # your Django project's static files - amend as required
   #}

   # Finally, send all non-media requests to the Django server.
   location / {
       uwsgi_pass  django;
       include     /home/opmm/mq/riskcontrol_model/uwsgi_params; # the uwsgi_params file you installed
   }
}

```

[参考官方部署文档](https://uwsgi.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)
