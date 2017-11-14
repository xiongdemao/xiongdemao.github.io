---
title: ros：（3）ROS话题_『ROS Topics』『rqt_graph』『ROS Messages』『rqt_plot』
date: 2017-11-13 21:08:01
tags: [ros]
categories: [ros]
---
# 1.开始
## 1.1 roscore
首先确保roscore已经运行, 打开一个新的终端：
```
$ roscore
```
## 1.2turtlesim

在本教程中我们也会使用到turtlesim，请在一个新的终端中运行：
```
$ rosrun turtlesim turtlesim_node
```
## 1.3通过键盘远程控制turtle

我们也需要通过键盘来控制turtle的运动，请在一个新的终端中运行：
```
$ rosrun turtlesim turtle_teleop_key
```
## 1.4使用 rqt_graph

rqt_graph能够创建一个显示当前系统运行情况的动态图形。rqt_graph是rqt程序包中的一部分。如果你没有安装，请通过以下命令来安装：
```
    $ sudo apt-get install ros-indigo-rqt
    $ sudo apt-get install ros-indigo-rqt-common-plugins
```
在一个新终端中运行:
```
$ rosrun rqt_graph rqt_graph
```
就会弹出`rqt_graph`窗口

![这里写图片描述](http://img.blog.csdn.net/20170927222157598?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


# 2.『ROS Topics』

如上图所示：
turtlesim_node节点和turtle_teleop_key节点之间是通过一个ROS话题来互相通信的。turtle_teleop_key在一个话题上发布按键输入消息，而turtlesim（这里是my_turtle）则订阅该话题以接收该消息。

## 2.1.rostopic介绍

rostopic命令工具能让你获取有关ROS话题的信息。 
### 2.1.1你可以使用帮助选项查看rostopic的子命令：
```
$ rostopic -h
```
```
    rostopic bw     display bandwidth used by topic
    rostopic echo   print messages to screen
    rostopic hz     display publishing rate of topic
    rostopic list   print information about active topics
    rostopic pub    publish data to topic
    rostopic type   print topic type
```
### 2.1.2rostopic echo可以显示在某个话题上发布的数据
让我们在一个新终端中看一下turtle_teleop_key节点在 /turtle1/cmd_vel话题上发布的数据。
```
$rostopic echo /turtle1/cmd_vel
```
你可能看不到任何东西因为现在还没有数据发布到该话题上。接下来我们通过按下方向键使turtle_teleop_key节点发布数据。记住如果turtle没有动起来的话就需要你重新选中turtle_teleop_key节点运行时所在的终端窗口。

现在当你按下向上方向键时应该会看到下面的信息： 
```
linear: 
  x: 2.0
  y: 0.0
  z: 0.0
angular: 
  x: 0.0
  y: 0.0
  z: 0.0
---
linear: 
  x: 0.0
  y: 0.0
  z: 0.0
angular: 
  x: 0.0
  y: 0.0
  z: 2.0
---
linear: 
  x: 0.0
  y: 0.0
  z: 0.0
angular: 
  x: 0.0
  y: 0.0
  z: -2.0
---

```
现在让我们再看一下rqt_graph（你可能需要刷新一下ROS graph）。正如你所看到的，rostopic echo(红色显示部分）现在也订阅了turtle1/command_velocity话题。 

![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_graph_echo.png)

## 2.1.3使用 rostopic list

rostopic list能够列出所有当前订阅和发布的话题。

让我们查看一下list子命令需要的参数，在一个新终端中运行：
```
$ rostopic list -h

    Usage: rostopic list [/topic]

    Options:
      -h, --help            show this help message and exit
      -b BAGFILE, --bag=BAGFILE
                            list topics in .bag file
      -v, --verbose         list full details about each topic
      -p                    list only publishers
      -s                    list only subscribers
```
在rostopic list中使用verbose选项：
```
$ rostopic list -v
```
这会显示出有关所发布和订阅的话题及其类型的详细信息。
```
    Published topics:
     * /turtle1/color_sensor [turtlesim/Color] 1 publisher
     * /turtle1/command_velocity [turtlesim/Velocity] 1 publisher
     * /rosout [roslib/Log] 2 publishers
     * /rosout_agg [roslib/Log] 1 publisher
     * /turtle1/pose [turtlesim/Pose] 1 publisher

    Subscribed topics:
     * /turtle1/command_velocity [turtlesim/Velocity] 1 subscriber
     * /rosout [roslib/Log] 1 subscriber
```
# 3.ROS Messages

话题之间的通信是通过在节点之间发送ROS消息实现的。对于发布器(turtle_teleop_key)和订阅器(turtulesim_node)之间的通信，发布器和订阅器之间必须发送和接收相同类型的消息。这意味着话题的类型是由发布在它上面的消息类型决定的。使用rostopic type命令可以查看发布在某个话题上的消息类型。

## 3.1使用 rostopic type

rostopic type 命令用来查看所发布话题的消息类型。

用法：
```
rostopic type [topic]
```

hydro版请运行：
```
$ rostopic type /turtle1/cmd_vel
```
    你应该会看到:
```
    geometry_msgs/Twist
```
我们可以使用rosmsg命令来查看消息的详细情况：

hydro版：
```
$ rosmsg show geometry_msgs/Twist
```
```
    geometry_msgs/Vector3 linear
      float64 x
      float64 y
      float64 z
    geometry_msgs/Vector3 angular
      float64 x
      float64 y
      float64 z
```
现在我们已经知道了turtlesim节点所期望的消息类型，接下来我们就可以给turtle发布命令了。

# 4.继续学习 rostopic

现在我们已经了解了什么是ROS的消息，接下来我们开始结合消息来使用rostopic。

## 4.1使用 rostopic pub

rostopic pub可以把数据发布到当前某个正在广播的话题上。

用法：
```
rostopic pub [topic] [msg_type] [args]
```

示例（hydro版)：
```
$ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
```
以上命令会发送一条消息给turtlesim，告诉它以2.0大小的线速度和1.8大小的角速度开始移动。
![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=turtle%28rostopicpub%29.png)
这是一个非常复杂的例子，因此让我们来详细分析一下其中的每一个参数。
```
    rostopic pub
```
        这条命令将会发布消息到某个给定的话题。 
```
     -1
```
        （单个破折号）这个参数选项使rostopic发布一条消息后马上退出。 
```
    /turtle1/command_velocity
```
        这是消息所发布到的话题名称。 
```
    turtlesim/Velocity
```
        这是所发布消息的类型。 
```
    --
```
    （双破折号）这会告诉命令选项解析器接下来的参数部分都不是命令选项。这在参数里面包含有破折号-（比如负号）时是必须要添加的。
```
    2.0 1.8
```
    正如之前提到的，在一个turtlesim/Velocity消息里面包含有两个浮点型元素：linear和angular。在本例中，2.0是linear的值，1.8是angular的值。这些参数其实是按照YAML语法格式编写的，这在YAML文档中有更多的描述。 

你可能已经注意到turtle已经停止移动了。这是因为turtle需要一个稳定的频率为1Hz的命令流来保持移动状态。我们可以使用rostopic pub -r命令来发布一个稳定的命令流（非hydro版）：
```
$ rostopic pub /turtle1/command_velocity turtlesim/Velocity -r 1 -- 2.0  -1.8
```
hydro版：
```
$ rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
```
这条命令以1Hz的频率发布速度命令到速度话题上。

  ![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=turtle%28rostopicpub%292.png)

我们也可以看一下rqt_graph中的情形，可以看到rostopic发布器节点（红色）正在与rostopic echo节点（绿色）进行通信：

![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_graph_pub.png)

正如你所看到的，turtle正沿着一个圆形轨迹连续运动。我们可以在一个新终端中通过rostopic echo命令来查看turtlesim所发布的数据。

## 4.2使用 rostopic hz

rostopic hz命令可以用来查看数据发布的频率。

用法：
```
rostopic hz [topic]
```
我们看一下turtlesim_node发布/turtle/pose时有多快：
```
$ rostopic hz /turtle1/pose
```
你会看到：
```
    subscribed to [/turtle1/pose]
    average rate: 59.354
            min: 0.005s max: 0.027s std dev: 0.00284s window: 58
    average rate: 59.459
            min: 0.005s max: 0.027s std dev: 0.00271s window: 118
    average rate: 59.539
            min: 0.004s max: 0.030s std dev: 0.00339s window: 177
    average rate: 59.492
            min: 0.004s max: 0.030s std dev: 0.00380s window: 237
    average rate: 59.463
            min: 0.004s max: 0.030s std dev: 0.00380s window: 290
```
现在我们可以知道了turtlesim正以大约60Hz的频率发布数据给turtle。我们也可以结合rostopic type和rosmsg show命令来获取关于某个话题的更深层次的信息（非hydro版）：
```
$ rostopic type /turtle1/command_velocity | rosmsg show
```
hydro版：
```
rostopic type /turtle1/cmd_vel | rosmsg show
```
到此我们已经完成了通过rostopic来查看话题相关情况的过程，接下来我将使用另一个工具来查看turtlesim发布的数据。

# 5.使用 rqt_plot

注意：如果你使用的是electric或更早期的ROS版本，那么rqt命令是不可用的，请使用rxplot命令来代替。

rqt_plot命令可以实时显示一个发布到某个话题上的数据变化图形。这里我们将使用rqt_plot命令来绘制正在发布到/turtle1/pose话题上的数据变化图形。首先，在一个新终端中运行rqt_plot命令：
```
$ rosrun rqt_plot rqt_plot
```
这会弹出一个新窗口，在窗口左上角的一个文本框里面你可以添加需要绘制的话题。在里面输入/turtle1/pose/x后之前处于禁用状态的加号按钮将会被使能变亮。按一下该按钮，并对/turtle1/pose/y重复相同的过程。现在你会在图形中看到turtle的x-y位置坐标图。

![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_plot.png)

按下减号按钮会显示一组菜单让你隐藏图形中指定的话题。现在隐藏掉你刚才添加的话题并添加/turtle1/pose/theta，你会看到如下图所示的图形：
![这里写图片描述](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_plot2.png)
本部分教程到此为止，请使用Ctrl-C退出rostopic命令，但要保持turtlesim继续运行。

到此我们已经理解了ROS话题是如何工作的，接下来我们开始学习理解ROS服务和参数。 



来自：
[理解ROS话题](http://wiki.ros.org/cn/ROS/Tutorials/UnderstandingTopics)
本教程介绍ROS话题（topics）以及如何使用rostopic 和 rqt_plot 命令行工具。 


