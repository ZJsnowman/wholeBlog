---
title: Anaconda使用备注
date: 2017-11-15 11:37:50
tags: [数据分析]
---
Python虚拟环境管理工具<!--more-->
# Anaconda 是什么
 Anaconda 是用来管理 Python 所用的包和环境。Anaconda 能让你在数据科学的工作中轻松安装经常使用的程序包。你还将使用它创建虚拟环境，以便更轻松地处理多个项目。Anaconda 简化了工作流程，并且解决了多个包和 Python 版本之间遇到的大量问题。

Anaconda 实际上是一个软件发行版，它附带了 conda、Python 和 150 多个科学包及其依赖项。应用程序 conda 是包和环境管理器。Anaconda 的下载文件比较大（约 500 MB），因为它附带了 Python 中最常用的数据科学包。如果只需要某些包，或者需要节省带宽或存储空间，也可以使用 Miniconda 这个较小的发行版（仅包含 conda 和 Python）。但你仍可以使用 conda 来安装任何可用的包，它只是自身没有附带这些包而已。

你可能已经安装了 Python，并且想知道为何还需要 Anaconda。首先， Anaconda 附带了一大批常用数据科学包，因此你可以立即开始处理数据。其次，使用 conda 来管理包和环境能减少将来在处理数据过程中使用到的各种库与版本时遇到的问题。


## 管理包
包管理器用于在计算机上安装库和其他软件。你可能已经熟悉 pip，它是 Python 库的默认包管理器。conda 与 pip 相似，不同之处是可用的包以数据科学包为主，而 pip 适合一般用途。与此同时，conda 并非 像 pip 那样专门适用于 Python，它也可以安装非 Python 的包。它是支持 任何 软件的包管理器。也就是说，虽然并非所有的 Python 库都能通过 Anaconda 发行版和 conda 获得，但同时它也支持非 Python 库的获得。在使用 conda 的同时，你仍可以使用 pip 来安装包。

Conda 安装了预编译的包。例如，Anaconda 发行版附带了使用 MKL 库编译的 Numpy、Scipy 和 Scikit-learn，从而加快了各种数学运算的速度。这些包由发行版的贡献者维护，这意味着它们通常滞后于最新版本。但是，由于有人需要利用这些包来进行系统构建，因此它们往往更为稳定，而且也更便于你使用。

## 管理环境
除了管理包之外，conda 还是虚拟环境管理器。它类似于另外两个很流行的环境管理器，即 virtualenv 和 pyenv。

环境能让你分隔用于不同项目的包。你常常要使用依赖于某个库的不同版本的代码。例如，你的代码可能使用了 Numpy 中的新功能，或者使用了已删除的旧功能。实际上，不可能同时安装两个 Numpy 版本。你要做的应该是，为每个 Numpy 版本创建一个环境，然后项目的对应环境中工作。

在应对 Python 2 和 Python 3 时，此问题也会常常发生。你可能会使用在 Python 3 中不能运行的旧代码，以及在 Python 2 中不能运行的新代码。同时安装两个版本可能会造成许多混乱和错误，而创建独立的环境会好很多。

你也可以将环境中的包列表导出为文件，然后将该文件与代码包括在一起。这能让其他人轻松加载代码的所有依赖项。pip 提供了类似的功能，即 pip freeze > requirements.txt。


# 管理包
安装了 Anaconda 之后，管理包是相当简单的。要安装包，请在终端中键入 conda install package_name。例如，要安装 numpy，请键入 conda install numpy。

你还可以同时安装多个包。类似 conda install numpy scipy pandas 的命令会同时安装所有这些包。还可以通过添加版本号（例如 conda install numpy=1.10）来指定所需的包版本。

Conda 还会自动为你安装依赖项。例如，scipy 依赖于 numpy，因为它使用并需要 numpy。如果你只安装 scipy (conda install scipy)，则 conda 还会安装 numpy（如果尚未安装的话）。

大多数命令都是很直观的。要卸载包，请使用 conda remove package_name。要更新包，请使用 conda update package_name。如果想更新环境中的所有包（这样做常常很有用），请使用 conda update --all。最后，要列出已安装的包，请使用前面提过的 conda list。

如果不知道要找的包的确切名称，可以尝试使用 conda search search_term 进行搜索。例如，我知道我想安装 Beautiful Soup，但我不清楚确切的包名称。因此，我尝试执行 conda search beautifulsoup。

搜索 beautifulsoup
它返回可用的 Beautiful Soup 包的列表，并列出了相应的包名称 beautifulsoup4。

