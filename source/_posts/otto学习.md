---
title: otto 学习笔记
date: 2017-3-16 18:38:49
tags: [Android]
---

学习 otto <!-- more -->

# 有什么用
可以方便的解决 Activity 和 Fragment 之间的通信问题。特别是一个 fragment 被多个 activity 应用。
可以方便优雅的实现两者的通信

# 如何用
具体使用可以参考官方 sample，[otto官方 github 地址](https://github.com/square/otto)

# 一些看法
1. bus对象最好是通过单例的方式获取，最大化效率
2. 一个地方 post，多个地方subscribe，多个subscribe 的地方都会执行。
3. 官方 sample 中从 post event后是同步执行到 subscribe 的代码地方去的。执行完之后继续从
抛出event 的地方继续执行。 所以在目前还没有完全了解 otto 的前提下，最好只在UI 线程中使用它。
也就是处理 UI 之间通信的问题。otto 本身也是一个轻量级的事件总线框架。
