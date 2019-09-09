# Python-常用数据类型列表、字典和集合的使用

* <a href="#列表">列表：增、删、改、查、初始化</a>

* <a href="#字典">字典：增、删、改、查</a>

* <a href="#集合">集合：增、删、改、查</a>

* <a href="#字符串">字符串：增、删、改、查</a>

  快捷方式：Ctrl + HOME 快速定位页首，点击 锚点 定位

****
<a name="列表"></a>

## 列表

* **面：有一个 list 如何把 list 的元素映射到 function 中？**
* **答**：Python 中内置的四种函数，filter、map、zip、reduce。
* **其他函数**：bisect、

```python
li = [0,1,2,3,4,5,6,7,8,9] #定义一个列表，共十个数
```
**注意：一般变量调用内置函数时，是无返回值的（连空都不是），不能在左边用 = 接收。**
* **增**

```python
li.append(10) #末尾添加一个元素
li.extend([11,12,13]) #末尾可以扩展一个列表，或者直接使用 li += [11,12,13]，效果相同
li.insert(i,x) # i = 0 是下标，x = -1 是元素，在首部插入 -1
li[0:0] = [-4,-3,-2] #可以在指定位置 0 的前面插入多个值
```

* **删**

```python
li.pop(i) #不写默认最后一个pop()，否则删除第 i 个值
li.remove(5) #删除某个数，此前可用 if 5 in li: 判断 li 中是否存在 5，存在多个会删除最前面那个
li.[0:5] = [] #删除前 5 个值，0 可写可不写 [:5] 也可
li.clear() #清空 li
```

* **改**

```python
li[0] = 0 #可重新赋值
li[0],li[-1] = li[-1],li[0] #首尾交换
li[0:10:2] = [0] * len(li[0:10:2]) #将奇数全部致 0
import bisect #将某个值，插入已经排好序的列表中，并返回下标
index = bisect.bisect(li,5,lo=0, hi=len(li)) #返回插入位置的下标 6
#请问one是多少？ 答：[]，one = two 是赋值了引用，如果 one = two[:]，one 就不会是空。
one = [];two = [1,2,3];one = two;two.clear()
```

* **查**

```python
li.count(x) #输出 x 有多少个。
li[-1] #可以倒序访问
li[:5] #输出前 5 个值
li[5:] #输出第 6 个到最后一个值
li[::2] #输出从 0 开始的，所有奇数位值 [0, 2, 4, 6, 8]
li[::-1] #倒序输出
```

* **初始化列表**

```python
li = [x for x in range(10)] #列表表达式初始化 [0-9]
li = [x for x in range(10) if x%2 == 0] #过滤出所有偶数 
li = [0]*10 #初始化为 10 个 0，但是不能用于二维列表定义 [[0]*10]*10，这样会出现浅拷贝现象
li = [0 for _ in range(10)]*10 #二维数组定义，不可用两个乘号
li = list('abc') #快速将字符串转换为列表 ['a', 'b', 'c'] 
```
****
<a name="字典"></a>
## 字典
dict 的第一个特点是查找速度快，无论 dict 有 10 个元素还是 10 万个元素，查找速度都一样。而 list 的查找速度随着元素增加而逐渐下降。不过 dic t的查找速度快不是没有代价的，dict 的缺点是占用内存大，还会浪费很多内容，list 正好相反，占用内存小，但是查找速度慢。由于 dict 是按 key 查找，所以，在一个 dict 中，key 不能重复。dict 的第二个特点就是存储的 key-value 序对是没有顺序的！这和 list 不一样。

```python
dic = {'Alice':21,'Bob':22,'Clike':23} #定义三个键值对
newDic = {'num':100, 'string':'hello', 'li':[1,2,3]} #可存储多种类型
```
* **增**

```python
if 'Dom' not in dic.keys(): #如果没有则新增一个
	dic['Dom'] = 24 
ones = {'E':25, 'F':26, 'G':27}
dic.update(ones) #增加多个
string = 'aaabbbccc'
import collections
counter = collections.Counter(string) #将字符串 string 统计成字典 {字符：数量}
counter.most_common() #按 值 从大到小 返回 dic 中的对 [('a', 3), ('b', 3), ('c', 3)]
dic = dict(counter) #注意：这里返回的是Counter对象，再转换一下成字典，但是字典没啥函数调用
```

* **删**

```python
del dic['Alice'] #删除
dic.pop('Alice') #删除并返回 21
dic.clear() #清空
```

