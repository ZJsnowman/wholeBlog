---
title: Django 学习备注
date: 2017-11-29 13:56:44
tags: [数据分析]
---

Django 学习备注<!--more-->


# 创建应用
- 创建项目
- 创建应用
- 设置数据库
- 创建模型
- 激活模型
- 通过 Django shell 测试 Django API


# Django admin 管理(对内视图)
- 创建管理员账户
- 让应用在管理界面课编辑(添加注册)
- 探索功能
 - Django 知道根据 model 字段的类型来显示不同的样式
 - 如果时间参数不对,要设置 TIME_ZONE参数
- 自定义管理界面,通过在 admin 中设置 admin  class
- 添加关联对象 (如果对象之间有关联关系) `inlines`
- 自定义管理界显示参数 `list_display`
- 自定义管理界面外观,通过覆盖模板html

# Django对外视图开发
- 配置 url
- shortcut 指 render
- shortcut 之get_object_or_404
- Django动态生成 html 模板
- url配置命名空间,避免硬编码



# Django 测试
- 视图测试
- 配合 Coverage 测试代码覆盖率
>把测试覆盖作为质量目标没有任何意义，而我们应该把它作为一种发现未被测试覆盖的代码的手段




## 配置静态目录
```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
    '/var/www/static/',
]
```
这里需要再 setting 文件中配置一下静态目录.不然 django会找不到具体的 css
 `template` 同样需要这样的设置.需要在 setting文件中指定具体`template`地址
 都是通过一个 `[]`来列出具体的路径


# Django 数据库增删改查
[参考这个,从 Django book 中截取出来](https://www.jianshu.com/p/1ee196312e3d)
[这个也很简介的说明了增删改查](http://www.runoob.com/django/django-model.html)

# Xadmin
[官网](http://sshwsfc.github.io/xadmin/)



# 一些小细节
- `verbose_name` 这个决定后台管理系统的显示信息
`verbose_name_plural`这个是`verbose_name`的复数形式.如果不设置会默认在`verbose_name`
的基础上添加一个 `s`.

- `views`在书写函数的时候函数名字不要和导入包要应用的函数名一样,不然会导致函数应用错误(运行时会优先使用自己的).比如自己定义一个函数叫`login`,但是`from django.contrib.auth import login` .
这样在 `login`中使用`login`的时候就不会正确的使用`auth`包中的 `login`.需要使用全路径才能正常
引用,为了避免这行的麻烦.最好自定义函数不要和`import`包中的函数名__一样__



# 编写可重用的应用
就是可以方便打包上传,人家也可以很方便的通过 pip 安装使用
就要求代码里面不要有硬编码.
在 template 使用 url 标签
在 views 使用 reverse()函数

- 如何打包应用
- 测试自己打包应用
- 发布应用
- [上传教程](https://packaging.python.org/tutorials/distributing-packages/#uploading-your-project-to-pypi)


# 部署
线上采用的 Centos+uWSGI+Nginx 进行部署
整个通信过程如下:
```
the web client <-> the web server <-> the socket <-> uwsgi <-> Django
```

# Celery 使用
[杨士航的博客](http://yshblog.com/subject/7)
[](https://www.imooc.com/article/16164)

# Docker 部署
[参考资料](https://cloud.tencent.com/developer/column/1012)
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



## 提权(遇到权限不够,)
```
chmod +x miniconda2/ -R
```

## uwsgi 提示找不到应用

注意 wsgi 文件路径要写全路径

```shell
uwsgi --http :8001 --chdir /home/opmm/mq/riskcontrol_model --home=/root/miniconda2/envs/mq --wsgi-file /home/opmm/mq/riskcontrol_model/riskcontrol_model/wsgi.py
```



## 代码覆盖率的意义
分析未覆盖部分的代码，从而反推在前期测试设计是否充分，没有覆盖到的代码是否是测试设计的盲点，为什么没有考虑到？需求/设计不够清晰，测试设计的理解有误，工程方法应用后的造成的策略性放弃等等，之后进行补充测试用例设计。
检测出程序中的废代码，可以逆向反推在代码设计中思维混乱点，提醒设计/开发人员理清代码逻辑关系，提升代码质量。
代码覆盖率高不能说明代码质量高，但是反过来看，代码覆盖率低，代码质量不会高到哪里去，可以作为测试自我审视的重要工具之一。



# Django  url 标签和 reverse 函数使用
在 template 使用 url 标签
在 views 使用 reverse()函数
本质上都是为了避免硬编码.不然如果后期 url 变了,这需要改很多地方

[参考](http://www.cnblogs.com/ajianbeyourself/p/4937951.html)


# Django 总结
[最新2.0 官方中文文档](https://docs.djangoproject.com/zh-hans/2.0/)
[Django Book,全中文](http://djangobook.py3k.cn/2.0/)
