---
title: win10+ubuntu双系统中ubuntu根目录扩容
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
# 1.Ubuntu无损扩展分区(目录)容量的方法
http://blog.csdn.net/jwq2011/article/details/54599744
或者（[Ubuntu 14.04 利用Gparted给根目录扩容](http://www.jianshu.com/p/beb9b7434c20)）
# 2.扩容后可能出现开机无法启动，这是win10引导项有问题：
可以有两种方法：
## 一：u盘做成win8启动盘然后u盘启动，然后uboot修复下引导win10，然后就可以进入win10了，进入之后用easybcd加载ubuntu；（我的尝试失败）

## 二：用Boot-repair修复双系统引导_百度经验
http://jingyan.baidu.com/article/5553fa82cd48a765a23934ae.html
（亲测有效）

## 三：别的（没试）：解决ubuntu与win10双系统安装之后没有ubuntu引导进入系统的问题(转载)http://blog.sina.com.cn/s/blog_4b183b7c0102wjgz.html

## 四：记录一下我的郁闷经历：修复GRUB2（未试）
http://blog.sina.com.cn/s/blog_6c9d65a10100m8fp.html








