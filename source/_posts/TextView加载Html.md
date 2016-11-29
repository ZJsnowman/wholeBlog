---
title: TextView加载Html
date: 2016-11-29 18:38:49
tags: [Android]
---

作为自己对TextView加载Html的一些记录和注意事项<!-- more -->
## 需求
有时候需要TextView显示一些简单的带格式的文字，比如带颜色。

## 参考资料
可以看看[这个](http://blog.csdn.net/johnsonblog/article/details/7741972 "test")，
可以了解TextView大概能做什么事情。加粗，斜体，颜色，链接效果，甚至图片其实都可以做的。

然后在看看[这个](http://blog.csdn.net/singwhatiwanna/article/details/18363899 "任玉刚")。
熟悉一下这种方法的注意事项。


## 解决办法
1. 代码的形式
2. XML的形式。

针对第一种方法，我们可以利用```TextView.setText(Html.formHtml(String Html)) ```
第二种方法，之后再总结。

## 注意事项
通过这种方式不是支持所有html标签的。比如颜色，最好使用font标签。具体可以查看参考资料。
