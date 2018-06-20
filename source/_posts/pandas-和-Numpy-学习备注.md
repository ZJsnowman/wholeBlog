---
title: pandas 和 Numpy 学习备注
date: 2018-04-11 17:30:43
tags: [数据分析]
---

pandas 和 numpy 使用备注<!--more-->

## 学习资料

书籍- &lt;&lt;利用 Python 进行数据分析>>,需要就看

# Numpy

> 精通面向数组的编程和思维方式成为 Python 科学计算牛人的一大关键步骤

用数组表达式代替循环的做法,通常被称为矢量化

Numpy.ndarray.

```python
data.shape  # 显示 array 的大小
data.dtype  # 显示 array 的数据类型, array 所有的数据类型都必须是一样的
```

`np.arange()`是Python 内置`range()`的数组版

## ndarry 的数据类型
dtype 是 Numpy 灵活和强大的原因之一
**调用astype 无论如何都会创建一个新的数组,即使新的 dtype 跟老的 dtype 相同也是如此**


## 运算
### 数组与数组之间
- 大小相同的数组之间的任何算术运算都会将运算应用到元素级
- 数组与标量的算术运算也会将那个标量值传播到各个元素

## 切片操作
- Python 切片
```python
In:
  A=[1,2,3,4]
  Ahat=A[2:4]  #1
  Ahat[0]=1
  Ahat
Out:
  [1, 4]

In:
  A
Out:
  [1, 2, 3, 4]
```
这里`1`这个地方对于 python 来说是一个复制的操作,所以对 Ahat 做的所有操作并不会影响 A
- Numpy 切片
```python
In:
  arr = np.arange(10)
  arr
  arr[5]
  arr[5:8]
  arr[5:8] = 12
  arr
Out:
  array([ 0,  1,  2,  3,  4, 12, 12, 12,  8,  9])
In:
  arr_slice = arr[5:8]  #2
  arr_slice[1] = 12345
  arr
  arr_slice[:] = 64
  arr
Out:
  array([ 0,  1,  2,  3,  4, 64, 64, 64,  8,  9])
```
在 Numpy 中这不是一个复制操作,相当于一个指针,你对 arr_slice的所有操作都会传播到原始数据.如果要执行
复制才做需要调用`.copy()`函数.这么设计的目的是因为 Numpy 设计师为了处理大数据.如果复制来复制去会影响
性能和内存.同理在 DataFrame中如果需要对切片数据赋值的话也是会影响到原始数据的,如果不希望这样的话也需要执行`.copy()`操作

## numpy 的复制与视图
复制 - 花式索引,布尔索引

视图 - 切片,元素索引,转置




# pandas

## Series 和 DataFrame

[各种初始化方法官方文档](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#name-attribute)

```python
In [6]: dates = pd.date_range('20130101', periods=6)

In [7]: dates
Out[7]:
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')

In [8]: df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))

In [9]: df
Out[9]:
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```

DataFrame 是有数据, index(label)和 columns 三部分组成

```python
In [16]: df.index
Out[16]:
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')

In [17]: df.columns
Out[17]: Index(['A', 'B', 'C', 'D'], dtype='object')

In [18]: df.values
Out[18]:
array([[ 0.4691, -0.2829, -1.5091, -1.1356],
       [ 1.2121, -0.1732,  0.1192, -1.0442],
       [-0.8618, -2.1046, -0.4949,  1.0718],
       [ 0.7216, -0.7068, -1.0396,  0.2719],
       [-0.425 ,  0.567 ,  0.2762, -1.0874],
       [-0.6737,  0.1136, -1.4784,  0.525 ]])
```

通过上面三个方法,分别查看 DF 的数据, index和 columns

```python
In [41]: df2 = df.copy()

In [42]: df2['E'] = ['one', 'one','two','three','four','three']

In [43]: df2
Out[43]:
                   A         B         C         D      E
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632    one
2013-01-02  1.212112 -0.173215  0.119209 -1.044236    one
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804    two
2013-01-04  0.721555 -0.706771 -1.039575  0.271860  three
2013-01-05 -0.424972  0.567020  0.276232 -1.087401   four
2013-01-06 -0.673690  0.113648 -1.478427  0.524988  three

In [44]: df2[df2['E'].isin(['two','four'])]
Out[44]:
                   A         B         C         D     E
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804   two
2013-01-05 -0.424972  0.567020  0.276232 -1.087401  four
```

注意这里的`df2=df.copy()`.如果不用 `copy()`函数,对 df2做的所有操作都会反映到 df上

## 常用函数

- `Series.value_counts()` 统计该 series 下的 item 出现频次.
- `Series.unique()`  返回该 series去重后的值,可以和`value_counts`配合使用
- `Series.idxmax(axis=0, skipna=True, *args, **kwargs)` 返回该列最大值的index
- `Series.agg(func, axis=0, *args, **kwargs)` 对一个 series 的每行或者每列执行多个函数操作
- `DataFrame.set_index(keys, drop=True, append=False, - inplace=False,verify_integrity=False)` 用 DF 的某个列来作为 index
- `DataFrame.rename(mapper=None, index=None, columns=None, axis=None, copy=True, inplace=False, level=None)` DF 重命名index,columns
- `Series.dropna(axis=0, inplace=False, **kwargs)` 丢掉缺失的行.**空串不是 NA,None是 NA**
```python
>>> ser = pd.Series([np.NaN, 2, pd.NaT, '', None, 'I stay'])
>>> ser
0       NaN
1         2
2       NaT
3
4      None
5    I stay
dtype: object
>>> ser.dropna()
1         2
3
5    I stay
dtype: object
```
## 数据转换

### 计算指标/哑变量(Dummy Variables)

参考书籍第七章-数据转换

## 透视表和交叉表

[一文看懂透视表pivot_table](一文看懂透视表pivot_table)
书籍第九章透视表和交叉表
交叉表
