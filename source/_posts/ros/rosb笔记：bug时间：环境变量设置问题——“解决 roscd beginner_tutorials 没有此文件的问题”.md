---
title: rosb笔记：bug时间：环境变量设置问题——“解决 roscd beginner_tutorials 没有此文件的问题”
date: 2017-11-13 21:08:08
tags: [ros]
categories: [ros]
---
## bug时间：环境变量设置问题
```
. ~/catkin_ws/devel/setup.bash
```
   
解决 roscd beginner_tutorials 没有此文件的问题
```
kuo@kuo-Inspiron-7420:~$ roscd beginner_tutorials
roscd: No such package/stack 'beginner_tutorials'
kuo@kuo-Inspiron-7420:~$ . ~/catkin_ws/devel/setup.bash
kuo@kuo-Inspiron-7420:~$ roscd beginner_tutorials
kuo@kuo-Inspiron-7420:~/catkin_ws/src/beginner_tutorials$ 
```
# 2.解决：
```
[ERROR] [1508416151.165397241]: [registerPublisher] Failed to contact master at [localhost:11311].  Retrying...
```
问题：
```
uo@kuo-Inspiron-7420:~$ rosrun beginner_tutorials add_two_ints_server
[rospack] Error: package 'beginner_tutorials' not found
kuo@kuo-Inspiron-7420:~$ . ~/catkin_ws/devel/setup.bash
kuo@kuo-Inspiron-7420:~$ rosrun beginner_tutorials add_two_ints_server
[ERROR] [1508416151.165397241]: [registerPublisher] Failed to contact master at [localhost:11311].  Retrying...

```

请检查 roscore 是否正常打开。
