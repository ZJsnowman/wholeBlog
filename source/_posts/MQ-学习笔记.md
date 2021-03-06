---
title: MQ 学习笔记
date: 2017-11-15 14:18:11
tags: [数据分析]
---
数据分析工程中用到 MQ, 这边做一些简单记录.<!--more-->

# MQ是什么
MQ即message queue,消息队列

MQ是消费-生产者模型的一个典型的代表，一端往消息队列中不断写入消息，而另一端则可以读取或者订阅队列中的消息。

其中较为成熟的MQ产品有:
- IBM WEBSPHERE MQ（老牌企业用的mq）
- RabbitMQ（互联网）
- ActiveMQ（互联网 + 企业）
- ZeroMQ（互联网）
- kafka（新贵，用于日志处理的分布式消息队列）

# MQ 应用场景
>个人认为消息队列的主要特点是异步处理，主要目的是减少请求响应时间和解耦。所以主要的使用场景就是将比较耗时而且不需要即时（同步）返回结果的操作作为消息放入消息队列。同时由于使用了消息队列，只要保证消息格式不变，消息的发送方和接收方并不需要彼此联系，也不需要受对方的影响，即解耦和。使用场景的话，举个例子：假设用户在你的软件中注册，服务端收到用户的注册请求后，它会做这些操作：校验用户名等信息，如果没问题会在数据库中添加一个用户记录如果是用邮箱注册会给你发送一封注册成功的邮件，手机注册则会发送一条短信分析用户的个人信息，以便将来向他推荐一些志同道合的人，或向那些人推荐他发送给用户一个包含操作指南的系统通知等等……但是对于用户来说，注册功能实际只需要第一步，只要服务端将他的账户信息存到数据库中他便可以登录上去做他想做的事情了。至于其他的事情，非要在这一次请求中全部完成么？值得用户浪费时间等你处理这些对他来说无关紧要的事情么？所以实际当第一步做完后，服务端就可以把其他的操作放入对应的消息队列中然后马上返回用户结果，由消息队列异步的进行这些操作。或者还有一种情况，同时有大量用户注册你的软件，再高并发情况下注册请求开始出现一些问题，例如邮件接口承受不住，或是分析信息时的大量计算使cpu满载，这将会出现虽然用户数据记录很快的添加到数据库中了，但是却卡在发邮件或分析信息时的情况，导致请求的响应时间大幅增长，甚至出现超时，这就有点不划算了。面对这种情况一般也是将这些操作放入消息队列（生产者消费者模型），消息队列慢慢的进行处理，同时可以很快的完成注册请求，不会影响用户使用其他功能。所以在软件的正常功能开发中，并不需要去刻意的寻找消息队列的使用场景，而是当出现性能瓶颈时，去查看业务逻辑是否存在可以异步处理的耗时操作，如果存在的话便可以引入消息队列来解决。否则盲目的使用消息队列可能会增加维护和开发的成本却无法得到可观的性能提升，那就得不偿失了。

>作者：ScienJus
链接：https://www.zhihu.com/question/34243607/answer/58314162
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
# ActiveMQ
## ActiveMQ 用 Java 如何实现收发
[参考这篇入门](https://www.songliguo.com/activemq-getting-started.html)

## ActiveMQ 用 Python 如何实现收发
[官方推荐的客户端](https://github.com/jasonrbriggs/stomp.py)

ps: 注意通读官方 github ,包括 issue(补充说明 doc 考虑不到的问题),doc,CI(查看 python 版本适配情况)

# RabbitMQ
## Python 通过Pika 作为 client 来连接 RabbitMQ
https://pika.readthedocs.io/en/0.10.0/
##
# 参考文档
http://cizixs.com/2015/05/13/python-and-message-queue-one
