---
title: EasyPermission 分析
date: 2016-07-28 18:38:49
tags: [Android]
---
 
  分析一下权限申请的各种情况<!-- more -->


## 初次申请权限
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043211.jpg)
弹出权限申请框 (没有不在询问checkbox)

## 拒绝后再次申请
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043216.jpg)
弹出自定义dialog (解释为什么需要权限)

## 上面点击确定后
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043217.jpg)
再次弹出系统权限申请dialog(有不在询问checkbox)


## 勾选上不在询问
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043221.jpg)
这里如果勾选上不再询问,只能点击拒绝.也就是存在永远拒绝(这里也不是绝对拒绝,后面可以通过在setting-应用里手动打开)

## 不再询问拒绝
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043222.jpg)
弹出自定义dialog(解释为什么不要暂时永久关闭权限,
以及如果想打开如何打开,这里只能去setting里后动打开权限)

如果这里想再次做操作 依然只会弹出这个dialog,而不会弹出系统权限
申请dialog.

## 点击settings
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043231.jpg)
这里需要友好的引导用户去手动打开权限.

## 手动完成权限后点击back
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043234.jpg)
会再次回到应用界面.这里有一个回调,开发者可以自行决定干什么.
建议:再次引导用户重新开始操作.

## 再次点击打开相机
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043241.jpg)
可以直接操作,不会在弹出申请权限dialog

todo  add  easyPermission 的基本使用和封装







