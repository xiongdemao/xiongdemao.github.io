title: E_ 软件包 ffmpeg 没有可供安装的候选者
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
# 问题：
```
kuo@kuo-Inspiron-7420:~/OpenCV/opencv/release$ sudo apt-get install libopencv-dev build-essential checkinstall cmake pkg-config yasm libtiff4-dev libjpeg-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev python-dev python-numpy libtbb-dev libqt4-dev libgtk2.0-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils ffmpeg
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
现在没有可用的软件包 ffmpeg，但是它被其它的软件包引用了。
这可能意味着这个缺失的软件包可能已被废弃，
或者只能在其他发布源中找到

E: 软件包 ffmpeg 没有可供安装的候选者
```
# 解决：

E: 软件包 gstreamer0.10-ffmpeg 没有可供安装的候选者

好吧，装不上来，因为在Ubuntu上gstreamer0.10-ffmpeg属于额外的版权受限程序，gstreamer0.10-ffmpeg包可通过一些第三方的PPA源，但是
所有您需要做的就是添加PPA到您的系统，更新本地存储索引和安装gstreamero.10-ffmpeg包。如下输入命令：

sudo add-apt-repository ppa:mc3man/trusty-media
sudo apt-get update
sudo apt-get install gstreamer0.10-ffmpeg

这样终于安装上来，测试来下程序也没有解码的问题。
