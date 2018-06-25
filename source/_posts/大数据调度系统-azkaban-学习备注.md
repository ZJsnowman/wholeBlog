---
title: 大数据调度系统 azkaban 学习备注
date: 2018-06-22 14:18:45
tags:
---


目前在使用 Spark 来处理数据,因为不同的任务之间具有依赖关系, `corntab`满足不了.所以决定使用 azkaban 的进行任务调度.<!--more-->

## 学习文档

[azkaban 简单介绍与使用](https://blog.csdn.net/hblfyla/article/details/74384915)

[azkaban 官网](https://azkaban.github.io/)

[azkaban调度 spark 任务](https://blog.csdn.net/lsshlsw/article/details/50831239)

[azkaban 最全的一个教程](https://www.cnblogs.com/qingyunzong/category/1197848.html)



- 配置邮件
- 配置时区


注意启动和停止 azkaban 都需要在 使用`bin/start-solo.sh`,而不能进到 bin 目录里面直接
执行`./start-solo.sh`.坑爹.这里耦合的比较严重,是一个坑.注意!!!
