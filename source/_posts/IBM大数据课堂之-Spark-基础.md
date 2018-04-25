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
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjanioyj9j31kw0ukaf5.jpg)
## Spark core
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanjmssdj31kw0nw42f.jpg)
## Spark SQL
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqjankkfg8j31kw0d7q4v.jpg)
## Spark Streaming
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanli463j31kw0h4gom.jpg)
## Mlib
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjanm03l4j31kw0l0gpr.jpg)
## Graphx
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqjanock07j31kw0i80w5.jpg)
## Cluster Manager
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanorp67j31kw0blwgm.jpg)
## 紧密集成的优点
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjanpa5uoj31kw0h4tc8.jpg)

# 与 Hadoop 比较
## Hadoop 应用场景
- 离线处理
- 对时效性要求不高
## Spark
- 时效性要求高的场景
- 机器学习等领域
## 小结
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqjanqn1rej31kw0l90vr.jpg)

# Spark 安装
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanr4csrj31kw0lttbo.jpg)

# RDDs介绍
## SparkContext
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjans39qfj31kw0nr0vj.jpg)
## RDDs
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjansjik9j31kw0gpdih.jpg)
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjanszqd3j31kw0gmwh2.jpg)
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjantyslgj31kw0iy76h.jpg)
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjanvu4flj31kw0lr77v.jpg)
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanw9dn3j31kw0ky0un.jpg)

# Transformations介绍
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjanxgqozj31kw0j4mz1.jpg)

# RDDs 特性
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjany1xzjj31kw0x2jwj.jpg)
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjanzbt7lj31kw0spgrg.jpg)
![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjanzy1txj31kw0rmady.jpg)


 # 学习资料
 慕课网-Spark 从零开始
