---
layout: '[post]'
title: python基础（1）：安装
date: 2017-07-26 10:51:57
tags: [python]
categories: [python基础]
---
# Windows 安装

 请到[官网](https://www.python.org/)下载需要的版本的安装包， 下载所需(注意自己的系统是32位还是64位)，安装路径最好选择默认, 不然对于新手容易出现各种问题。

Windows 安装附加要点: 
设置环境变量: 
1.找到安装路径,默认
>C:\Users\你的用户名\AppData\Local\Programs\Python\Python35-32 粘贴路径 

2.我的电脑 - 属性-高级-环境变量-系统变量中的PATH为（复制路径）:
>C:\Users\**你的用户名**\AppData\Local\Programs\Python\Python35-32;

3.设置环境变量:
>C:\Users\你的用户名\AppData\Local\Programs\Python\Python35-32\Scripts;

# 检测安装是否成功？

打开idle, print(1) 如果系统输出1,则表明安装成功.

> 
  ">>>print(1)
      1
  ">>>
>


# 运行Python

　　安装成功后，打开命令提示符窗口，敲入python后，会出现：
![此处输入图片的描述][1]

　　看到上面的画面，就说明Python安装成功！

　　你看到提示符`>>>`就表示我们已经在Python交互式环境中了，可以输入任何Python代码，回车后会立刻得到执行结果。
　　现在，输入exit()并回车，就可以退出Python交互式环境（直接关掉命令行窗口也可以）。


  [1]: https://www.liaoxuefeng.com/files/attachments/001446601591019cbba6e698d32429bb4754753d86e286a000/l