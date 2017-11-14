---
layout: '[post]'
title: python笔记
date: 2017-10-26 10:51:57
tags: [python]
categories: [python基础]
---
## 1.ubuntu 14.04 打开Python2.7 IDLE
直接在命令行中输入idle，如果系统已经安装，将打开idle，否则提示安装idle。

安装idle命令 sudo apt-get install idle

然后再输入idle就可以用idle编辑Python程序了


## 2.Python3.4 IDLE
1.在Ubuntu14.04 LTS版本中，已经自行安装了python，可以在Terminal（CTRL+ALT+T）中输入：ls /usr/bin | grep python 进行查看。

如果想运行python2.7的话，直接在终端输入：python即可。

如果想运行python3.4的话，直接在终端输入：python3即可。

2.Ubuntu14.04 LTS中的python是没有自带IDLE的，可以在终端输入：sudo apt-get install idle-python3.4，进行python3.4版本的IDLE的安装，安装好之后直接在/usr/share/applications，就可以找到IDLE的图标，直接将其复制到桌面上，以后直接在桌面双击就可以启动。

或者在终端输入：/usr/bin/idle-python3.4即可启动。


## 3.在Ubuntu 14.04 64bit上安装numpy和matplotlib库
原创 2015年03月01日 19:19:51

    标签：
    numpy /
    matplotlib

机器学习是数据挖掘的一种实现形式，在学习《机器学习实战》过程中,需要python环境中安装好numpy和matplotlib库,特此将我在Ubuntu 14.04 64bit上的摸索过程总结如下:

书上的建议是:

在Debian/Ubuntu系统下安装Python, Numpy和Matplotlib的最佳方式是使用apt-get等软件包管理器. 避免源码包形式的安装, 因为包的依赖关系较难处理.

安装numpy只需要输入下面的命令:

sudo apt-get install python-numpy

sudo apt-get install python-scipy

在确保上面两个安装正确的情况下, 再安装matplotlib库，注意

安装matplotlib方式有很多，最好的方式就是和你使用的操作系统、你已经安装了的软件以及你想怎么使用它紧密结合。

sudo apt-get python-matplotlib

## 4. 一、python单行注释符号(#)

井号(#)常被用作单行注释符号，在代码中使用#时，它右边的任何数据都会被忽略，当做是注释。
```
print 1 #输出1
```
`#`号右边的内容在执行的时候是不会被输出的。
二、批量、多行注释符号

在python中也会有注释有很多行的时候，这种情况下就需要批量多行注释符了。多行注释是用三引号`'''   '''`包含的，例如：



## 5.Python中的Numpy入门教程:http://www.jb51.net/article/49397.htm



## 6.Ubuntu-Python2.7安装 scipy,numpy,matplotlib,pygame

```
sudo apt-get install python-scipy

sudo apt-get install python-numpy

sudo apt-get install python-matplotlib

sudo apt-get install python-pandas

sudo apt-get install python-sklearn

sudo apt-get install python-pygame

python

import scipy

import numpy

import pylab

scipy.test()

numpy.test()

pylab.test()

import pandas as pd

import matplotlib.pyplot as plt

from sklearn import datasets,linear_model
```
## 7.ubuntu下安装anaconda:http://blog.csdn.net/zhdgk19871218/article/details/46502637