* **改**

```python
dic['Alice'] = 20 #直接修改
```

* **查**

```python
dic.keys() #查看所有的键，返回一个可迭代的对象，可以用 for x in dic.keys() 获取，不能直接操作。
dic.values() #查看所有的值，也是一个可迭代的对象
dic.get('Eli', 0) #找不到也不会报错，还会返回 0
for k,v in dic.items(): #注意这里要有 items() 函数
    print('key:',k,'value:',v)
li = sorted(dic.keys()) #返回一个排好序的关键字 列表
```
****
<a name="集合"></a>
## 集合

set 持有一系列元素，这一点和 list 很像，但是 set 的元素没有重复，而且是无序的，这点和 dict 的 key 很像。set 的内部结构和 dict 很像，唯一区别是不存储 value，因此，判断一个元素是否在 set 中速度很快。**set 存储的元素和 dict 的 key 类似，必须是不变对象，因此，任何可变对象是不能放入 set 中的。**

**内部实现原理**： set 的去重是通过两个函数 __ hash __  和 __ eq __ 结合实现的。

1. 当两个变量的哈希值不相同时，就认为这两个变量是不同的 。
2. 当两个变量哈希值一样时，调用 __ eq __ 方法，当返回值为 True 时认为这两个变量是同一个，应该去除一个。返回False 时，不去重。

```python
st = {1,2,3,4,5} #里面存储是乱序的
```
* **增**

```python
st.add(x) #添加一个元素
```

* **删**

```python
st.remove(x) #删除一个元素
st.discard(x) #删除 x，不存在不会报错
st.clear() #清空
```

* **改 + 集合操作**

```python
dic['Alice'] = 20 #直接修改
a = {'a', 'b', 'r', 'd', 'c'} 
b = {'a', 'z', 'c', 'l', 'm'}
a - b #集合 a 中去除 b 的元素
a | b #并集
a & b #交集
a ^ b #不同时包含于 a 和 b 的元素
```

* **查**

```python
for x in set(li):#统计不同数的个数
    temp[x] = li.count(x)
```
****
<a name="字符串"></a>
## 字符串
在Python 3.x中，字符串统一默认为以unicode，不用再加前缀 u，要以字节存储则必须加前缀 b。b 即 byte ， 3.x 里要加标注才会识别为字节。
```python
#输入
name = input('请输入你的姓名：') #输入函数，默认字符串
li = [int(x) for x in input().split()] #输入一串空格隔开的整数，保存包列表中
li = map(int, input().split())

#输出
print('{} {}'.format('hello','Python')) #可以在大括号中添加数字，不加默认从 0 开始。
print('编号：%d 价格：%d'%(item, price)) #类似 C 语言的输出
print('hello', end='') #输出不带回车符
print(r'\abc') #Python中字符串前面加上 r 表示原生字符串，数据里面的反斜杠不需要进行转义，针对的只是反斜杠
```
* **增**

```python
string = 'lo' + 've' #合并两个字符串
```

* **删**

```python
string.strip() #清除两边的空格
```

* **改：replace、title、lower、upper、reversed、isalpha、isdigit、islower、isupper**

```python
#字符串是不可更改的，一般使用函数返回更改后的值，但是原始字符串没有改变，牢记！！！

'hello python'.title() #单词首字母大写 Hello Python
'HELLO PYTHON'.lower() #将单词转换为小写 hello python
'hello python'.upper() #将单词转换为大写 HELLO PYTHON

S = 'hello Tom'; S = S[:6] + 'Jerry' #通过切片和重新赋值的方法。
S = S.replace('m', 'M') #将 S 中的 小 m 换成，大 M  
li = [str(x) for x in li]; S = ''.join(li) #通过两步，将列表转为字符串

string.isalpha() #判断 string 是否都是字母
string.isdigit() #判断 string 是否都是数字
string.islower() #判断 string 是否都是小写字母
string.isupper() #判断 string 是否都是大写字母

num = ord('a') #将字符转为数字，只能输入一个字符
char = chr(97) #将数字转为字符

reversed('abc') #返回一个可迭代的地址，我猜根本没有倒转，在你输出的时候才真正倒着输出。

```

* **查**

```python
S = find(subStr) #返回最左边出现的位置，找不到返回 -1
S = rfind(subStr) #返回最右边出现的位置，找不到返回 -1
if subStr in S: #返回 bool 值
```

* 正则表达式








