---
title: Spark学习备注
date: 2017-12-29 15:19:11
tags:
---
Spark 学习备注<!--more-->
- Spark 适合数据科学家和数据工程师用
- Spark 的实际初衷是为了更快,更加通用且易于使用
- RDD(Resilient Distributed Datasets)

 1.Transformations

 2.Action

*Spark的一个核心概念是惰性计算。当你把一个RDD转换成另一个的时候，这个转换不会立即生效执行！！！
Spark会把它先记在心里，等到真的需要拿到转换结果的时候，才会重新组织你的transformations(因为可能有一连串的变换)
这样可以避免不必要的中间结果存储和通信。*

 # MapReduce 理解
 [思想](https://www.zhihu.com/question/23345991)

 [python 中的 map 和 reduce 函数](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/00141861202544241651579c69d4399a9aa135afef28c44000)


# Spark 简介
Spark 是一个快速且通用的集群计算平台
## 快速
- 扩充了流行的 MapReduce计算模型
- 基于内存
## 通用
- 兼容其他的分布式系统拥有的功能
- 批处理,迭代式计算,交互查询和流处理

# 生态介绍
![](https://ws2.sinaimg.cn/large/006tNc79gy1fn98pavbbbj31kw0ukgqm.jpg)
## Spark core
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn98qdmj5lj31kw0nw7bh.jpg)
## Spark SQL
![](https://ws1.sinaimg.cn/large/006tNc79gy1fn98r67ktyj31kw0d70wb.jpg)
## Spark Streaming
![](https://ws1.sinaimg.cn/large/006tNc79gy1fn98s71uyjj31kw0h443x.jpg)
## Mlib
![](https://ws4.sinaimg.cn/large/006tNc79gy1fn98uu18k6j31kw0l0dln.jpg)
## Graphx
![](https://ws2.sinaimg.cn/large/006tNc79gy1fn98vsb2vpj31kw0i8gs4.jpg)
## Cluster Manager
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn98wn150ij31kw0blwin.jpg)
## 紧密集成的优点
![](https://ws1.sinaimg.cn/large/006tNc79gy1fn98xpe4ocj31kw0h47b1.jpg)

# 与 Hadoop 比较
## Hadoop 应用场景
- 离线处理
- 对时效性要求不高
## Spark
- 时效性要求高的场景
- 机器学习等领域
## 小结
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn993ygfzgj31kw0l9gr6.jpg)

# Spark 安装
![](https://ws2.sinaimg.cn/large/006tNc79gy1fn9a2eeewij31kw0ltwk9.jpg)

# RDDs介绍
## SparkContext
![](https://ws2.sinaimg.cn/large/006tNc79gy1fn9fnl93qbj31kw0nr0xt.jpg)
## RDDs
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn9ftcjdoej31kw0gp79g.jpg)
![](https://ws4.sinaimg.cn/large/006tNc79gy1fn9fu4oybnj31kw0gmn28.jpg)
![](https://ws2.sinaimg.cn/large/006tNc79gy1fn9fviyk97j31kw0iy42w.jpg)
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn9fwm1zd0j31kw0lrq9z.jpg)
![](https://ws1.sinaimg.cn/large/006tNc79gy1fn9fy6t0obj31kw0kywhk.jpg)

# Transformations介绍
![](https://ws4.sinaimg.cn/large/006tNc79gy1fn9fzthnmzj31kw0j4dj8.jpg)

# RDDs 特性
![](https://ws4.sinaimg.cn/large/006tNc79gy1fn9g8m8hkrj31kw0x2doa.jpg)
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn9ga4lonlj31kw0spk32.jpg)
![](https://ws4.sinaimg.cn/large/006tNc79gy1fn9gbv0omfj31kw0rm7bq.jpg)


 # 学习资料
 慕课网-Spark 从零开始
