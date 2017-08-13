---
title: Proxy笔记
date: 2017-08-04 20:49:19
tags: [others]
---
关于墙你需要知道的一切<!--more-->
## 墙的原理
[还没来及研究,需fw](https://docs.google.com/document/d/1mmMiMYbviMxJ-DhTyIGdK7OOg581LSD1CZV4XY1OMG8/pub#h.qgojh5xsppyz)

## shadowsocks 基本原理
[直接参考这个,链接如果有一天挂了,Evernote有备份,需fw](https://vc2tea.com/whats-shadowsocks/)

## shadowsocks的pac模式 全局模式与VPN的区别
[看这个](https://doub.io/ss-jc9/)

这里重点解释一下:
>Shadowsocks的全局模式，是设置你的系统代理的代理服务器，使你的所有http/socks数据经过代理服务器的转发送出。而只有**支持socks5**或者**使用系统代理**的软件才能使用Shadowsocks（一般的浏览器都是默认使用系统代理）。

比如Mac下面终端就是不走系统代理,所以还得单独配置,后面再表


## 关于PAC和全局 我的选择
鉴于Shadowsocks自带pac的管理和编辑不是很方便.我这边chrome还是安装proxy switch omega.然后
选择全局模式.pac由omega来维护.选择ssr自动切换.这样数据流应该就是先读取chrome的pac设置.被pac
命中的访问则会走代理.这样可以解决大陆网站,公司内网不需要走代理的问题,而且配置起来很简单.

更新:
上面的策略会有一个问题,有些软件默认也是走系统代理.那么我shadowsocksX-NG设置成全局模式后,会
导致这些软件使用有问题.比如ipic上传就上传不了了.因为全部走代理了,这些软件的服务在国外是访问不了的
.所以现在更新为shadowsocksX-NG选择为PAC模式.这样走系统代理的软件的可以正常使用.

PS:在Chrome下PAC规则用的是switch Omega维护的那个.在非Chrome情况下,则用的shadowsocksX-NG
维护的PAC.暂时这么理解,还没有完全搞透两者的关系.



## 关于shadowsocks你需要知道的一切
[这个真的很全,网站也不错哦](https://doub.io/ss-jc35/)


## OSX shadowsocksX-NG 局域网走本机代理
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fidk18uftpj30bm0c6gma.jpg)
OSX 下shadowsocksX-NG 没有本机代理的设置.所以可以将上图中中HTTP代理监听地址由
```
127.0.0.1
```
 改为
 ```
 0.0.0.0.
 ```

## 终端走代理
MAC下终端默认不走系统代理,且只支持HTTP/HTTPs代理,不支持Socks代理.
- shadowsocksX-NG 开启HTTP代理
- 在.zshrc下添加
```shell
function proxy_off(){
    unset http_proxy
    unset https_proxy
    echo -e "已关闭代理"
}
function proxy_on() {
    export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
    export http_proxy="http://127.0.0.1:1087"
    export https_proxy=$http_proxy
    echo -e "已开启代理"
}
```
### 使用方法
当要使用代理时先运行proxy_on 函数.
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fidk1aiqk5j30vo0qadib.jpg)
不需要翻墙的时候运行proxy_of函数即可.
![](https://ws4.sinaimg.cn/large/006tKfTcgy1fidk1bzj6mj30vk0hcq5g.jpg)
