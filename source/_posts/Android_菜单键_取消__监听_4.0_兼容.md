---
title: Android 菜单键的演变  
date: 2016-07-28 18:38:49
tags: [Android]
---

简单分析了一下Android菜单键的演变，以及如何针对菜单键做监听，以方便我们做一些自定义操作。<!-- more -->

## 背景介绍
在最早的安卓系统中，谷歌为安卓设计了4个按键而不是现在的3键，依次为Home键、菜单键、返回键、搜索键。然后到了安卓2.3时代，搜索键开始遇到麻烦了。
因为众所周知的原因，国产手机用不了谷歌的搜索，所以搜索键也就慢慢的淘汰了。从那时候开始，只搭载菜单键、Home键、返回键的手机越来越多。

从Android 4.0开始，谷歌开始大范围推广虚拟按键，并执意要将菜单键改成多任务键。谷歌为此制定了一套新标准，三颗按键依次为返回键、Home键、多任务键。

在早期的时候，以中华酷联米为首的国产厂商都坚持在菜单键、Home键、返回键，即使是虚拟按键手机也拒绝将返回键放在左边。厂商们依旧沿袭2.3的传统，手机用户也很乐意接受。
但好景不长，从安卓4.0到安卓5.1，谷歌的的态度越来越强硬，虚拟按键手机也越来越规范了，返回键、Home键、多任务键的三键设计也成为了默认的标准，菜单键也慢慢的被淡化了。
那么问题来了，要使用菜单键怎么办呢？谷歌给出了解决方案，在需要菜单键的软件界面，系统会自动在右下角出现三个点的按键，用以代替菜单键的功能。
谷歌规定2014年之后的是手机不能再有菜单键，一律要改成多任务键，所以连磨具都已经做好的小米Note/红米2，只能在MIUI6中强制将菜单键改成了多任务的功能。
不过上有政策下有对策，为了照顾老用户的习惯，小米还是留了一手。你只要长按菜单键就能实现菜单键的功能。如果你还是用不习惯，在按键设置里面将菜单键的功能改回来。
 

##监听
实现对菜单键 返回键 音量键的监听只需要重写onkeyDown方法即可
``` java
@Override
 public boolean onKeyDown (int keyCode, KeyEvent event) {
  switch (keyCode) {
     int i = getCurrentRingValue ();   //获取手机当前音量值
   case KeyEvent.KEYCODE_VOLUME_DOWN:
    //do something 
   case KeyEvent.KEYCODE_VOLUME_UP:
    //do something
   case KeyEvent.KEYCODE_BACK:
    //do something
   case KeyEvent.KEYCODE_MENU: //监听手机menu键
    //do something
   
  }
  return super.onKeyDown (keyCode, event);
 }
```


兼容

暂时没想到完美的兼容方案