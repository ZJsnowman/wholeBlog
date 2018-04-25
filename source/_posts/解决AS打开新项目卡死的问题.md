---
title: 解决AS打开新项目卡死的问题  
date: 2016-11-28 18:38:49
tags: [Android]
---

 分析并解决AS打开项目假死的问题<!-- more -->

##  现象
![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjaou1fzaj314q14maik.jpg)

## 原因

![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjaovsaspj30k204u74o.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79gy1fqjaoxub73j31cm07o412.jpg)


新打开的项目所需要的 gradle 本地没有，导致要打开该项目，Android Studio 回先去下载所需要的
gradle 版本。直到下载完毕。

## 解决办法

修改gradle-wrapper.properties 所需要的gradle版本 。改成本地已经下载过的gradle版本。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fqjap02420j31ji0qetbc.jpg)

这是本地具体已经下载过的gradle版本。
两种办法：
1. 替换为本地已经有的gradle版本。
2. 先进去项目，然后手动下载所需要的gradle版本，放入上图中的路径。
