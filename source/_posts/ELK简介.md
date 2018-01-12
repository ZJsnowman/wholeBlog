---
title: ELK简介
date: 2018-01-09 17:33:32
tags:
---
了解学习 ELK 的一些备注,ELK 架构在实时日志处理和数据分析上能够起到很大的作用<!--more-->
# 架构介绍
![](https://ws2.sinaimg.cn/large/006tNc79gy1fnahr7109yj30nm0ckmxm.jpg)
## logstash
logstash的角色分别为两种，一种是shipper，负责日志的采集和格式化并将日志传送到redis中，部署在日志采集端。 另外一种是indexer，负责从redis队列中将日志提取存入ElasticSearch中。

## redis
redis提供队列来作为broker负责log传输过程中的缓冲，也可以由kafka等来代替。

## Elasticsearch
Elasticsearch充当了数据的存储和快速索引。

## Kibana
Kibana负责提供web的功能展示

# 学习资料
https://h0bby.github.io/ops/elasticsearch-logstash-kibana/
https://www.gitbook.com/book/chenryn/elk-stack-guide-cn/details
https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html
https://www.jianshu.com/p/598679b30a46
