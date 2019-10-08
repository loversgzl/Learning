# Python-基础语法

**Python 简介**

- **Python 是一种解释型语言** ：这意味着开发过程中没有了编译这个环节。
- **Python 是面向对象语言**: 这意味着Python支持面向对象的风格或代码封装在对象的编程技术。
- **Python 是开源的，最大的优势之一是丰富的库**，所以使用方向广泛，因为机器学习的热浪，开始在程序员间展露头角，可以进行 web 开发、爬虫、数据科学（包括机器学习、数据分析和数据可视化），还是跨平台的，在UNIX，Windows 和 MacOS 兼容很好。

**Python 语法基础**

```python
#查看 Python 的所有关键字，在以后的练习中要做到对每个都很熟悉
import keyword
print(keyword.kwlist)
"""
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
"""

#注释
#单行注释
'''
多行注释，三个单双引号都可以。
'''
```

**基本数据类型**

* 基本数据类型：数字、字符串（str）、列表（list）、元组（tuple）、集合（set）、字典（dict），前三个类型不可更改，具体可参考[我的博客](https://blog.csdn.net/qq_29611345/article/details/99693298)。
* Python中的变量不需要声明，每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

```python
# / 表示浮点数除法，// 表示整数除法，所以 Python 的单行注释是 # 。
z = x / y 

#使用 type（val） 函数查看变量类型。

#字符串，使用 '' 或者 "" 都是可以的，没有字符类型，一个字符就是长度为 1 的字符串。
#字符串不可修改，类似 word[0] = 'H'，都是错误的。
word = 'hello'
print('str' + 'ing', 'my'*3)

#元组，不可修改，可以存放多种类型
tup = (11, 'hello', 22)

#列表，不同于数组，里面存放的值可以是多种类型。
li = [11, 'hello', 25]

#集合，一个无序不重复的集合，可以利用 set 去重。
student = {'tom', 'jerry', 'mary'}

#字典, key - value 对
dic = {‘key’:'value', 11:22, 'id':10}

```



**基本控制语句**

```python
#Python 变量不用提前声明，随用随写，它会根据赋值的类型自动判断。
a,b,c,d = 10,'hello',[1,2,3],True

#Python 是使用 缩进 来表示代码块的，而不是使用 { }，一般使用 四个空格 表示一个缩进，末尾不要加 分号。
#条件控制
if a > b: #注意条件后面要加冒号，代码块拥有相同的缩进。
    print("YES")
    print("YES")
elif a < b:
    print('NO')
    print('NO')
else:
    print('equal')

#循环语句
while True:
    print("这是死循环")

for i in range(10):#range 是一个可迭代的函数，循环输出 0-9。
    print(i)

#函数声明，不用定义返回值，无返回值默认返回空。
def area(width, height):
    return width * height
```

* **编程常用语法**
```python
#输入一串数据，空格分隔，转换为整数如： 1 2 3 4 5
li = list(map(int, input().split())) #split 默认以空格分隔，输入默认是字符串
li = [int(x) for x in input().split()] #这种也可以，效果相同
#li = [1,2,3,4,5]

import collections
counter = dict(collections.Counter(string)) #将字符串 string 统计成 字典 {字符：数量},需要转换
counter.most_common() #输出：[('a', 3), ('b', 2), ('c', 1)] 按值从大到小 返回 dic 中的对 

```

**文件输入输出流**
```python
import os
os.chdir('F:')
with open('test.txt') as test:
	count = 0
	for ch in test.read():
		if ch.isupper():#统计大写字母
			count += 1
	print(count)
```