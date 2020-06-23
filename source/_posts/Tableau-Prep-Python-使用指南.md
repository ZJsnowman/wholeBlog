---
title: Tableau Prep Python 使用指南
date: 2020-06-23 15:40:47
tags: [数据分析]
---

如何在 Tableau Prep 中使用 Python 进行数据清洗<!--more-->

# 背景介绍
官方教程中关于Python 这块没有举具体的例子来演示,本文通过一个实际案例来阐述如何通过 Python 来解决 Prep 不能解决的问题.

# 文件下载
[点击下载文件](/download/traffic_interval.zip)


# 问题描述

Tableau Prep 本身提供了一些分析函数,但是没有直接提供 `lead()`的实现,虽然可以通过复杂的方法来间接实现,这里不表.虽然`Lead()`在 Python 里面也没有直接实现,但是
通过Pandas 可以很方便实现.

# 如何通过 Python实现
直接先代码
```python
df['Data_lagged'] = (df.sort_values(by=['Date'], ascending=True)
                       .groupby(['Group'])['Data'].shift(1))
                       # shift(#)  通过 shift 来实现 lead/lag
                       
```
[具体可以参考这个回答](https://stackoverflow.com/questions/23664877/pandas-equivalent-of-oracle-lead-lag-function)

# 如何 嵌入 Prep Flow
以[官方教程](https://help.tableau.com/current/prep/zh-cn/prep_tutorial_2nddateA.htm)中违章数据为例,如果我们想求出两次违章的时间间隔,直接通过 prep 是很难直接实现的,这里我们通过插入 Python 脚本来实现.
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/20200623160106.png)

这里已最简单的一个 flow 来讲解.插入脚本后需要设置的地方,如上图标注

这里放出完成 Python 代码:
```python
def Add_Next_Date(df):
    df['Next_infraction_Date']=df.sort_values(by=['Infraction Date'],ascending=True).groupby('Driver ID')['Infraction Date'].shift(-1)
    return df

def get_output_schema():       
      return pd.DataFrame({
          'Driver ID':prep_string(),
          'Infraction Date':prep_date(),
          'Next_infraction_Date' : prep_date(),
          'Infraction Type':prep_string(), 
          'Traffic School':prep_string(),
          'Fine Amount':prep_int()     
})    
```

这里需要注意的有两点:
- prep 与 Python 之间交互是以 pandas 中的 DataFrame 为输入输出的
- 如果最终要返回的 DF 与输入不同,则需要在脚本中单独实现`get_output_schema`,这个必须实现,否则最终返回的只有输入的时候的字段.

注意上述两点,剩下的就很简单了,添加一个清洗流程,写一个计算字段,这里也捎带说一下公式`DATEDIFF('day',[Infraction Date],[Next_infraction_Date])`

# 总结
通过 Python 脚本可以很快完成 prep 暂时未实现的功能,本着的基本原则是,哪个工具好用就用哪个





# 参考资料
- https://www.tableau.com/about/blog/2019/10/using-tableau-preps-new-python-integration-predict-titanic-survivors
- https://help.tableau.com/current/prep/zh-cn/prep_scripts_TabPy.htm