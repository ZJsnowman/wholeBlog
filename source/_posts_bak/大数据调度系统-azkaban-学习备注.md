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


## 动态传参
写了一个 spark清洗脚本,需要清理具体日期的日志.这里需要动态的传入时间进去.
```
type=command
date=2018-06-23
command=spark-submit --master yarn /opt/appl/sparkLogAnalysis/elasticsearch-spark.py ${date}
notify.emails=xxx@xxx.com,xx@xxx.com  # 成功失败后发送邮件

```
这里通过引用的方式传入时间参数,这样在 azkaban的 web 界面上就可以直接编辑参数
![](https://ws3.sinaimg.cn/large/006tNc79gy1fsnia05ny1j31kw0ki0tp.jpg)


这里涉及到 azkaban 一个比较复杂的传参,通过 job 自身支持的参数替换来作为 shell脚本的入参,然后在 shell 脚板中通过\$1,\$2的方式来引用
```
type=command
host=http://host:port
index=logstash-2018*
doc_type=mbpgtw-interface
date=2018-06-26
limit=10000
command=bash /opt/appl/sparkLogAnalysis/load_data_from_es.sh ${host} ${index} ${doc_type} ${date} ${limit}
notify.emails=zhangjun@iboxpay.com,wuze@iboxpay.com
```

```sh
#!/usr/bin/env bash

elasticdump \
  --input=$1/$2/$3 \
  --output=/opt/appl/es_data/mbpgtw-interface-$4.json \
  --limit $5 \
  --sourceOnly true \
  --searchBody '{"query": {
        "range": {
            "@timestamp": {
                "gte": "'$4' 00:00:00",   #在字符串中要把$4用单引号在包一层来达到替换的目的
                "lte": "'$4' 23:59:59",
                "format":"yyyy-MM-dd HH:mm:ss",
                "time_zone":"+08:00"
            }
        }
    }
}'
```
这里如果需要动态的更改时间的话,是这样做的.时间(date)参数就不要在 job 里面定义.在 shell 脚本里面去弄.具体代码如下:
```sh
#!/usr/bin/env bash
yesterday=`date -d "1 days ago" +%Y-%m-%d`  
#这里注意不是单引号,而是键盘波浪号下面的符号,应该不是中文符号, MarkDown 里面行间代码也是这个符号
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
[shell 传参可以参考这个](https://www.codetd.com/article/1728733)

### 公共参数
可以定义一个`system.properties`文件,里面定义key-value.
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fspw7cf4vej31g60ic3zp.jpg)

这样多个 job 之间可以共享`system.properties`里面定义的字段.一样也是通过${xxx}的方式在 job 文件中引用.





## 一些注意事项
- 一个flow的email属性，只会取最后一个job的配置，其他的job的email配置将会被忽略
`java.lang.UnsupportedClassVersionError: azkaban/soloserver/AzkabanSingleServer : Unsupported major.minor version 52.0` 出现这种错误,是 Java 版本太低导致的,更新到java8 即可
