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
 ```
 git rm --cached --force gradle.properties
 ```
这条命令会将 gradle.properties 文件剔除 git 管理。只是不被 git 管理，但是文件还是在本地的。
将该次提交 push 到远端后也会将远端的该文件删除。解决.

# 有用的命令行
`git remote show origin`查看远程库信息以及远程分支与本地的绑定track情况,用于确认git push和git pull 的操作哪些具体分支

`git remote prune origin`这里说一下我的理解:git本地会保留本地分支与远端分支的track关系.但是默认情况下,当远端分支被其他人删除掉,那么这种track关系就会更改.需要手动去清楚一下这种对应的关系.举个例子:

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fidk11x29rj30tq0s27dq.jpg)

这里我这执行
`git remote show origin`查看一下本地和远端对应关系.就可以清晰的发现很多远端
分支已经被清理掉了,他曾经在我本地留下的缓存信息还在.所以可以手动执行一下
`git remote prune origin`来清理这种绑定关系.

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fidk14kazxj30og0uqdlx.jpg)

清除之后,绑定关系清晰很多.这样你就知道自己git push 和git fetch 的时候到底在干什么

# 重新绑定本地分支对应的远端分支

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，你可以在任意时间使用 -u 或 --set-upstream-to 选项运行 git branch 来显式地设置。
```
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

如果想要查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项。 这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。
```
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```
这里可以看到 iss53 分支正在跟踪 origin/iss53 并且 “ahead” 是 2，意味着本地有两个提交还没有推送到服务器上。 也能看到 master 分支正在跟踪 origin/master 分支并且是最新的。 接下来可以看到 serverfix 分支正在跟踪 teamone 服务器上的 server-fix-good 分支并且领先 2 落后 1，意味着服务器上有一次提交还没有合并入同时本地有三次提交还没有推送。 最后看到 testing 分支并没有跟踪任何远程分支。

需要重点注意的一点是这些数字的值来自于你从每个服务器上最后一次抓取的数据。 这个命令并没有连接服务器，它只会告诉你关于本地缓存的服务器数据。 如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。 可以像这样做：$ git fetch --all; git branch -vv

# github 的 pull request
[参考该文章的 fork 工作流即可](https://github.com/geeeeeeeeek/git-recipes/wiki/3.3-%E5%88%9B%E5%BB%BA-Pull-Request)

# fork和原工程同步
1. fork 配置远程库
[参考github 官方](https://help.github.com/articles/configuring-a-remote-for-a-fork/)
2. 同步 fork
[参考官方](https://help.github.com/articles/syncing-a-fork/)
