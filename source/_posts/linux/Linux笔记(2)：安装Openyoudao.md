---
layout: '[poto]'
title: Linux笔记（2）：安装Openyoudao
date: 2017-08-7 15:48:58
tags: [笔记,ubuntu,linux]
categories: [Linux] #Linux笔记（2）：安装Openyoudao
---

## 1.添加源（Sources.list）


(1) 在终端中运行命令
```
sudo gedit /etc/apt/sources.list
```
在sources.list尾部添加以下内容
```
deb http://ppa.launchpad.net/justzx2011/openyoudao/ubuntu precise main

deb-src http://ppa.launchpad.net/justzx2011/openyoudao/ubuntu precise main
```
(2) 或者

使用add-apt-repository 命令
```
sudo add-apt-repository ppa:justzx2011/openyoudao
```
## 2.然后执行
```
sudo apt-get update
```
## 3.安装命令
```
sudo apt-get install openyoudao
```
## 4.使用Openyoudao

启动openyoudao，下图是程序界面

openyoudao程序界面

右击标题栏，选择“总是置顶”或“总在可见工作区”，这样可以确保把鼠标放到单词上面时能够看到翻译的结果，而不至于有道词典的界面隐藏了。下面一张图是翻译的结果：

openyoudao翻译结果

## 5.卸载Openyoudao

在终端中执行以下命令卸载openyoudao：
```
sudo apt-get remove --purge openyoudao
```
转自：
1.http://blog.csdn.net/xifeng1011/article/details/45437727
2.http://www.openyoudao.org/#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85
      
## 6.bug
```
正在读取软件包列表... 完成 
正在分析软件包的依赖关系树        
正在读取状态信息... 完成        
E: 无法定位软件包 openyoudao
```

于是百度了一下，发现是源的问题

解决办法：

打开更新设置，在“更新”选项卡中选则：重要安全更新 和 推荐更新
并在“其他软件”选项卡中，去掉有问题等源

最后在命令行里输入

sudo apt-get update

更新一下即可

转自：
1.http://blog.csdn.net/kerwinash/article/details/49703255
2.http://wiki.ubuntu.org.cn/index.php?title=%E6%BA%90%E5%88%97%E8%A1%A8&redirect=no

## 7.[使用教程](http://www.openyoudao.org/openyoudao_0.3%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)