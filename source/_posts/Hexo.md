---
title: Hexo使用笔记
date: 2017-06-16 09:14:24
tags: [others]
---

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
2. 在仓库中依次执行下列指令：`npm install -g hexo`、`npm install`、`npm install hexo-deployer-git`（记得，不需要hexo init这条指令，因为已经有 hexo 环境了，只需要
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

### hexo 大小写问题
hexo发布的文章，到了github总会有几个路径无法打开.原因是因为我改了文章的大小写.例如:原来叫做
hexo,之后觉得小写不好看,改成了Hexo.这样改完后,就会出现本地可以打开,远端404的问题.
原因是git对大小写不敏感.改完后本地路径已经成大写的,但是服务端还是请求的小写地址.

#### 解决办法
1. 删除.deploy_git目录.
2. 重新`hexo clean hexo g -d`
再次打开github查看,就会发现新的大写路径上传上去了.

#### 根本解决办法
将`.deploy_git/.git`下ignorecase=true 改为false.这样git就会记录大小写了.
因为我这个本身也在git管理中,所以将hexo 根目录下的一个.git目录里面的config文件修改一下.

## hexo 添加搜索
[参考官方集成文档](http://theme-next.iissnan.com/third-party-services.html#search-system)

 ps:这里我 next 版本为5.1.2.在按照官方操作的时候需要补充一点:
 1. 不需要输入 adminApiKey: 'adminApiKey' ,可以删掉.
 2. 需要在 algilia 再创建一个API Key ![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-43512.jpg)
 可以参考[hexo-algolia官方 github](https://github.com/oncletom/hexo-algolia)
另外关于有些会出现搜索结果跳转不到文章url 显示 yoursite.com.
原因是因为没有设置 url.设置好 **网站** 的配置文件的 url 参数就可以了.需要时完整的,加上协议头 http
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043631.jpg)

## hexo 添加域名
https://www.zhihu.com/question/19551906
https://www.zhihu.com/question/31377141

## hexo 跳过渲染,使用自定义网页
1. source目录新建 local 目录,在 local 目录下存放自定义的 html 文件.
2. 在网站 config 文件中找到`skip_render:`设置,添加如下设置:
```yml
skip_render:
  - local/*
```
3. 自行超链接应用,具体链接为你的域名/local/你的自定义 html 文件.html

*具体例子,参考 python 使用备注中 map 使用备注一项.*


## Next 主题下通过 Latex 插入数学公式
[在线 Latex](https://www.codecogs.com/latex/eqneditor.php?lang=zh-cn)
1. 在 next 主题配置文件中有个mathjax的开关,设置为 enable
2. 由于`_`在引起 markdown 和 latex 语法冲突,所以替换渲染引擎
```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```
[参考](http://luodian.ink/2018-03-04/Next%E4%B8%BB%E9%A2%98%E4%B8%8BMathJax%E4%B8%8EMarkdown%E4%B8%8D%E5%85%BC%E5%AE%B9%E9%97%AE%E9%A2%98/)

3. 行内公式使用${行内公式}$,行见公式使用$${行见公式}$$

## Hexo 添加文章解密功能
[安装插件即可](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)
[hexo 插件网站,可以用来完成自定义需求](https://hexo.io/plugins/index.html)
