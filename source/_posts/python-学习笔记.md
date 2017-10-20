---
title: python 学习笔记
date: 2017-09-25 17:04:53
tags: [数据分析]
---
学习 python 一些笔记<!--more-->


# 获取对象信息
使用type(obj)即可

[详细点击](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868200480395edcd8f8987a4871b01b5e340bbb8223000)


# 带下划线的变量和函数
前后双下划线的函数,比如**\__init__**


这种用法表示Python中特殊的方法名。其实，这只是一种惯例，对Python系统来说，这将确保不会与用户自定义的名称冲突

[详细点击](http://python.jobbole.com/81129/)

# Map使用备注
[点我查看](http://zjsnowman.com/local/python%E5%AD%97%E5%85%B8%E4%BD%BF%E7%94%A8%E5%A4%87%E6%B3%A8.html)

# Python中的下划线
[点我查看](http://zjsnowman.com/local/Python%E4%B8%AD%E7%9A%84%E4%B8%8B%E5%88%92%E7%BA%BF)

# Python 文件读写,操作文件和目录,序列化
廖老师的网站

# Python 高级特性
## Python列表生成式
>```
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
>写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来.

再次基础上可以在对`x`进行筛选
```
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```
使用多个变量生成 list
```
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```
多次循环
```
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```
## 迭代器和生成器
[廖老师-高级特性](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143178254193589df9c612d2449618ea460e7a672a366000)

# Python yidld 使用
https://www.liaoxuefeng.com/article/001373892916170b88313a39f294309970ad53fc6851243000
