# Python-常用函数

**python运行速度**：因为 Python 是动态语言，解释执行，因此 Python 代码运行速度慢。如果要提高 Python 代码的运行速度，最简单的方法是把 **某些关键函数用 C 语言重写**，这样就能大大提高执行速度。同样的功能，StringIO 是纯 Python 代码编写的，而 cStringIO 部分函数是 C 写的，因此 cStringIO 运行速度更快。

函数列表：

* map、zip、filter、reduce

* help、dir

* itertools

  

* **帮助文档：help 和 dir 的使用**
```python
dir(func) #可以查看某个函数的 所有属性 和 方法
help(func) #可以查看该函数的使用方法包括 需要的参数 和 返回值
```



* **无穷大与无穷小**

```python
float('inf') #无穷大
float('-inf') #无穷小
```



* **面：简述 any() 和 all() 方法**

```Python
any(li) #只要迭代器中有一个元素为真就为真
all(li) #迭代器中所有的判断项返回都是真，结果才为真
```





* **global 全局变量** [超详细参考博客](https://blog.csdn.net/qw_sunny/article/details/80972357)



* **Python 中内置的四种函数，map、zip、filter 返回值都是一个可迭代对象，reduce 需要加库文件**

```python
# map(func, li) 函数的用法：将函数作用于列表中的每个值
li = map( lambda x:x*x，[1,2,3]); li = list(li) #返回一个可迭代的对象，需用 list 转换 [1,4,9]
li = map(int,input().split()) #对输入的值进行类型转换

# zip(li,li) 函数的用法：将两个参数的值一一对应成元组，返回一个可迭代对象
tup = list(zip([1,2,3],[4,5,6])) #返回：需用 list 转换，得到一系列元组 [(1, 4), (2, 5), (3, 6)]

# filter(func, li)函数的用法：和 map 函数类似，不过只有返回 True 的值会被保留
li = list(filter(lambda x:x%2,range(10))) #返回：需用 list 转换，得到 [1, 3, 5, 7, 9]

# reduce(func, li)函数的用法：唯一可以使用两个参数的函数
import functools # 需要加库
num = functools.reduce(lambda x,y:x+y, [1,2,3,4,5]) # 返回 15 此函数还可实现阶乘！

```



* **二进制函数**

```python
binary = lambda x: '' if x == 0 else binary(x/2) + str(x%2)
bin(10) #转为二进制，python 中二进制表示0b1010
oct(10) #转为八进制, 0o12
hex(10) #转为十六进制，0xa
int('10', 2); int('10', 8); int('10', 16 ); #将字符串以二、八、十六进制进行转换，返回 2，8，16
#将某个字符串转为相应的值，还是需要使用 int
num = int(bin(0b1010)[2:]) #直接转换为数字 1010
```



* **sorted 函数**

```python
sorted(li) # 返回排序后的列表，但原列表不变。
li.sort() # 返回 None，li 递增排序

# key 关键字一般需要传入一个函数
sorted("This is a test string from Andrew".split(), key=str.lower)
# 输出 ['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']

#如果比较的值是两个值的列表，则下式表示按 第一个值 递增排序，如果相同，按 第二个值 递减排序
li.sort(key = lambda x:(x[0],-x[1])) 

#传入两个参数：2.x版本有cmp参数，现在只有key了，但是加了这个函数模拟以前的功能。
#问：将一个字符串列表，组成字典序最大的串。
from functools import cmp_to_key
li.sort(key=cmp_to_key(lambda s1,s2:s1+s2 if s2+s1<s1+s2 else s2+s1))
''.join(li)
```




* **assert 断言的使用**

```python
if a > 0:
    assert a <= 0 #如果出现不在预期的结果，那么可以主动抛出一个异常
```



****

* **import itertools、import random、import time**

```python
import itertools #该函数返回一个可迭代对象，将 li 里的元素，按 num 数量进行组合，每个组合以元组存储
li = [1,2,3]; num = 2
res = list(itertools.combinations(li,num)) #转为列表 [(1, 2), (1, 3), (2, 3)]

import random
print(random.randint(a,b)) # 返回 [a,b]
print(random.random()) # 返回 [0.0,1.0)

import time
time.localtime() # 字典形式年月日时分秒，可以单独取出
time.ctime() # 输出标准年月日时分秒
time.sleep(x) # 程序阻塞 x 秒
```
[记录程序时间的三种方法：参考博客](https://www.jb51.net/article/118699.htm)



****


* **enumerate、isinstance、eval、reverse**
```python
for index, num in enumerate(li): # 迭代输出 li 的 索引 和 值
    
isinstance(var, str) # 判断 var 是否是字符串，或者 type(var) is str: 效果相同

eval('1+1') # 将字符串当做表达式运算，返回 2

li.reverse() #列表的属性，返回 None
reversed(li) # 返回可迭代对象，原列表不变


```

