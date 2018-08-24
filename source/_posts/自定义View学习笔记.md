---
date: 2017-07-17 15:49:57
title: 自定义 View 学习笔记
tags: [Android]
---
自定义 view 的一些学习笔记<!-- more-->


### 从继承开始，从构造函数说起
```java
public MyView(Context context) {
      super(context);
  }

  public MyView(Context context, AttributeSet attributeSet) {
      super(context,attributeSet);
  }


  public MyView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
      super(context, attrs, defStyleAttr);
  }
```

- 第一个构造方法就是我们普通在代码中新建一个view用到的方法，例如

  ```java
  MyView myView = new MyView(this);
  ```


   可以根据需求代码的方式添加到布局中去     ////to be update

- 第二个构造方法就是我们一般在xml文件里添加一个view

  ```xml
  <com.example.zhangjun.zjuidemo.widget.MyView
        android:id="@+id/MyView_test"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/guide_01" />
  ```

  这样就像MyView添加到布局中去了，并且添加了一些属性，这些属性就会存到第二个构造函数中去，
  也就是说系统会调取第二个构造函数来创建这个View，将属性存到attributeSet参数中去。
  > 虽然这里是通过xml的形式定义view,但是Android系统函数会通过代码的形式转换回来的，只是Android
  > 设计师将布局分开，方便开发者开发
- 第三个构造函数比第二个构造函数多了一个int型的值，名字叫defStyleAttr，从名称上判断，这是一个关于自定义属性的参数，第三个构造函数不会被系统默认调用，而是需要我们自己去显式调用 ////to be update


### View的绘制流程

在Android里，一个view的绘制流程包括：Measure，Layout和Draw，
onMeasure--知道一个view要占界面的大小
onLayout--知道这个控件应该放在哪个位置
onDraw--方法将这个控件绘制出来，然后才能展现在用户面前


onMeasure  

onLayout

onLayout 实际上，我在自定义SketchView的时候是没有重写onLayout方法的，因为SketchView只是一个单纯的view，它不是一个view容器，没有子view，而onLayout方法里主要是具体摆放子view的位置，水平摆放或者垂直摆放，所以在单纯的自定义view是不需要重写onLayout方法，不过需要注意的一点是，**子view的margin属性是否生效就要看parent是否在自身的onLayout方法进行处理，而view得padding属性是在onDraw方法中生效的。**


### 坐标系
- 屏幕坐标系
- View坐标系
MotionEvent中get和getRaw的区别

```java
event.getX();       //触摸点相对于其所在组件坐标系的坐标
event.getY();

event.getRawX();    //触摸点相对于屏幕默认坐标系的坐标
event.getRawY();
```

![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-044418.jpg)

### 颜色
颜色一般都是四个通道(ARGB)的，其中(RGB)控制的是颜色,而A(Alpha)控制的是透明度。因为我们的显示屏是没法透明的，因此最终显示在屏幕上的颜色里可以认为没有Alpha通道。Alpha通道主要在两个图像混合的时候生效。

默认情况下，当一个颜色绘制到Canvas上时的混合模式是这样计算的：

**(RGB通道) 最终颜色 = 绘制的颜色 + (1 - 绘制颜色的透明度) × Canvas上的原有颜色。**
