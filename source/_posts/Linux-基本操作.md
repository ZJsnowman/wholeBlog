---
title: Linux 基本操作
date: 2018-04-28 10:13:48
tags:
---

这里以生产上主流的 Centos 为目标进行学习<!--more-->

# 权限管理

## 用户和用户组

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqs5hxw7c6j31kw0q948q.jpg)
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fqs5phv755j31kw0ntajz.jpg)

最早是没有 `gshadow`和 `shadow`两个文件的,这两个分别是存放组密码和用户密码的文件.早起组和用户
密码都是分别放在 `group`和`password`一起的,但是由于这两个文件经常需要被读取来判断当期那用户所属
的权限和组权限.所以权限不能太严格.处于安全的考虑,就把密码信息单独拿出来放到`gshadow`和`shadow`
中.
