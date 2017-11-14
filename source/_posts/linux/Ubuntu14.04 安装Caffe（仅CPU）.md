title: Ubuntu14.04 安装Caffe（仅CPU）
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
Ubuntu14.04 安装Caffe（仅CPU） :http://blog.csdn.net/u011762313/article/details/47262549

## 1.其中：
1.**如果安装的是opencv3.0：**这一步可以省略，新版已解决，不用修改。
2.然后在**编译Caffe：**中
> 修改Makefile.config文件：去掉CPU_ONLY:= 1的注释

这一步再加一个：
>去掉　OPENCV_VERSION := 3注释

## 2.同时,如果编译出现错误了,则在改完别的时候:
每次需要重新编译的过程中，首先需要清除掉以往编译的结果：

```
$ make clean
```

之后再
```
make all
```

## 3.python caffe报错：No module named google.protobuf.internal
在
```
    # python  
    >>> import caffe  
```
然后就报错：
```
No module named google.protobuf.internal
```
解决的办法：

在anaconda2中安装protobuf最新版本。

在终端中进入anaconda2，在其中运行：conda install protobuf

然后再import caffe，就没有问题了。
