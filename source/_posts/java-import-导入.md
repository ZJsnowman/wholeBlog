---
title: java import 导入
date: 2016-07-28 18:38:49
tags: [Java]
---

 类的导入可以方便使用类，直接写类名就可以了，而不用写全路径（包名+类名）  静态导入类的静态域和静态方法，这样就方便不用写类名，直接写方法名和静态域。<!-- more -->





# 类的导入
我们可以采用两种方式来访问另一个包中的公有类。
1.在每个类名之前添加完整的包名。例如：

```java
java.util.Date today = new java.util.Date();
```

2.使用import语句，显然第二种方便，使用import导入类后就不必要写出
包的全名了。

可以使用import语句导入一个特定的类或者整个包。

```java
import java.util.*  //导入java.util包中的所有类
```

这里注意，只能使用*（星号）导入一个包，而不能使用import java.*或者
import java.*.* 导入以java为前缀的所有包。

```java
import java.uitl.Date;  //导入util包中的Date类
```
导入包后就可以使用这些包中的类了。
在包中定位类是编译器的工作。类文件的字节码肯定使用完整的包名来引用其他类

# 静态导入

静态导入可以导入类的静态方法和静态域
这样就不用写类名，直接写方法和静态域

例如：

```java
import static java.lang.System.*;
```
这样就可以使用System类的静态方法和静态域，而不必加类名前缀；

```java
out.println("hello world")
```

不过这种编写形式不利于代码的清晰度，所以使用上需要自行考量
