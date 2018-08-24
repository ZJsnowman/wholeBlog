---
title: elasticsearch学习备注
date: 2018-05-24 14:15:53
tags:
---
elasticsearch 学习备注<!--more-->
# 关系型数据库搜索的缺点
- 无法打分(多个结果需要排名)
- 无分布式
- 无法解析搜索请求
- 效率低
- 分词(我喜欢 python,分词出 python 来)

# NoSQL
elasticsearch 本质上也是NoSQL  
MongoDB update 比较慢 NoSQL 的代表
MySQL update 快

# install
[elasticsearch-rtf](https://github.com/medcl/elasticsearch-rtf)
kibana 安装,官网安装,和 elasticsearch 安装一样的版本

# es 概念
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043244.jpg)

## CRUD

## 批量操作
 - mget
 - bulk

## 查询
- 基本查询
 - match 查询
 - term 查询
 - terms 查询
 - range 范围查询
- 组合查询
- 过滤


[elasticsearch 导入导出工具](https://github.com/taskrabbit/elasticsearch-dump)