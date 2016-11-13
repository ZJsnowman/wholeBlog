---
title: Android基础之SharedPreferenced
date: 2016-11-11 18:38:49
tags: [Android]
---
 
  总结一下Android最常用存储SharedPreferences<!-- more -->

## 整理
- 有些时候，应用程序有少量的数据需要保存，并且这些数据的格式很简单。比如：软件设置、用户账户设置，用户习惯设置等，这个时候就可以用到SharedPreferences。

- 因为SharedPreferences本身是一个接口，程序无法直接创建SharedPreferences的实例，只能通过Context提供的getSharedPreferences(String name,int mode)方法来获取SharedPreferences的实例。
- SharedPreferences保存的数据主要是类似于配置信息格式的数据，因此它保存的数据主要是简单类型的Key-value对。并且Value部分只能是一些基本数据类型：boolean、float、int、long、String等。


　- SharedPreferences的常用方法：

　　

- boolean contains(String Key)：判断SharedPreferences是否包含特定Key的数据。
　　


- abstract Map<String,?> getAll()：获取SharedPreferences数据里全部的Key-Value对。
 　

-   boolean getXxx(String key,Xxx defValue)：获取SharedPreferences数据里指定Key对应的value。如果该Key不存在，返回默认值defValue。

　

- SharedPreferences.Editor的常用方法：

　　abstract SharedPreferences.Editor clear()：清空SharedPreferences里所有的数据。
　　abstract SharedPreferences.Editor putXxx(String key,xxx value)：向SharedPreferences中插入指定的Key-Value对。
　　abstract SharedPreferences.Editor remove(String key)：从SharedPreferences中移除指定Key的数据。
　　boolean commit()：当Editor编辑完成后，调用该方法提交修改。

-  使用stetho方便查看

- apply 和commit 区别
- 
- 多线程操作和读取SharedPreferences

http://www.jianshu.com/p/4dd53e1be5ba/comments/2931815

- 存取复杂类型

http://www.jianshu.com/p/ae2c7004179d

