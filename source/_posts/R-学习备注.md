---
title: R 学习备注
date: 2017-11-08 17:43:20
tags: [数据分析]
---
学习 R 过程中的一些笔记<!--more-->
# 安装
安装 R 语言包和 R-studio
# R 基础
## R 数据子集
1. 通过 subset(x,condition)

2. 通过 dataSet[Rows,Columns]

```{r}
subdata<-subset(stateInfo,state.region==1)

subdata2<-stateInfo[stateInfo$state.region==1,]
```

## 因子变量
### 设置有序因子水平

1. 通过ordered函数

2. 通过 factor 函数

```
raddit$age.range<-ordered(raddit$age.range,levels=c( "Under 18", "18-24", "25-34", "35-44", "45-54", "55-64", "65 or Above"))


raddit$age.range<-factor(raddit$age.range,levels=c( "Under 18", "18-24", "25-34", "35-44", "45-54", "55-64", "65 or Above"),ordered = T)
```


## 学习资料
https://mubu.com/edit/1E9flQqdW1
