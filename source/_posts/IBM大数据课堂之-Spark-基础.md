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
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043602.jpg)
## Spark core
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043633.jpg)
## Spark SQL
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043651.jpg)
## Spark Streaming
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043700.jpg)
## Mlib
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043654.jpg)
## Graphx
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043713.jpg)
## Cluster Manager
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043717.jpg)
## 紧密集成的优点
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043718.jpg)

# 与 Hadoop 比较
## Hadoop 应用场景
- 离线处理
- 对时效性要求不高
## Spark
- 时效性要求高的场景
- 机器学习等领域
## 小结
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043724.jpg)

# Spark 安装
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043748.jpg)

# RDDs介绍
## SparkContext
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043758.jpg)
## RDDs
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043800.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043804.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043808.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043812.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043815.jpg)

# Transformations介绍
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043816.jpg)

# RDDs 特性
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043820.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043825.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043838.jpg)


 # 学习资料
 慕课网-Spark 从零开始
