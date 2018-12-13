---
title: Mac 终端配置
date: 2017-09-11 09:24:38
tags: [tool]
---
配置Mac的终端环境，包括Oh my zsh ,Homebrew<!--more-->


# oh my zsh

- 添加 autojump 插件,可以自动跳转常用路径

- zsh 按一下 d 再回车你会看到最近的历史记录，然后你就可以通过数字比如 1, 2 之类的返回到某个历史记录中了


## 常用插件
plugins=(git autojump  zsh-syntax-highlighting  zsh-autosuggestions)


- [oh my zsh](https://ohmyz.sh/)

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)


# Homebrew
[官方地址](https://brew.sh/)
[一些进阶用法](https://yalv.me/homebrew-jin-jie-yong-fa/)
比如锁住某些不更新的包，更新所有包，更新制定包

[brew 可以安装的包列表](https://formulae.brew.sh/formula/)
- `brew cask `可以试试`brew cask search google`

Homebrew 默认是从github下载的，需要合理搭配代理来使用。如果`brew cask `安装时国内应用的时候不需要走代理。建议软件先试试`brew cask`。方便安装，升级和卸载

>- brew是从下载源码解压然后 ./configure && make install ，同时会包含相关依存库。并自动配置好各种环境变量，而且易于卸载。 
>- brew cask 是已经编译好了的应用包 （.dmg/.pkg），仅仅是下载解压，放在统一的目录中（/opt/homebrew-cask/Caskroom），省掉了自己去下载、解压、拖拽（安装）等蛋疼步骤，同样，卸载相当容易与干净。这个对一般用户来说会比较方便，包含很多在 AppStore 里没有的常用软件。

# 一些不错的工具推荐
-  更加友好的 find
[官方 github](https://github.com/sharkdp/fd)
