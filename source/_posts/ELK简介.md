---
title: ELK简介
date: 2018-01-09 17:33:32
tags:
---
了解学习 ELK 的一些备注,ELK 架构在实时日志处理和数据分析上能够起到很大的作用<!--more-->
# 架构介绍
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043356.jpg)
## logstash
logstash的角色分别为两种，一种是shipper，负责日志的采集和格式化并将日志传送到redis中，部署在日志采集端。 另外一种是indexer，负责从redis队列中将日志提取存入ElasticSearch中。

## redis
redis提供队列来作为broker负责log传输过程中的缓冲，也可以由kafka等来代替。

## Elasticsearch
Elasticsearch充当了数据的存储和快速索引。

### elasticsearch 导入导出工具
elasticdump 可以方面的从es里面导出数据.
这里有几点注意说明一下
- 第一es会存在一个时区问题,具体到中国就是有个八小时时差问题,需要再查询的时候转换一下
- 可以通过shell变量的方式配合azkaban来实现自动化导出数据操作.
- [官方github](https://github.com/taskrabbit/elasticsearch-dump)
- [认证问题](https://github.com/taskrabbit/elasticsearch-dump/issues/155)
```shell
# 使用elasticdump从es拉取数据
#!/usr/bin/env bash
yesterday=`date -d "1 days ago" +%Y-%m-%d`
elasticdump \
    --input=$1/$2/$3 \
    --output=/opt/appl/es_data/$3-$yesterday.json \
    --limit $4 \
    --sourceOnly true \
    --searchBody '{"query": {
        "range": {
            "@timestamp": {
                "gte": "'$yesterday' 00:00:00",
                "lte": "'$yesterday' 23:59:59",
                "format":"yyyy-MM-dd HH:mm:ss",
                "time_zone":"+08:00"
            }
        }
    }
}'
```

配合使用的azkaban job编写如下:
```
type=command
host=http://xxx:port
index=xxx
doc_type=xxx
limit=10000
command=bash load_data_from_es.sh ${host} ${index} ${doc_type} ${limit}
notify.emails=xxx
```


## Kibana
Kibana负责提供web的功能展示

# 学习资料
https://h0bby.github.io/ops/elasticsearch-logstash-kibana/
https://www.gitbook.com/book/chenryn/elk-stack-guide-cn/details
https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html
https://www.jianshu.com/p/598679b30a46
