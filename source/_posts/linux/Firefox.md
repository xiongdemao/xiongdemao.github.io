title: Ubuntu 火狐浏览器中，鼠标选择文字被删除的解决办法 
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
# 1. Ubuntu 火狐浏览器中，鼠标选择文字被删除的解决办法 

系统版本 14.04
ibus版本 1.5

系统输入法选择为ibus时会自动清除选中的文本，如果是英文输入法就没有这个问题。

解决方法：
终端中 ibus-setup
勾掉 在应用窗口中启用内嵌编辑模式

#2.Firefox中撤销按钮丢失了
1.新版本把撤销按钮去掉了。。

默认Firefox支持：Ctrl+Shift+T，实现撤销恢复。

2.如果不习惯用快捷键的，则去：

http://mozilla.com.cn/addon/63-undo-closed-tabs-button/

去安装对应插件，然后启用后，自己通过定制，即可加到工具栏中。
