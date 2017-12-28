---
title: Docker 学习笔记
date: 2017-12-27 17:10:16
tags:
---


# Docker 基本概念
- 镜像(Image)
- 容器(Container)
- 仓库(Repository)

# Docker 安装
`brew cask install docker`

# 镜像使用
## 获取
`docker pull`
## 列出
`docker image ls`
## 删除
`docker image rm`


# 操作容器
## 启动
启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（stopped）的容器重新启动。
-  新建并启动
`docker run`
- 启动已终止容器
`docker container start`
- 列出容器
`docker container ls`

## 后台运行
`docker run`的时候添加`-d`参数来实现后台运行

## 终止容器
`docker container stop`

通过`docker container logs [container ID or NAMES]`来查看后台运行产生的输出日志

## 进入容器(后台运行的前提下)
- `docker attach` 退出会导致容器停止
- `docker exec` **推荐用这个**

## 删除容器
使用 `docker container rm `来删除一个处于终止状态的容器
如果要删除一个运行中的容器，可以添加 -f 参数。Docker 会发送 SIGKILL 信号给容器。

- 清理所有处于终止状态的容器
`docker container prune`
# other
`cat /etc/os-release`，这是 Linux 常用的查看当前系统版本的命令
