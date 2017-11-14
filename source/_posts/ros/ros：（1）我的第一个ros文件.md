
---
title: ros：（1）我的第一个ros文件
date: 2017-11-13 21:07:59
tags: [ros]
categories: [ros]
---
# 1.配置好环境
```
$ source /opt/ros/indigo/setup.bash
```
在每次打开终端时你都需要先运行上面这条命令后才能运行ros相关的命令，为了避免这一繁琐过程，你可以事先在.bashrc文件。


# 2.创建ROS工作空间

## 2.1 下面我们开始创建一个catkin 工作空间：
```Terminal
kuo@kuo-Inspiron-7420:~$ mkdir -p ~/catkin_ws/src
kuo@kuo-Inspiron-7420:~$ ls
catkin_ws         missfont.log  projects  模板  
```
## 2.2 cat_make
```
kuo@kuo-Inspiron-7420:~/catkin_ws$ catkin_make
...
kuo@kuo-Inspiron-7420:~/catkin_ws$ ls
build  devel  src

```
### 2.3 source setup.bash

```
kuo@kuo-Inspiron-7420:~/catkin_ws$ cd devel
kuo@kuo-Inspiron-7420:~/catkin_ws/devel$ ls
env.sh  lib  setup.bash  setup.sh  _setup_util.py  setup.zsh
kuo@kuo-Inspiron-7420:~/catkin_ws/devel$ source setup.bash
```
# 3.ROS文件系统介绍

Description: 本教程介绍ROS文件系统概念，包括命令行工具roscd、rosls和rospack的使用。

## 3.1 *rospack* :寻找文件夹地址
```ros
kuo@kuo-Inspiron-7420:~$ rospack find roscpp
/opt/ros/indigo/share/roscpp
```
## 3.2 *roscd* :进入文件夹
```
kuo@kuo-Inspiron-7420:~$ roscd roscpp
kuo@kuo-Inspiron-7420:/opt/ros/indigo/share/roscpp$ pwd
/opt/ros/indigo/share/roscpp
```
## 3.3 *rosls* :显示问价夹内容
```
kuo@kuo-Inspiron-7420:~/.ros/log$ rosls roscpp_tutorials
cmake  launch  package.xml  srv
```

# 4.创建ROS程序包
Description: 本教程介绍如何使用 *roscreate-pkg* 或 *catkin* 创建一个新程序包,并使用 *rospack* 查看程序包的依赖关系。
#### 4.1 本教程中我们将会用到ros-tutorials程序包，请先安装： 
```
kuo@kuo-Inspiron-7420:~/catkin_ws/src$ sudo apt-get install ros-indigo-ros-tutorials
```
#### 4.2 现在使用 *catkin_create_pkg* 命令来创建一个名为'beginner_tutorials'的新程序包，这个程序包依赖于std_msgs、roscpp和rospy： 
```
kuo@kuo-Inspiron-7420:~/catkin_ws/src$ cd ~/catkin_ws/src
kuo@kuo-Inspiron-7420:~/catkin_ws/src$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

# 5.编译ROS程序包
```
kuo@kuo-Inspiron-7420:~/catkin_ws/src$ cd ~/catkin_ws/
kuo@kuo-Inspiron-7420:~/catkin_ws$ catkin_make
```


# 6.运行ros
```
kuo@kuo-Inspiron-7420:~/catkin_ws$ roscore
```
# 7.打开 turtlesim 窗口

运行turtlesim包中的 turtlesim_node节点：

```bash
kuo@kuo-Inspiron-7420:~$ rosrun turtlesim turtlesim_node
[ INFO] [1506515421.527660398]: Starting turtlesim with node name /turtlesim
[ INFO] [1506515421.540510002]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
```
如果出现 turtlesim 窗口，就表示文件创建成功！
![这里写图片描述](http://img.blog.csdn.net/20170927203643496?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)




# bug时间：环境变量设置问题
```
. ~/catkin_ws/devel/setup.bash
```
解决 *roscd beginner_tutorials* 没有此文件的问题
```
kuo@kuo-Inspiron-7420:~$ roscd beginner_tutorials
roscd: No such package/stack 'beginner_tutorials'
kuo@kuo-Inspiron-7420:~$ . ~/catkin_ws/devel/setup.bash
kuo@kuo-Inspiron-7420:~$ roscd beginner_tutorials
kuo@kuo-Inspiron-7420:~/catkin_ws/src/beginner_tutorials$ 
```

来自：初级教程1-4：http://wiki.ros.org/cn/ROS/Tutorials
