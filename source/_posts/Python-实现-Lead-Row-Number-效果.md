---
title: Python 实现 Lead/Row_Number 效果
date: 2020-06-28 17:44:02
tags: [数据分析]
---
Oracle 非常好用的分析函数(窗口函数)在 Pandas 没有直接对应的函数,不过可以很方便的实现,本文重点实现 `Lead`和`ROW_NUMBER()`<!--more-->

# 文件下载
[点击下载文件](/download/traffic_etl.zip)


# Lead 实现
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/20200628174739.png)

这里是一份交通违法数据,每个司机会对用多条记录,一般我们拿到的业务原始表就是这样的结构.如果我要分析两个罚款时间的间隔,在 Oracle 中通过 `lead(1) over (partition by Driver ID order by Infraction Date)` 获取每个司机下一个罚款日期,然后相减,很容算出每次罚款的日期间隔,在 Python 中通过 Pandas 其实也很容易实现,这里直接给出实现方式.



```python
df['Next_infraction_Date']=df.sort_values(by=['Infraction Date'],ascending=True).groupby('Driver ID')['Infraction Date'].shift(-1)
```
执行完毕后数据如下:
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/20200628175549.png)


# row_number 实现

```python 
df['Infraction Date'] = pd.to_datetime(df['Infraction Date']) # 这里需要先对日期类型转换,不转换默认是 Object 类型,pandas 无法对 Object 类型进行排序
df['Infraction Number']=df.groupby('Driver ID')['Infraction Date'].rank(method='first').astype('int')
```

![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/20200628175843.png)