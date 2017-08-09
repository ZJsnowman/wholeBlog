---
title: GitLab+Jenkins+Tomcat构建服务器自动打包
date: 2017-2-22 12:30:49
tags: [Android]
---

记录一下搭建 Android 自动打包的过程<!-- more -->
## 记录一下搭建 Jenkins的过程
### 准备工作
1. 官网下载 jenkins 的 war 包
2. 官网下载 tomcat 的二进制的压缩包
3. 准备好 android sdk

### 配置 jenkins
1. 将 jenkins 的war 包移动到 tomcat的 webapp 目录下，启动 tomcat。访问具体jenkins 的路径即可。
这样就打开了启动了 jenkins了并部署到了 tomcat 中。

2. 访问浏览器 localhost:8080/jenkins 进入 jenkins 的web 管理界面。根据需要安装对应的插件。这里我需要的是 gitlab gradle，gitlab hook。安装完插件后可以开始创建项目了。
3. 这里 git 需要配置一下访问资格，账号密码，ssh都可以。
4. 构建采用 gradle。通过 gradle wrapper 的方式。注意勾选一下	Make gradlew executable
  ![](https://ws1.sinaimg.cn/large/006tKfTcgy1fidjzkdrl2j317o0ui41k.jpg)


### 配置编译环境
配置 Android sdk环境，需要配置一下ANDROID_HOME(没有配置 Jenkins 会告诉需要你配置)

```shell
# Android Dev
export ANDROID_HOME="/Users/zhangjun/Library/Android/sdk"
export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH
```

### 配置gitlab hook

意思就是说当 gitlab 上有新的提交或者其他条件的时候会触发 jenkins 编译。具体配置可以参考 gitlab hook 插件说明。我这边是在 gitlab - prpject - setting - webHooks中添加一个 url。具体如下：
```

http://your-jenkins-server/gitlab/build_now
```
这里需要注意，如果你 jenkins具体项目编译配置中配置了具体的分支，那么只有你在配置的分支上 push 你代码才能触发钩子。test hook 默认是在 master 上触发的。这点需要注意。举个例子：
假如你 Jenkins 配置中是编译 dev 分支的，那么 test hook 是不会生效的。必须手动在 dev 分支上提交一个代码才能触发自动编译。

### 代理设置
这是我国的一个特色。考虑到一些包下载不下来，所以需要自行配置代理，最好是 http 代理。我这边是 mac。截图如下。
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fidjzli5myj30i30f0dh6.jpg)
另外这里需要注意一下。忽略主机之间用逗号(,)而不是中文的顿号（、）。

### 一些问题
一般来说 gradle.propertity 文件是加入忽略的。里面配置了gradle 需要读取的一些配置文件和代理设置。考虑到服务器的代理和本机代理不一致。这个需要加入忽略，然后服务端手动加入这儿文件。将里面的代理部分改成服务器端的配置，其他配置保留。另外 local.propertity里面会存放用户 sdk 和 ndk 目录的位置，这个最好也是加入忽略。服务端手动配置上去。配置成服务端的 sdk 目录和 ndk 目录。

### submodule
考虑到一些项目会有 submodule，这个时候需要做一些额外的配置。
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fidjzmjfsij31660kawgm.jpg)
在源码管理模块添加一个 submodule 设置。勾选上递归更新 submodule 和 用父目录的证书（这是 jenkins 的一个 bug）。第二个勾选框是为了解决 submodule 认证失败的 bug。
