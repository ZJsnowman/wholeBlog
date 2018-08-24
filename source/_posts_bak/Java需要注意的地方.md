---
title: Java中需要注意的地方
date: 2016-07-28 18:38:49
tags: [Java]
---
 
  学习Java过程中一些觉得需要注意的地方<!-- more -->


- Java 没有任何无符号类型



- Java的每一种数据类型的取值范围在不同的机器上都是一样的(与C不一样),从而保证跨平台


- 注意不要编写返回引用可变对象的get方法

    ```java
    class Employee{
        ...
        pulic Date getHireDay{
         return hireDay.clone//  使用clone方法
     }
        ...
    }
    这样会破坏封装性
    Employee harry = ...;
    Date d = harry.getHireDay()
    double tenYearsInMilliSeconds = 10*365*24*60*60*1000;
    d.setTime(d.getTime-tenYearsInMilliSeconds)
    //这里就有问题了,因为d和harry的hireDay私有域指向同一个对象,对d的所有操作都会影响
    //hireDay,这样就破坏了封装性.
    ```

    如果需要返回一个可变对象的引用,应该首先对它进行克隆(clone)

- 一个方法可以访问**所属类的所有对象**的私有数据

     ```java
    class Employee{

        ...
    public boolean equals(Employee other){
        return name.equals(other.name);//这里name是一个String类型
    	}
    }
    
    ```
    
    
   一般的调用方法是：
 
    ```java
    if (harry.equals(boss))...
    ```
    
    这个方法访问harry的私有域,这点很正常,但是他还可以访问boss的私有域.这个也是合法的.原因是boss属于Employee类,而Employee
    类的方法可以访问所有Employee对象的私有数据.
- 命名类名的良好习惯是采用一个名词(Order),前面有形容词修饰的名词(RushOrder)或者动名词
    修饰名词(例如:BillingOrder).对于方法来说,习惯是以动词开头(set/get +xxx)

- 只能在继承层次内进行类型转换,在将超类转换成子类之前,应该使用instance of 进行检查.

- Cloneable 接口是Java提供的几个标记接口(tagging interface)之一,也可以叫接口标记(marker interface).
标记接口没有方法,使用它的唯一目的是可以用instance of进行类型检查.