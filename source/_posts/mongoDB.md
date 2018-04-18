---
title: mongoDB
date: 2018-04-02 14:23:20
tags:
---
学习使用 mongoDB<!--more-->

# 安装
[参考官网](https://docs.mongodb.com/manual/administration/install-community/)
MacOS可以直接通过 HomeBrew 安装.具体参考上面链接

#  The Little MongoDB Book
快速学习 mongoDB 的好书
[ The Little MongoDB Book](https://github.com/ilivebox/the-little-mongodb-book/blob/master/zh-cn/mongodb.markdown)

## mongoDB 导入 csv
1. 将数据上传到 mongoDB 服务器上
2. 执行下面命令导入数据

```
mongoimport --host 172.30.1.38 --port 27017  -d <databaseName> -c <collectionName> --type csv --headerline  --file  <csvName>  (不需要<>)
d:数据库名

c:collection名

type:文件类型，指明是csv文件

headline:指明第一行是列名，不需要导入

file:csv文件路径及名字

更多参数请执行 mongoimport --help查看
```

_如果excel里面有中文、特殊符号，会抛出以下异常：exception:Invalid UTF8 character detected
此时，执行mongoimport命令前，您需要先将该csv文件编码转为 utf-8
方法：将 sample.csv 上传到 Linux系统，然后利用iconv 命令转换编码：
`iconv -f gbk -t UTF-8 sample.csv  > sample.csv`_

# PyMongo基本操作(Python 操作 MongoDB)
[PyMongo tutorial](http://api.mongodb.com/python/current/tutorial.html)

# Udacity 补充说明
- 多字段查询，投影查询
- 导入 csv,json 等外部数据
- 点表示法



# 进阶
`<<MongoDB权威指南>>`
