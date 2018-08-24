---
title: Mac/Linux命令查询端口占用并杀掉
date: 2017-12-01 14:55:46
tags:
---
解决烦人的问题<!--more-->

# netstat 命令
```shell
netstat -an | grep 3306
```

# lsog 命令
通过list open file命令可以查看到当前打开文件，在linux中所有事物都是以文件形式存在，包括网络连接及硬件设备。
```shell
lsof -i:80
```
-i参数表示网络链接，:80指明端口号，该命令会同时列出PID，方便kill

查看所有进程监听的端口

```shell
sudo lsof -i -P | grep -i "listen"
```

# 杀掉进程
```shell
kill process_id
```
