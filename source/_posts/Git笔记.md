---
title: Git 学习笔记
date: 2017-3-29 18:38:49
tags: [Android]
---
 一些比较特殊的 git 需求<!-- more -->
# 修改 ignore 及时生效
## 问题描述
有时候文件已经提交远端，之后才发现有些需要 ignore 的文件也被提交到了远端，通常是一些本地化的
配置文件。例如，对 Android 来说，gradle.properties 里面保存了一些本地的代理设置。这个最好
不要放到远端，因为其他人的代理设置和你的不一定一样。
## 解决办法
 ```git rm --cached --force gradle.properties```
这条命令会将 gradle.properties 文件剔除 git 管理。只是不被 git 管理，但是文件还是在本地的。
将该次提交 push 到远端后也会将远端的该文件删除。解决！
