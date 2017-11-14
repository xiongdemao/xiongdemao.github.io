---
title: python：（3）使用list和tuple
date: 2017-09-13 20:58:04
tags: [python]
categories: [python]
---
# 1.list
新建列表；
```python
>>> classmates = ['Michael','Bob','Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
```
看长度及查询：
```python
>>> len(classmates)
3
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[-1]
'Tracy'
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'
```
增：
```python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
>>> classmates.insert(1,'Jack')
>>> classmates
['Michael', 'Jack','Bob', 'Tracy', 'Adam']
```
删：
```python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael','Jack', 'Bob', 'Tracy']
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael','Bob', 'Tracy']
```
改：
```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```
嵌套：
```python
>>> L = ['Aplle',123,True]
>>> L
['Aplle', 123, True]
>>> s = ['Python','java',L,'scheme']
>>> s
['Python', 'java', ['Aplle',123,True], 'scheme']
>>> len(s)
4
>>> s[2]
['Aplle',123,True]
>>> s[2][1]
123
```
# 2.tuple
## 2.1
另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改.
```python
>>> c = ('Micheal','Bob','ssss')
>>> c.pop()
Traceback (most recent call last):
  File "<pyshell#44>", line 1, in <module>
    c.pop()
AttributeError: 'tuple' object has no attribute 'pop'
```


## 2.2
但是，要定义一个只有1个元素的tuple，如果你这么定义：
```python
>>> t = (1)
>>> t
1
```
定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。

所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：
```python
>>> t = (1,)
>>> t
(1,)
```
Python在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。

## 2.3
最后来看一个“可变的”tuple：
```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```
2.4
[廖雪峰教程练习:](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014316724772904521142196b74a3f8abf93d8e97c6ee6000)

练习：
```python
请用索引取出下面list的指定元素：

# -*- coding: utf-8 -*-

L = [
    ['Apple', 'Google', 'Microsoft'],
    ['Java', 'Python', 'Ruby', 'PHP'],
    ['Adam', 'Bart', 'Lisa']
]
# 打印Apple:
print(?)
# 打印Python:
print(?)
# 打印Lisa:
print(?)
```
答：
```python
>>> L = [['Apple','Google','Microsoft'],['Java','Python','Ruby','PHP'],'Adam','Bart','Lisa']
>>> L
[['Apple', 'Google', 'Microsoft'], ['Java', 'Python', 'Ruby', 'PHP'], 'Adam', 'Bart', 'Lisa']
>>> print(L[0][1])
Google
>>> print(L[0][0])
Apple
>>> print(L[0][1])
Google
>>> print(L[4])
Lisa
```













