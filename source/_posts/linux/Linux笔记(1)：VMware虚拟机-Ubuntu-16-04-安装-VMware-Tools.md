---
title: Linux笔记(1)：VMware虚拟机 Ubuntu 16.04 安装 VMware Tools
date: 2017-08-06 20:33:30
tags: [笔记,WMware,Ubuntu]
categories: [Ubuntu] #Linux笔记(1)：VMware虚拟机 Ubuntu 16.04 安装 VMware Tools
---
在VMware中装完Ubuntu后，为了与host之间互传文件，需要安装VMware tools。
## 1、解决路径问题
在“虚拟机设置”下修改“CD/DVD(SATA)”路径（以下关于linux.iso的说明为我的猜想，还未找到明确的说明），否则会出现如下提示：


```
客户机操作系统已将 CD-ROM 门锁定，并且可能正在使用 CD-ROM，这可能会导致客户机无法识别介质的更改。
如果可能，请在断开连接之前从客户机内部弹出 CD-ROM。确实要断开连接并覆盖锁定设置吗?
```

解决：
一般刚安装完linux虚拟机时，这个路径指向的是iso安装文件，比如Ubuntu-16.04-desktop-amd64.iso。
在安装VMware Tools时，需要修改指向VMware Tools所在（VMware Workstation\linux.iso），
在这个路径下有个linux.iso文件，其中提供了linux操作系统平台需要的一些工具文件，当然包括VMware Tools安装文件。
为方便，我直接将安装目录下的linux.iso拷贝到 E:\Linuxidc.com虚拟机\Ubuntu16.04 目录下。

参考：[安装 VMware Tools 时报 客户机操作系统已将 CD-ROM 门锁定，并且可能正在使用CD-ROM](http://www.linuxidc.com/Linux/2016-04/130806.htm)

## 2.安装tools


现在再开始进入系统后，在VMware菜单栏找到安装虚拟工具的时候，它会弹出一个文件夹，里面就有VMware Tools的安装包。
然后我们直接把WMwareTools拷贝出来到桌面吧

然后打开终端解压

命令：tar -xzvf  VMwareTools-10.0.6-3595377.tar.gz
进入解压后的目录，执行：sudo ./wmware-install.pl  然后就一直回车了。
Ubuntu会进行的很顺利，而其他发行版却未必。一直回车到底，到最后提示成功，reboot就可以了。
现在你可以在虚拟机与实体机之间自由复制文件了。

参考：[VMware虚拟机 Ubuntu 16.04 安装 VMware Tools](http://www.linuxidc.com/Linux/2016-04/130807.htm)

---

参考：[Installing VMware Tools in an Ubuntu virtual machine (1022525)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1022525)
      [成功安装VMware Tools!](http://jingyan.baidu.com/article/a17d52851ab9f98099c8f262.html)



