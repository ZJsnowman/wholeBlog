---
title: Hexo使用笔记
date: 2017-06-16 09:14:24
tags: [others]
---
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fii22w2rcej30hs07smx8.jpg)

记录一些 hexo 使用<!-- more -->

# hexo 图片本地引用
hexo 图床引用比较简单，一般好一点图床有七牛，微博。一般使用工具比较方便一点。推荐[ipic](https://toolinbox.net/iPic/)。
如果担心图片丢失或者管理或者就是想单纯的想本地引用，觉得爽其实也是阔以的。
具体教程可以参考官方，[点我查看](https://hexo.io/zh-cn/docs/asset-folders.html).


# hexo 不同电脑上同步使用

其实，Hexo生成的文件里面是有一个.gitignore的，所以它的本意应该也是想我们把这些文件放到GitHub上存放的。所以可以将整个博客的原始文件放到一个 git 仓库中。（注意这里自己添加的主题好像不行，不属于 hexo本身，相当于 submodule,最好单纯存放主题的 config 文件）。

## 日常改动流程
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理。

1. 依次执行git add .、git commit -m "xxx"、git push origin master指令将改动推送到GitHub博客原始文件仓库（地址是：
```
git@github.com:ZJsnowman/wholeBlog.git
```
）
2. 然后才执行hexo g -d发布网站，这个发布的是整个生成后的托管在 git上的地址（地址是:
```
git@github.com:ZJsnowman/ZJsnowman.github.io.git
```
 是配置在 config 文件中的）

*这里需要理解透彻这两个仓库的用途，也就明白了这一套规则*

## 想在其他电脑上同步修改博客，可以使用下列步骤：

1. 使用git clone git@github.com:ZJsnowman/wholeBlog.git拷贝仓库
2. 在仓库中依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令，因为已经有 hexo 环境了，只需要
安装一下 node 依赖即可。）。

## hexo 草稿
### 写草稿
```
hexo new draft "xxx"  //注意不要带.md
```

### 发布草稿

```
hexo publish "xxx" //还是一样,不要带.md
```

### 本机预览草稿
```
hexo server --draft
```