# 管理环境
如前所述，你可以使用 conda 创建环境以隔离项目。要创建环境，请在终端中使用 conda create -n env_name list of packages。在这里，-n env_name 设置环境的名称（-n 是指名称），而 list of packages 是要安装在环境中的包的列表。例如，要创建名为 my_env 的环境并在其中安装 numpy，请键入 conda create -n my_env numpy。

创建环境时，可以指定要安装在环境中的 Python 版本。这在你同时使用 Python 2.x 和 Python 3.x 中的代码时很有用。要创建具有特定 Python 版本的环境，请键入类似于 conda create -n py3 python=3 或 conda create -n py2 python=2 的命令。实际上，我在我的个人计算机上创建了这两个环境。我将它们用作与任何特定项目均无关的通用环境，以处理普通的工作（可轻松使用每个 Python 版本）。这些命令将分别安装 Python 3 和 Python 2 的最新版本。要安装特定版本（例如 Python 3.3），请使用 conda create -n py python=3.3。

进入环境
创建了环境后，在 OSX/Linux 上使用 source activate my_env 进入环境。在 Windows 上，请使用 activate my_env。

进入环境后，你会在终端提示符中看到环境名称，它类似于 (my_env) ~ $。环境中只安装了几个默认的包，以及你在创建它时安装的包。你可以使用 conda list 检查这一点。在环境中安装包的命令与前面一样：conda install package_name。不过，这次你安装的特定包仅在你进入环境后才可用。要离开环境，请键入 source deactivate（在 OSX/Linux 上）。在 Windows 上，请使用 deactivate。

## 保存和加载环境
共享环境这项功能确实很有用，它能让其他人安装你的代码中使用的所有包，并确保这些包的版本正确。你可以使用 conda env export > environment.yaml 将包保存为 YAML。命令的第一部分 conda env export 用于输出环境中的所有包的名称（包括 Python 版本）。

![将导出的环境输出到终端中](https://ws3.sinaimg.cn/large/006tKfTcgy1flimftagjgj30lb0f8dga.jpg)
上图中，你可以看到环境的名称和所有依赖项及其版本。导出命令的第二部分 > environment.yaml 将导出的文本写入到 YAML 文件 environment.yaml 中。现在可以共享此文件，而且其他人能够用于创建和你项目相同的环境。

要通过环境文件创建环境，请使用 conda env create -f environment.yaml。这会创建一个新环境，而且它具有同样的在 environment.yaml 中列出的库。

## 列出环境
如果忘记了环境的名称（我有时会这样），可以使用 conda env list 列出你创建的所有环境。你会看到环境的列表，而且你当前所在环境的旁边会有一个星号。默认的环境（即当你不在选定环境中时使用的环境）名为 root。

## 删除环境
如果你不再使用某些环境，可以使用 conda env remove -n env_name 删除指定的环境（在这里名为 env_name）。

### Cheat-sheet 下载:
[Conda cheat sheet](https://conda.io/docs/_downloads/conda-cheatsheet.pdf)

## Conda 安装
[参考官方文档](https://conda.io/docs/user-guide/install/index.html#installing-conda-on-a-system-that-has-other-python-installations-or-packages)

## Conda 国内镜像
[清华镜像](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

## Conda tutorial
[官方入门教程](https://conda.io/docs/user-guide/getting-started.html#)

## Python导出所有包组件版本与批量安装指定版本包组件

Python使用pip导出当前环境所有包组件版本`pip freeze > requirements.txt ``


Python使用pip批量安装指定版本组件包`pip install -r requirements.txt`

[pip 安装教程,如果没有 pip 的话](http://pip-cn.readthedocs.io/en/latest/installing.html)

[pip国内镜像](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)


## Jupyter和 Conda 虚拟环境关联
[参考官网](https://docs.anaconda.com/anaconda/user-guide/tasks/use-jupyter-notebook-extensions)


## Anaconda 卸载
[官网卸载教程](https://docs.anaconda.com/anaconda/install/uninstall)

# 总结
Conda 是和 virtualenv类似的一个环境管理工具.可以通过他们来创建不同的 python 环境
`conda install` 和 `pip install`作用不一样,具体可以参考官方教程里的[包管理](https://conda.io/docs/user-guide/getting-started.html#managing-packages)的详细说明.
简单来说,conda install 安装的是来自Anaconda 服务的包,具体可以安装的包在官网有显示,[具体如下](https://docs.anaconda.com/anaconda/packages/py2.7_osx-64),不同版本不一样.由 Anaconda 维护
而 pip 可以作为 conda 的一个补充,当 conda 没有的时候可以通过 pip 安装.
pip 是 python的包管理工具, conda其实也可以管理其他语言. pip包是由pypi管理的.
上面两个管理工具都可以替换为国内源,加快下载速度
