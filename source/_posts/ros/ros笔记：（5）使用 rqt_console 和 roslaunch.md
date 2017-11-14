---
title: ros笔记：（5）使用 rqt_console 和 roslaunch
date: 2017-11-13 21:08:05
tags: [ros]
categories: [ros]
---
Description: 本教程介绍如何使用rqt_console和rqt_logger_level进行调试，以及如何使用roslaunch同时运行多个节点。

 

## 预先安装rqt和turtlesim程序包

本教程会用到rqt 和 turtlesim这两个程序包，如果你没有安装，请先安装：
```
$ sudo apt-get install ros-<distro>-rqt ros-<distro>-rqt-common-plugins ros-<distro>-turtlesim
```
请使用ROS发行版名称(比如 electric、fuerte、groovy、hydro或最新的indigo)替换掉<distro>。

注意： 你可能已经在之前的某篇教程中编译过rqt和turtlesim，如果你不确定的话重新编译一次也没事。

## 使用rqt_console和rqt_logger_level

rqt_console属于ROS日志框架(logging framework)的一部分，用来显示节点的输出信息。rqt_logger_level允许我们修改节点运行时输出信息的日志等级（logger levels）（包括 DEBUG、WARN、INFO和ERROR）。

现在让我们来看一下turtlesim在rqt_console中的输出信息，同时在rqt_logger_level中修改日志等级。在启动turtlesim之前先在另外两个新终端中运行rqt_console和rqt_logger_level：
```
$ rosrun rqt_console rqt_console
```
```
$ rosrun rqt_logger_level rqt_logger_level
```
你会看到弹出两个窗口：

![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=rqt_console%28start%29.png)

![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=rqt_logger_level.png)

现在让我们在一个新终端中启动turtlesim：
```
$ rosrun turtlesim turtlesim_node
```
因为默认日志等级是INFO，所以你会看到turtlesim启动后输出的所有信息，如下图所示：

![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=rqt_console%28turtlesimstart%29.png)

现在让我们刷新一下rqt_logger_level窗口并选择Warn将日志等级修改为WARN，如下图所示：

![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=rqt_logger_level%28error%29.png)

现在我们让turtle动起来并观察rqt_console中的输出
hydro版：
```
rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 0.0]'
```
![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=rqt_console%28turtlesimerror%29.png)
### 日志等级说明

日志等级按以下优先顺序排列：
```
Fatal
Error
Warn
Info
Debug
```
Fatal是最高优先级，Debug是最低优先级。通过设置日志等级你可以获取该等级及其以上优先等级的所有日志消息。比如，将日志等级设为Warn时，你会得到Warn、Error和Fatal这三个等级的所有日志消息。

现在让我们按Ctrl-C退出turtlesim节点，接下来我们将使用roslaunch来启动多个turtlesim节点和一个模仿节点以让一个turtlesim节点来模仿另一个turtlesim节点。

### 使用roslaunch

roslaunch可以用来启动定义在launch文件中的多个节点。

用法：
```
$ roslaunch [package] [filename.launch]
```
先切换到beginner_tutorials程序包目录下：
```
$ roscd beginner_tutorials
```
如果roscd执行失败了，记得设置你当前终端下的ROS_PACKAGE_PATH环境变量，设置方法如下：
```
$ export ROS_PACKAGE_PATH=~/<distro>_workspace/sandbox:$ROS_PACKAGE_PATH
$ roscd beginner_tutorials
```
如果你仍然无法找到beginner_tutorials程序包，说明该程序包还没有创建，那么请返回到ROS/Tutorials/CreatingPackage教程，并按照创建程序包的操作方法创建一个beginner_tutorials程序包。

然后创建一个launch文件夹：
```
$ mkdir launch
$ cd launch
```
### Launch 文件


现在我们来创建一个名为turtlemimic.launch的launch文件并复制粘贴以下内容到该文件里面：


```
<launch>

  <group ns="turtlesim1">
    <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
  </group>

  <group ns="turtlesim2">
    <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
  </group>

  <node pkg="turtlesim" name="mimic" type="mimic">
    <remap from="input" to="turtlesim1/turtle1"/>
    <remap from="output" to="turtlesim2/turtle1"/>
  </node>

</launch>
```
### Launch 文件解析

现在我们开始逐句解析launch xml文件。

```

    <launch>
```
在这里我们以launch标签开头以表明这是一个launch文件。

```

   3   <group ns="turtlesim1">
   4     <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
   5   </group>
   6 
   7   <group ns="turtlesim2">
   8     <node pkg="turtlesim" name="sim" type="turtlesim_node"/>
   9   </group>
```
在这里我们创建了两个节点分组并以'命名空间（namespace)'标签来区分，其中一个名为turtulesim1，另一个名为turtlesim2，两个组里面都使用相同的turtlesim节点并命名为'sim'。这样可以让我们同时启动两个turtlesim模拟器而不会产生命名冲突。

```

  11   <node pkg="turtlesim" name="mimic" type="mimic">
  12     <remap from="input" to="turtlesim1/turtle1"/>
  13     <remap from="output" to="turtlesim2/turtle1"/>
  14   </node>
```
在这里我们启动模仿节点，并将所有话题的输入和输出分别重命名为turtlesim1和turtlesim2，这样就会使turtlesim2模仿turtlesim1。

```

  16 </launch>
```
这个是launch文件的结束标签。

### roslaunching

现在让我们通过roslaunch命令来启动launch文件：
```
$ roslaunch beginner_tutorials turtlemimic.launch
```
现在将会有两个turtlesims被启动，然后我们在一个新终端中使用rostopic命令发送速度设定消息：


hydro版：
```
$ rostopic pub /turtlesim1/turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'
```
你会看到两个turtlesims会同时开始移动，虽然发布命令只是给turtlesim1发送了速度设定消息。

![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=mimic.png)

我们也可以通过rqt_graph来更好的理解在launch文件中所做的事情。运行rqt并在主窗口中选择rqt_graph：
```
$ rqt
```
或者直接运行：
```
$ rqt_graph
```
![这里写图片描述](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch?action=AttachFile&do=get&target=mimiclaunch.jpg)

到此，我们算是已经学会了rqt_console和roslaunch命令的使用，接下来我们开始学习使用rosed——ROS中的编辑器。现在你可以按Ctrl-C退出所有turtlesims节点了，因为在下一篇教程中你不会再用到它们。 


转自：http://wiki.ros.org/cn/ROS/Tutorials/UsingRqtconsoleRoslaunch
