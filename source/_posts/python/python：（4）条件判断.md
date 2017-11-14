---
title: python：（4）条件判断
date: 2017-09-18 21:23:21
tags: [python]
categories: [python]
---
# 例子：
```python
birth = input('birth:')
if int(birth) < 2000:
    print('00前')
else:
    print('00后')
```
```python
birth:1993
00前
>>> 
```
# 练习：

小明身高1.75，体重80.5kg。请根据BMI公式（体重除以身高的平方）帮小明计算他的BMI指数，并根据BMI指数：

低于18.5：过轻
18.5-25：正常
25-28：过重
28-32：肥胖
高于32：严重肥胖
用if-elif判断并打印结果：
```python
high = 1.75
weight = 80.5
BMI = high + weight
if BMI < 18.5:
    print('过轻')
elif BMI <= 25:
    print('正常')
elif BMI <= 28:
    print('过重')
elif BMI <=32:
    print('肥胖')
else:
    print('严重肥胖')
```
```python
====================== RESTART: E:\python_files\test.py ======================
严重肥胖
>>> 
```