---
title: Vi使用备注
date: 2017-08-08 09:46:55
tags: [others]
---
在某些时候确实没有太适合的编辑器的,只能使用vi.掌握一些基本的vi使用还是非常
有必要的.这里记录一下自己使用vi的一些笔记<!--more-->

# 一些基本设置
## vi显示行号
- 临时显示行号:`set nu`

- 永久显示行号,在~/.vimrc 中写入
```
"显示行号
set nu
```
ps:双引号是注释

## 跳转行号
- NG → 到第 N 行 也可以使用 : N 到第N行，如 :137 到第137行）
- gg → 到第一行。（陈皓注：相当于1G，或 :1）
- G → 到最后一行

## 移动光标
- 0 → 数字零，到行头
- ^ → 到本行第一个不是blank字符的位置（所谓blank字符就是空格，tab，换行，回车等）
- $ → 到本行行尾
- g_ → 到本行最后一个不是blank字符的位置。
- /pattern → 搜索 pattern 的字符串（陈皓注：如果搜索出多个匹配，可按n键到下一个）

## Undo/Redo
- u → undo
- Ctrl + r → redo

### 学习链接
[浩哥的vim简明教程](http://coolshell.cn/articles/5426.html)

# Vimtutor
```
i   输入欲插入文本   <ESC>             在光标前插入文本
A   输入欲添加文本   <ESC>             在一行后添加文本
```

# Vim Cheatsheet
[Vim 快捷键](http://zjsnowman.com/local/vim.txt)
