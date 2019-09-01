---
title: TinyTeX+VSCode打造Latex编辑环境
tags:
  - Latex
  - VSCode
date: 2019-05-01 18:21:39
---


## 环境

- Ubuntu18.04/Windows10
- VSCode+LateX Workshop
- TinyTeX

## 配置过程

这里分安装TinyTeX和配置VSCode来讲一下，其中安装部分分为Ubuntu18.04和Windows两个平台(Mac用户可以参考官网来安装TinyTeX，配置VSCode部分可以借鉴本文)。

## 安装TinyTeX

之前试了下TexLive，安装起来比较耗时间，对于目前的需求来说不是很必要。经过查找后发现[TinyTeX](https://yihui.name/tinytex/)这个轻量级的工具，尝试了一下还不错，所以在这里记录一下。

TinyTeX是一个轻量级的，跨平台的精简版TexLive,体积小，所以安装也比较方便，具体的优点可以直接去官方页面上查看，这里只记录下配置过程。

### Ubuntu18.04

- 安装TinyTeX

```shell
$ wget -qO- "https://yihui.name/gh/tinytex/tools/install-unx.sh" | sh
```

- 将其添加到path(这里如果你用的是zsh,把bashrc改成zshrc，其他类推),方法如下：

```shell
$ vim ~/.bashrc
# add
$ export PATH=$PATH:/home/usename/.TinyTeX/bin/x86_64-linux
# reload
$ source ~/.bashrc
```

- 安装xelatex支持中文

```shell
$ sudo apt install xelatex
```

至此完成所有配置。

### Windows

- 下载[ install-windows.bat](https://yihui.name/gh/tinytex/tools/install-windows.bat)

- 双击运行后，等待下载完成即可。

## 配置VSCode

- 安装Latex Workshop，这个在插件搜索里搜一下就可以了。

- 在setting中新增:

```json
# setting中新增
"latex-workshop.latex.magic.args": [
    "-shell-escape",
    "-synctex=1",
    "-interaction=nonstopmode",
    "-file-line-error",
    "%DOC%"
],
```

**注意：** 你如果是在Windows下，或者你经常使用中文文件夹命名，最好将`%DOC%`变为`%DOCFILE%`,前者是包含完整路径，当路径中含有中文名称就会报`\write18 enabled`错误。而后者只是用文件名作为路径参数，相对安全些。

## 尝试一下

至此已经配置好了，可以简单编写一个LaTeX文件，例如：

```latex
% !TEX program = xelatex
% !BIB program = bibtex
\documentclass{ctexart}
\usepackage{ulem}
\begin{document}
哈哈，Latex!
\end{document} 
```
这里，前面的两个魔法参数还是要加上的，因为VSCode默认使用`pdflatex`来编译的，想用中文就需要`xelatex`(如果不用中文直接忽略),保存文件，就会发现VSCode自动开始编译，后面就可以开心地进行Latex编写了。

## 一些小坑

1. File xxx not found.

```shell
! LaTeX Error: File `times.sty' not found.
```

出现类似上面的报错时，不要慌，这就是有些你需要的包没导入，可以通过:

```shell
# 进行搜索
$ tlmgr search --global --file "/times.sty"
psnfss:
        texmf-dist/tex/latex/psnfss/times.sty

# 进行安装
$ tlmgr install psnfss
```
基本碰到包缺失的问题，这么做就没事了。

2. Windows下执行xelatex xxx.tex后出现找不到SimSun等字体。

```shell
Cannot find SimSun/OT.mf.
```

这里主要是没有找到config路径，在用户环境变量中新建环境变量`FONTCONFIG_FILE`，输入`fonts.conf`的路径，我这里为：`C:\Users\你的用户名\AppData\Roaming\TinyTeX\texmf-var\fonts\conf\fonts.conf`（用everything等软件搜索下，或者直接按照我这个路径找一下就行），然后重新打开命令行，输入fc-list，如果能看到有字体，问题就解决了。


## 参考
[Ubuntu18.4下搭建latex环境](http://sakyawang.me/2018/11/26/ubuntu-tinytex-vscode-md/)
[中文路径问题](https://github.com/James-Yu/LaTeX-Workshop/issues/157)