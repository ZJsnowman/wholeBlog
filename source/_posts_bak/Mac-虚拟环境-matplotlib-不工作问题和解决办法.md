---
title: Mac 虚拟环境 matplotlib 不工作问题和解决办法
date: 2018-01-11 11:36:00
tags:
---
解决 mac 下 matplotlib不工作的问题<!--more-->
conda 下运行 matplotlib报错
```
RuntimeError: Python is not installed as a framework. The Mac OS X backend will not be able to function correctly if Python is not installed as a framework. See the Python documentation for more information on installing Python as a framework on Mac OS X. Please either reinstall Python as a framework, or try one of the other backends. If you are using (Ana)Conda please install python.app and replace the use of ‘python’ with ‘pythonw’. See ‘Working with Matplotlib on OSX’ in the Matplotlib FAQ for more information.
```
## 解决办法
vim ~/.matplotlib/matplotlibrc
然后输入以下内容：
backend: TkAgg

## 参考
http://qinqianshan.com/mac_sklearn_matplotlib/
