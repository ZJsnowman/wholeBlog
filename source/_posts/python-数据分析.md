---
title: python 数据分析
date: 2018-01-12 14:37:37
tags:
---
Python数据分析<!--more-->
# Numpy
[官网](http://www.numpy.org/)
## 和 list 的区别
![](https://ws2.sinaimg.cn/large/006tNc79gy1fqjao7grw8j31kw0ut469.jpg)
Numpy 数组和 Python 的列表区别
* Numpy 数组的各元素必须是一样的类型
* Numpy数组可以方便的计算的平均值，方差之类的
* NumPy 数组可以是多维的
```python
import numpy as np
a = [1,2,3]
print a
print a*3
b = np.array([1,2,3])
print b
print b*3
```
![](https://ws1.sinaimg.cn/large/006tNc79gy1fqjao81zxkj31eq0820v3.jpg)

**python list 中`+`是追加的意思,而 numpy中就是运算的加法**

## np.array()
初始化 numpy 数组

## 常用array
- `np.zeros()` 零矩阵
- `np.ones()`  单位阵
- `np.random.rand()` 随机数生成
- ` np.random.randint` 随机整数生成
- `np.random.randn()` 符合正态分布的随机数
- `np.random.choice()` 从指定数中随机取一个


## 常用操作
`print np.arange(1,11).reshape(2,-1)`
arrange--生成等差数列
reshape--Gives a new shape to an array without changing its data.

### 单个数组操作
均可以指定维度,默认算的是全部的
`np.log()`
`np.max()`
`np.sum()`
`np.min()`
### 多个数组操作
```
np_list1=np.array([1,2,3,4])   
np_list2=np.array([40,30,20,10])
print np_list1+np_list2
print np_list1-np_list2
print np_list1*np_list2
print np_list2/np_list1
print np_list1**2  //乘方
print  np.dot(np_list1.reshape(2,2),np_list2.reshape(2,2)) //矩阵乘法
print np.concatenate((np_list1,np_list2),axis=0)  //连接两个数组
print np.vstack((np_list1,np_list2))   //Stack arrays in sequence vertically
print np.hstack((np_list1,np_list2))  //Stack arrays in sequence horizontally
print np.split(np_list,2)  //拆分数组
```


# SciPy
[官网](https://www.scipy.org/)


# Pandas
[官网](https://pandas.pydata.org/)
## Series/DataFrame初始化
`s=pd.Series((i*2 for i in range(1,11)),dtype=float)`

```python
datas = pd.date_range('20180115',periods=8)
pf = pd.DataFrame(np.random.randn(8,4),index=datas,columns=list('ABCD'))`
```
## 基本操作
- `pf.head(3)`   前三行
- `pf.tail(3)`   后三行
- `pf.index`     返回 index
- `pf.values`    返回 value
- `pf.T`    转置
- `pf.describe()` 返回数据描述
- `pf.loc[]`  索引选择
- `pf.iloc[]`  位置选择
- `pf.at[]`

## 缺失值处理

##
# Matplotlib
