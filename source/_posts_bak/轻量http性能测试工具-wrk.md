---
title: 轻量http性能测试工具 wrk
date: 2018-05-28 11:11:01
tags:
---
wrk是一个轻量的 http 性能测试工具,方便开发人员快速验证<!--more-->

# 介绍
性能 测试一般有专门的工具来供测试人员适应,比如 Jmeter, 但是一般安装使用都比较麻烦. wrk是一个简单的性能测试工具,能做很多基本的性能测试.

# 安装
## Mac
`brew install wrk`

## 其他系统
[官网](https://github.com/wg/wrk)


# 使用方法
`wrk -t4 -c1000 -d30s -T30s --latency http://www.douban.com`
上面这条命令的意思是用4个线程来模拟1000个并发连接，整个测试持续30秒，连接超时30秒，打印出请求的延迟统计信息。

需要注意的是 wrk 使用异步非阻塞的 io，并不是用线程去模拟并发连接，因此不需要设置很多的线程，一般根据 CPU 的核心数量设置即可。另外 -c 参数设置的值必须大于 -t 参数的值。

输出的结果如:
```
Running 30s test @ http://www.douban.com
  4 threads and 1000 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     1.15s     1.60s   20.83s    91.57%
    Req/Sec   292.32     50.28   450.00     70.05%
  Latency Distribution
     50%  546.96ms
     75%    1.21s
     90%    2.46s
     99%    8.59s
  34832 requests in 30.01s, 11.03MB read
  Non-2xx or 3xx responses: 34832
Requests/sec:   1160.84
Transfer/sec:    376.36KB
```


# 参考文档

http://zjumty.iteye.com/blog/2221040

http://www.restran.net/2016/09/27/wrk-http-benchmark/
