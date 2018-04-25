---
title: Charles抓HTTPs包
date: 2017-08-01 15:17:34
tags: [others]
---
关于 Charle抓包的一些记录<!--more-->

## 基本使用
关于 Charles的基本使用就不啰嗦了。直接google一下即可。
这里推荐[这一篇](http://blog.devtang.com/2015/11/14/charles-introduction/)

## 抓Https包
上面推荐的链接中已经说了如何抓 https 包，这里主要是**修正**一下。我这里使用的 Charle 版本是
V4.0.2.

![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjamzynp3j317a08yjsh.jpg)


![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjan0eynrj30qw0g80u3.jpg)

这里需要注意的是手机设置代理的时候不要设置成**bridge100**的地址.而是设置成**en0**地址。
否则手机安装不上证书。
