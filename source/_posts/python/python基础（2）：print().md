---
layout: '[post]'
title: python基础(2)：print()
date: 2017-07-26 11:51:57
tags: [python]
categories: [python基础]
---


# print 字符串

python 中 print 字符串 要加''或者""
```
>>> print('hello world')
'''
hello world
'''
>>> print("hello world 2")
'''
hello world 2
'''
```
# print 字符串叠加

可以使用 + 将两个字符串链接起来, 如以下代码.
```
>>> print('Hello world'+' Hello Hong Kong')
"""
Hello world Hello Hong Kong
"""
```
# 简单运算

可以直接print 加法+,减法-,乘法*,除法/. 注意：字符串不可以直接和数字相加，否则出现错误。
```
>>> print(1+1)
"""
2
"""
>>> print(3-1)
"""
2
"""
>>> print(3*4)
"""
12
"""
>>> print(12/4)
"""
3.0
"""
>>> print('iphone'+4) #字符串不可以直接和数字相加
"""
Traceback (most recent call last):
  File "<pyshell#10>", line 1, in <module>
    print('iphone'+4)
TypeError: Can't convert 'int' object to str implicitly
"""
```
int() 和 float()；当int()一个浮点型数时，int会保留整数部分,比如 int(1.9),会输出1,而不是四舍五入。
```
>>> print(int('2')+3) #int为定义整数型
"""
5
"""
>>> print(int(1.9))  #当int一个浮点型数时，int会保留整数部分
"""
1
"""
>>> print(float('1.2')+3) #float()是浮点型，可以把字符串转换成小数
""""
4.2
""""

```


