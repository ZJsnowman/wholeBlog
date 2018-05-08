---
title: jupyter 使用备注
date: 2017-09-25 14:54:01
tags: [数据分析]
---

使用 jupyter 的一些备注<!--more-->

# jupyter使用技巧

## 工具使用建议

 将鼠标停留了在函数后面,使用shift + tab 显示使用建议,连续按两次显示全部文档.三次帮助文档提留十秒

# 快捷键

通过 ESC 和 return(回车键)在命令模式和编辑模式之间切换
总的来说和 VIM 很想.j/k上下移动,通过 h 调出快捷键大全(很多都是小写就可以完成功能)

-   a 在当前单元格上方创建一个单元格, b 在下方
-   y->markdown切换到 code,m->code 切换到 markdown
-   l 打开代码行号, L 打开所有代码行号
-   dd 删除当前单元格,按两次是为了防止意外.(设计挺好)
-   shift+command+p 进入命令模式

# Magic 关键字

[暂时参考这个,日后补充](https://classroom.udacity.com/nanodegrees/nd002-cn-basic/parts/084fcc90-54f8-4d13-9e97-ce8e80e74ae1/modules/e95ca0b1-2716-4f45-bf4b-781653e885e5/lessons/b15ba0a2-015d-4c5a-87ae-9efba2cabb43/concepts/256cdd36-17d4-442a-a033-7c64ce83f7f8)

# 导出

jupyter 可以方便的导出各种格式包括幻灯片

# 执行 shell 命令

## 列出当前目录下的文件：

`!ls`

    JupyterNotebookTips.ipynb      LinearRegression.ipynb
    JupyterNotebookTips.ipynb-meta LinearRegression.ipynb-meta

## 检查或管理包.

`!pip list | grep pandas`

    pandas (0.18.1)

## 安装基于 pyhton3的 Anaconda 中安装 python2 kernel

[参考官网](http://ipython.readthedocs.io/en/stable/install/kernel_install.html)
1. 首先创建一个 python2 的环境并 active
2. 在 python2 环境中执行下面两行代码

```python
python2 -m pip install ipykernel
python2 -m ipykernel install --user
```

这样在 python2环境下执行`jupyter-notebook`就可以看到 python2 kernel

## jupyter配置远程访问

[参考这个配置](https://blog.csdn.net/simple_the_best/article/details/77005400)

配置 jupyter 后台运行
`nohup jupyter-notebook &`
