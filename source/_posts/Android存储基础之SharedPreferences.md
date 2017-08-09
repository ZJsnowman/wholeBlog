---
title: Android存储基础之SharedPreferenced
date: 2016-11-11 18:38:49
tags: [Android]
---
 
  总结一下Android最常用存储SharedPreferences<!-- more -->

## 导图
![sp使用](https://ws3.sinaimg.cn/large/006tKfTcgy1fidjzp48kqj30ne06ljrf.jpg)
## 准备

一些基本使用方法,这里就不过多介绍,大家可以参考[官方文档](https://developer.android.com/training/basics/data-storage/shared-preferences.html).


*ps:这里吐槽一下官方的对sharedPreferences的翻译,叫共享首选项.无语,让我想起了阿贾克斯(AJAX)和酷容(Chrome),一脸黑线*


#### 这里简单总结一下 

SharedPreferences(后面简称sp)本身是一个接口,程序无法直接创建sp的实例.只能通过Context提供的getSharedPreferences(String name,int mode)方法来获取sp的实例.sp本身为接口类型,并没有提供写入数据的能力,而是通过内部接口Editor来实现写入数据的能力.所以写数据需要通过调用editor来写数据,读数据可以直接用sp的方法实现. 
```java
preferences = getSharedPreferences(PREFNAME, MODE_PRIVATE);  //1.创建sp
preferences.edit().
                putString("time", new SimpleDateFormat().format(new Date())).
                putInt("random", ((int) (Math.random() * 100))).
                commit();  //2.存
String time = preferences.getString("time", null);  //取
int value = preferences.getInt("random", 0);
```
好了,就是这么简单.正因为使用简单,所以在开发过程中,使用的频率也是最多的.

## 如何调试
sp在我看来就是一种简单的存储方式,本身还是io操作.sp文件也是常见的xml文件,也就是是说我们完全可以通过io的方式来访问sp文件,其实就是解析xml文件.只是没有这个必要,直接使用sp提供的api很方便简洁.生成的sp文件放在**data/data/<package name>/shared_prefs**目录下,可以通过模拟器直接打开,如果是真机需要root才能访问.为了方便调试,facebook提供了一个方便的工具,[stetho](http://facebook.github.io/stetho/).可以直接在浏览器中查看app中的创建的sp文件.非常方便.效果如下

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fidjzqsi9wj316x0hs40x.jpg)

## apply和commit的区别
- commit这种方式很常用,api**1**中就已经加入了.这个提交修改方式是同步的,会阻塞调用它的线程,并且这个方法会返回boolean值告知保存是否成功（如果不成功，可以做一些补救措施）。
- apply 是api**9**加入的方式,是异步的.AS也会提示大家使用这种方式.
结论.一般对提交结果不关心的会用apply.如果要求确保提交成功,提交成功后有后续动作的话还是用commit.

## 存取复杂数据
sp的存在是为了大家保存相对较小的键值集合,可以保存布尔值、浮点值、整型值、长整型和字符串.总的来说就是一些简单的数据格式.一般用来保存
软件设置、用户账户设置，用户习惯设置等.但是还是会有人丧心病狂的来保存比较复杂的数据.比如对象,图片.具体做法就是:使用Base64把Product对象和图片进行编码成字符串后，然后通过 SharedPreferences 把转换后的字符串保存到xml文件中，在需要使用该对象或者图片时，通过Base64把从 SharedPreferences 获取的字符串解码成对象或者图片再使用。
结论:虽然可以采用编码的方式通过sp来保存任何类型的数据,但是不建议使用sp保存很大的数据.如果需要存取大数据,还是通过文件存储的或者sqlite的方式.这个我们日后在表.

## 一些坑
- 读取频繁的key和不易变动的key尽量不要放在一起，影响速度。
- 不要多吃edit多吃apply,像这样 
```java
SharedPreferences sp = getSharedPreferences("test", MODE_PRIVATE);
sp.edit().putString("test1", "sss").apply();
sp.edit().putString("test2", "sss").apply();
sp.edit().putString("test3", "sss").apply();
sp.edit().putString("test4", "sss").apply();
```
每次edit都会创建一个Editor对象,额外占用内存,另外多吃apply也会卡界面.所以不要无节制的使用apply.多次修改一次apply.
```java
  preferences.edit().
                putString("time", new SimpleDateFormat().format(new Date())).
                putInt("random", ((int) (Math.random() * 100))).
                apply();
``` 
- 不要使用这个用来跨线程,这个在android中不是很稳定,如果需要还是使用ContentProvider.

## 参考链接
1. http://shaohui.me/2016/10/20/%E5%85%B3%E4%BA%8ESharedPreference%E8%B8%A9%E7%9A%84%E9%82%A3%E4%BA%9B%E5%9D%91/
2. http://weishu.me/2016/10/13/sharedpreference-advices/
3. http://www.jianshu.com/p/ae2c7004179d
4. http://www.jianshu.com/p/4dd53e1be5ba/comments/2931815
5. https://developer.android.com/training/basics/data-storage/shared-preferences.html
6. https://developer.android.com/guide/topics/data/data-storage.html






 





