---
title: ubuntu_ kdevelop 未指定有效的可执行文件!
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
# 1.ubuntu下载安装 Kdevelop 之后想运行一下例子，却遇到这样的问题：

![这里写图片描述](http://img.blog.csdn.net/20171020221536697?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解决：在最上边一栏【运行】->『配置启动器』中『Add New』->『应用程序』即可

![这里写图片描述](http://img.blog.csdn.net/20171020221920586?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解释：当我们点击『execute』的时候是为了执行刚才『build』之后的代码，但是没有设置是否在『execute』（执行）的同时生成可运行程序（.exe）就会提示：“未指定有效的可执行文件!”


设置之后再『execute』一下，就可以看到久违的“ hello，world！”啦！

![这里写图片描述](http://img.blog.csdn.net/20171020222855302?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)






## 2 .tar.bz2文件解压命令

从网络上下载到的源码包， 最常见的是 .tar.gz 包， 还有一部分是 .tar.bz2包


要解压很简单 ：

```
.tar.gz     格式解压为          tar   -zxvf   xx.tar.gz

.tar.bz2   格式解压为          tar   -jxvf    xx.tar.bz2
```
