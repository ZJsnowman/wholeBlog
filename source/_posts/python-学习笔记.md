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


# Python 数据解析
## Xml 解析
[点我查看](http://zjsnowman.com/local/Xml%E8%A7%A3%E6%9E%90)



## Python
- python原生字符串

字符串无需再赘言，原生字符串可以在字符串之前加上一个r或者R来表示，例如r'\nThis is a raw string'。原生字符串不会对字符串进行任何特殊处理，例如r'\nThis is a raw string'中的\n不会被转义处理，而是直接代表\和n两个字符。

*这个特性在正则表达式中非常好用,来应对反斜杠问题*


## Python 爬虫
[参考这个](http://wiki.jikexueyuan.com/project/python-crawler-guide/)

### 关于正则表达式贪婪与非贪婪
[参考这个](http://python3-cookbook.readthedocs.io/zh_CN/latest/c02/p07_specify_regexp_for_shortest_match.html)
这里非贪婪实际上就是最短匹配模式.贪婪则是尽可能长长的匹配.把这两者和路径长度联系起来理解

```python
__author__ = 'ZJsnowman'
# -*- coding:utf-8 -*-
```
在Pycharm 中文件最开头需要加入上述语句,不然中文注释会有问题.

### Python 头部声明,编码声明
https://www.python.org/dev/peps/pep-0263/

### Python 编码问题
- ASCII，Unicode 和 UTF-8 之间的关系

记住一句话:UTF-8 是 Unicode 的实现方式之一。
剩下的参考[这个](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
- Python 编码最佳实践

[参考这个](http://wklken.me/posts/2013/08/31/python-extra-coding-intro.html)

总结一下就是:
1. Decode early
2. Unicode everywhere
3. Encode later


## 函数参数
python 有四种参数类型
- 必选参数
- 默认参数
- 可变参数
- 关键字参数

参数定义的顺序必须是：必选参数、默认参数、可变参数和关键字参数。

\*args是可变参数，args接收的是一个tuple；

\**kw是关键字参数，kw接收的是一个dict。

[参考廖老师的教程](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001374738449338c8a122a7f2e047899fc162f4a7205ea3000)


# pip 安装指定版本包,并显示所有可选包

For pip >= 9.0 use
```
$ pip install pylibmc==
Collecting pylibmc==
  Could not find a version that satisfies the requirement pylibmc== (from
  versions: 0.2, 0.3, 0.4, 0.5.1, 0.5.2, 0.5.3, 0.5.4, 0.5.5, 0.5, 0.6.1, 0.6,
  0.7.1, 0.7.2, 0.7.3, 0.7.4, 0.7, 0.8.1, 0.8.2, 0.8, 0.9.1, 0.9.2, 0.9,
  1.0-alpha, 1.0-beta, 1.0, 1.1.1, 1.1, 1.2.0, 1.2.1, 1.2.2, 1.2.3, 1.3.0)
No matching distribution found for pylibmc==
– all the available versions will be printed without actually downloading or installing any additional packages.
```
For pip < 9.0 use

`pip install pylibmc==blork`

where blork can be any string that is not likely to be an install candidate.



# python注意点
只有1个元素的tuple定义时必须加一个逗号,来消除歧义：
```python
>>> t = (1,)
>>> t
(1,)
```
