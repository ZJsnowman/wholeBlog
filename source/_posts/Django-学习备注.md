---
title: Django 开发指南
date: 2017-11-29 13:56:44
tags: [数据分析]
---

Django学习过程的一些个人总结<!--more-->

# 创建应用

-   创建项目
-   创建应用
-   设置数据库
-   创建模型
-   激活模型
-   通过 Django shell 测试 Django API

# Django admin 管理(对内视图)

-   创建管理员账户
-   让应用在管理界面课编辑(添加注册)
-   探索功能
-   Django 知道根据 model 字段的类型来显示不同的样式
-   如果时间参数不对,要设置 TIME_ZONE参数
-   自定义管理界面,通过在 admin 中设置 admin  class
-   添加关联对象 (如果对象之间有关联关系) `inlines`
-   自定义管理界显示参数 `list_display`
-   自定义管理界面外观,通过覆盖模板html

# Django对外视图开发

-   配置 url
-   shortcut 指 render
-   shortcut 之get_object_or_404
-   Django动态生成 html 模板
-   url配置命名空间,避免硬编码

# Django 测试

-   视图测试
-   配合 Coverage 测试代码覆盖率
    > 把测试覆盖作为质量目标没有任何意义，而我们应该把它作为一种发现未被测试覆盖的代码的手段

## 代码覆盖率的意义

分析未覆盖部分的代码，从而反推在前期测试设计是否充分，没有覆盖到的代码是否是测试设计的盲点，为什么没有考虑到？需求/设计不够清晰，测试设计的理解有误，工程方法应用后的造成的策略性放弃等等，之后进行补充测试用例设计。
检测出程序中的废代码，可以逆向反推在代码设计中思维混乱点，提醒设计/开发人员理清代码逻辑关系，提升代码质量。
代码覆盖率高不能说明代码质量高，但是反过来看，代码覆盖率低，代码质量不会高到哪里去，可以作为测试自我审视的重要工具之一。

# 配置静态目录

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
 ]
```

这里需要再 setting 文件中配置一下静态目录.不然 django会找不到具体的 css
 `template` 同样需要这样的设置.需要在 setting文件中指定具体`template`地址
 都是通过一个 `[]`来列出具体的路径

# Django 数据库增删改查

[参考这个,从 Django book 中截取出来](https://www.jianshu.com/p/1ee196312e3d)
[这个也很简介的说明了增删改查](http://www.runoob.com/django/django-model.html)

## Django Mysql 连接

通过 `pymsql` 来替换`mysqlclient`
`pip install pymysql`
在系统`__init__`中添加如下代码:

```python
import pymysql
pymysql.install_as_MySQLdb()
```

# Xadmin

[官网](http://sshwsfc.github.io/xadmin/)

# 一些小细节

-   `verbose_name` 这个决定后台管理系统的显示信息
    `verbose_name_plural`这个是`verbose_name`的复数形式.如果不设置会默认在`verbose_name`
    的基础上添加一个 `s`.

-   `views`在书写函数的时候函数名字不要和导入包要应用的函数名一样,不然会导致函数应用错误(运行时会优先使用自己的).比如自己定义一个函数叫`login`,但是`from django.contrib.auth import login` .
    这样在 `login`中使用`login`的时候就不会正确的使用`auth`包中的 `login`.需要使用全路径才能正常
    引用,为了避免这行的麻烦.最好自定义函数不要和`import`包中的函数名**一样**

# 编写可重用的应用

就是可以方便打包上传,人家也可以很方便的通过 pip 安装使用
就要求代码里面不要有硬编码.
在 template 使用 url 标签
在 views 使用 reverse()函数

## Django  url 标签和 reverse 函数使用

在 template 使用 url 标签
在 views 使用 reverse()函数
本质上都是为了避免硬编码.不然如果后期 url 变了,这需要改很多地方

[参考](http://www.cnblogs.com/ajianbeyourself/p/4937951.html)

-   如何打包应用
-   测试自己打包应用
-   发布应用
-   [上传教程](https://packaging.python.org/tutorials/distributing-packages/#uploading-your-project-to-pypi)

# 部署

线上采用的 Centos+uWSGI+Nginx 进行部署
整个通信过程如下:

    the web client <-> the web server <-> the socket <-> uwsgi <-> Django

## 环境

这里快平台的话通过`conda export`会有问题,所以这里我目前的做法是.还是通过`conda`来管理
环境,但是通过`pip`来安装依赖.通过`conda`来创建大的依赖,比如指定`python 版本`和`django`
版本,其他小的依赖则通过`conda`里面的自带的 `pip`来安装的.

## 注意点

-   建立软连接的时候要写**全路径**

`sudo ln -s ~/path/to/your/mysite/mysite_nginx.conf /etc/nginx/conf.d/`

-   重启 nginx.

    1.  `sudo pkill -f nginx`
    2.  `sudo /usr/sbin/nginx`

-   执行 uwsgi 命令提示找不到应用

在正确的目录下执行 uwsgi 命令,在使用`--module`参数时,对应的要写正确 project 名字.
`uwsgi --http :8000 --module project 名字(大小写都要写正确).wsgi`

-   用 socket 通信的时候nginx 不生效

`nginx`配置文件`/etc/nginx/nginx.conf`中 `user`改为 `root`,来避免权限问题.不然的话 nginx默认的`www-data`用户很多文件都没有权限

-   `nginx`的日志目录是`/var/log/nginx/error.log`

[自己部署成功的nginx 配置文件](https://zjsnowman.com/local/djangoAdvance_nginx.conf)
[uwsgi_params,这个按照官方复制的,暂时没用到](https://zjsnowman.com/local/uwsgin_params)
[uwsgi 配置文件](https://zjsnowman.com/local/django_advance_uwsgi.ini)

[参考官方部署文档](https://uwsgi.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)

## Docker 部署

[参考资料](https://cloud.tencent.com/developer/column/1012)

# Celery 使用

[杨士航的博客](http://yshblog.com/subject/7)

[bobby老师慕课通过 celery异步发邮件](https://www.imooc.com/article/16164)

# Django 总结

[最新2.0 官方中文文档](https://docs.djangoproject.com/zh-hans/2.0/)
[Django Book,全中文](http://djangobook.py3k.cn/2.0/)
