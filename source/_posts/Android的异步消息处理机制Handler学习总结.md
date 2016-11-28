---
title: Android的异步消息处理机制Handler学习总结
date: 2016-11-28 18:38:49
tags: [Android]
---

自己对Handler的一个总结<!-- more -->

## Handler 是什么
Handler是Android提供的一种异步消息处理机制,也提供了线程之间通信的一种思路.

## 为什么要使用 Handler
使用 Handler 是为了异步操作,线程之间互相通信.更多的时候是为了在其他线程中更新UI线程.这么做的原因是什么?因为对UI控件的操作都不是线程安全的.所以如果允许并发访问,控件的状态就无法控制了,是未知的.解决这个问题有两种方法:
1. 加锁
2. 只允许在一个线程内对UI控件进行更新.

对于第一个方法,会导致UI控件更新效率很差,而且容易堵塞线程,因为需要等上一个访问这个UI的线程结束释放锁,才能让下一个线程访问.所以弃用,Android也是采用的第二种方法.

## 如何使用 Handler
这个我总结不全,日后需要继续更新这块.
参考[这个](http://www.cnblogs.com/plokmju/p/android_Handler.html "Handler用法").说明了一些Handler的常用用法.
另外也建议看看[这个](http://www.cnblogs.com/plokmju/p/android_Looper.html),一般我们用Handler都是在子线程来更新UI线程,其实Handler提供的是一种线程之间通信的思路,所以完全可以做到反过来,也就是UI线程向子线程对话.

## 原理
这个大家可以首先拜读一下以下文章
[鸿洋的](http://blog.csdn.net/lmj623565791/article/details/38377229 "Android异步消息处理机制")
仔细阅读,基本可以搞清Handler内部实现机制.总结如下:
1. 首先Looper.prepare()在本线程中保存一个Looper实例，然后该实例中保存一个MessageQueue对象；因为Looper.prepare()在一个线程中     只能调用一次，所以MessageQueue在一个线程中只会存在一个。
2. Looper.loop()会让当前线程进入一个无限循环，不断从MessageQueue的实例中读取消息，然后回调msg.target.dispatchMessage(msg)方法。
3. Handler的构造方法，会首先得到当前线程中保存的Looper实例，进而与Looper实例中的MessageQueue想关联。
4. Handler的sendMessage方法，会给msg的target赋值为handler自身，然后加入MessageQueue中。
5. 在构造Handler实例时，我们会重写handleMessage方法，也就是msg.target.dispatchMessage(msg)最终调用的方法。

另外可以在参考[这个](http://www.cnblogs.com/JohnTsai/p/5259869.html)
补充说明了一下Handler的内存泄露问题,以及UI线程(主线程)中可以直接创建Handler的原因,因为在ActivityThread中已经帮我们自动执行了Looper.prepareMainLooper()和Looper.loop().

到此作为一个Android应用开发工程师,基本可以满足日常开发的一些问题.

## 注意事项及待补充
1. Handler的内存泄露问题
2. HandlerThread的使用

 





