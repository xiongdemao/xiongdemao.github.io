---
title: python：（2）数据类型和变量
date: 2017-09-12 17:18:06
tags: [python]
categories: [python]
---
# 1.整数
十六进制用0x前缀和0-9，a-f表示，例如：`0xff00`，`0xa5b4c3d2`，等等。
# 2.浮点数
用科学计数法表示，把10用e替代，`1.23x109`就是`1.23e9`，或者`12.3e8`，`0.000012`可以写成`1.2e-5`，等等。    	
```python
>>> 0xff
255
>>> 1.2e3-1
1199.0
>>> 1.2e-3 + 1
1.0012
```
# 3.字符串
转义字符`\`来标识
```python
>>> print('I\'m ok')
I'm ok
>>> print("I'm ok") # " " 可以圈 '' 
I'm ok
>>> print('I\'m \"ok\"')
I'm "ok"
>>> print('I\'m learning \n Python!')  # \n 表示换行
I'm learning 
 Python!
>>> print('\\ \n \\') # \\ 表示 \ 
\ 
 \
>>> print('\\\n\\')
\
\
>>> print('\\\t\\')  # \t 表示制表（也就是四个Tab空格）
\	\
>>> print(r'\\t\\')
\\t\\
>>> print('''line1
line2
line3''')             #Python允许用 '''...''' 的格式表示多行内容
line1
line2
line3
```

# 4.布尔值
```python
>>> 3>4
False
>>> 3>2
True
>>> True and True
True
>>> True and False
False
>>> 5 > 3 and 5 > 9
False
```
# 5.空值
空值是Python里一个特殊的值，用`None`表示。`None`不能理解为`0`，因为`0`是有意义的，而`None`是一个特殊的空值。

# 6.变量
```python
>>> a = 'ABC'
>>> b = a
>>> a = 'XYZ'
>>> print(b)
ABC
>>> 
```

# 7.常量
## 7.1
　　在Python中，通常用全部大写的变量名表示常量

## 7.2
　　在Python中，有两种除法:
- 一种除法是/：
```python
>>> 10 / 3
3.3333333333333335
```
- `/`除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数：
```python
>>> 9 / 3
3.0
```
- 还有一种除法是//，称为地板除，两个整数的除法仍然是整数：
```python
>>> 10 // 3
3
```
整数的地板除//永远是整数，即使除不尽。要做精确的除法，使用/就可以。

# 8.字符编码
对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：
```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```
如果知道字符的整数编码，还可以用十六进制这么写str：
```python
>>> '\u4e2d\u6587'
'中文'
```
# 9.格式化
在Python中，采用的格式化方式和C语言是一致的，用%实现，举例如下：
```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```
你可能猜到了，%运算符就是用来格式化字符串的。在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略。

常见的占位符有：

| \%d	| 整数|
| \%f	| 浮点数|
| \%s	| 字符串|
| \%x	| 十六进制整数|

其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：
```python
>>> '%2d-%02d' % (3, 1)
' 3-01'
>>> '%.2f' % 3.1415926
'3.14'
```
如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串：
```python
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
```
有些时候，字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：
```python
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
```











