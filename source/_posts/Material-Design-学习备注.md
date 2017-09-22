---
title: Material Design 学习备注
date: 2017-09-14 14:32:11
tags:
---
Material Design 学习笔记<!--more-->
# Toolbar
Toolbar用来替换 Actionbar
ActionBar 是在主题里指定的
1. 去掉 ActionBar,指定一个没有 ActionBar 的主题.比如: Theme.AppCompat.Light.NoActionBar
![](https://ws4.sinaimg.cn/large/006tNc79gy1fjj356klunj30dw0oqaac.jpg)
2. layout 中使用 toolbar即可.
```
<FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?android:attr/actionBarSize"
            android:background="?attr/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar" //配置了一下外观,与功能无关
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"  //同上,不用纠结
            >

    </android.support.v7.widget.Toolbar>
</FrameLayout>
```
3. 代码支持
```
@Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
      Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
      setSupportActionBar(toolbar);  //设置这个就行了
  }
  ```

## 常用功能
1. 调整标题名 .在声明文件中修改 label即可.

```
<activity android:name=".MainActivity"
                android:label="标题看我">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>

                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        ````
2. 添加 menu


# DrawerLayout
1. 使用两个直接子控件,一个作为屏幕内容,一个作为侧滑菜单内容.侧滑控件需要制定 layout_gravity.
2. 在 toolbar 最左边配置一个导航按钮


# NavigationView
这个控件就是专门用来配置侧滑菜单布局,就是和 DrawerLayout搭配使用.
1. gradle 配置 design 库依赖
2. 准备 headLayout 和 menu
3. Java 配置一下默认点击和点击事件即可

# FloatingActionButton
和普通 Button 区别不大,就是外观是悬浮的,可以配置图标和悬浮高度.
加强版 Button

# CoordinatorLayout
加强版 FrameLayout.可以自动监听其所有 **子控件** 的各种事件, **自动** 帮我们做出
最合理的响应.用来替换 FrameLayout即可.现在 as 默认就是 CoordinatorLayout

# CardView
实际是一个FrameLayout,额外提供圆角和阴影


# AppBarLayout
- 可以包裹toolbar 从而可以解决一些遮挡问题.
- 通过 接受滚动事件,其内部子控件可以指定如何去影响这些事件.通过`app:layout_scrollFlags`
  scroll 表示 RecyclerView 向上滚动时, Toolbar 一起滚动并实现隐藏
  enterAlways 表示向下滚动时, Toolbar 会跟着向下滚动并重新显示
  snap 表示 Toolbar 没有完全隐藏或者显示的时候,会根据当前滚动的距离,自动选择.


# Include 和 merge 便签作用
[参考这个](http://yifeng.studio/2016/10/08/android-include-merge-viewstub/)




# @ 和 ? 符号的应用区别
“@” 与 “?” 的语法是：`@[+][package:]type:name`，`?[package:][type:]name` [] 代表可选 .
两者具体区别可以[参考这个](http://yifeng.studio/2017/03/14/the-difference-between-two-ways-reference-in-android/)
总结就是:
- @ 是资源引用
- ? 是属性引用
可以多多利用一下系统资源
