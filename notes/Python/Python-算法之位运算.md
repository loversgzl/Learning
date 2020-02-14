# Python-算法之位运算

1. <a href="#基本概念">基本概念</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

1. 基本概念
python中二进制表示：0b1111
函数：bin(num)
基本操作：与&    或|    非~     异或^    左移<<
1-汉明重量：统计一个数中，1的数量。
python函数：bin(num).count('1')
```python
while (num != 0):
    num &= (num-1)
    cnt += 1
```

2-汉明距离：两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目
解题思路：异或之后，统计1的数量
测试用例：大于等于0，小于等于32位
python函数：bin(x^y).count('1')
```python
def hammingDistance(x,y):
    num = x^y
    cnt = 0
    while num != 0:
        num &= (num-1)
        cnt += 1
    return cnt

```
3-只出现一次的数字I，II,III:
I：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。
II:除了某个元素只出现一次以外，其余每个元素均出现了三次。
解题思路：两次用一位寄存器，三位需要用到两位寄存器。
它是一个逻辑电路，a、b变量中，相同位置上，分别取出一位，
负责完成00->01->10->00，也就是开头的那句话，当数字出现3次时置零。
先带入一位，即a,b,num都是一位，三次出现num为1的时，进行理解即可。不要带入数值，带入一位。
第一次b=1,第二次出现a=1,第三次出现a=b=0.
III:其中恰好有两个元素只出现一次，其余所有元素均出现两次。
解题思路：先全部异或一次, 得到的结果, 结果有1的位置必然是两个数不同位的地方。
用这一点将数分为两堆，分别进行I的解法。
```python
#def One:全部异或即可，虽然简单，但很多题目都是这个的扩展。

def Two(nums):
    a,b = 0,0
    for num in nums:
    	b = ~a & (b ^ num)
    	a = ~b & (a ^ num)
    return b

def Three(nums):
    diff = 0
    for num in nums:
        diff ^= num
    diff &= -diff#可以取出一位不同bit的位置。
    
    sol = [0, 0]
    for num in nums:
        if num & diff:#将数分为两堆。
            sol[0] ^= num
        else:
            sol[1] ^= num
    return sol
```

4-异或：相同为0，不同为1，字母不能异或，只有数字可以。
num = ord(char)
char = chr(num)
经典例题：
One数字的补数：补数是对该数的二进制表示取反。
Two交替位二进制：换句话说，就是他的二进制数相邻的两个位数永不相等，01交替。
```python
#解题思路：找一个2**n大于num的数，然后减一与num异或。
#测试用例：0不是正整数，最小为1.
def One(num):
    n = 1
    while n <= num:
        n <<= 1
    return num^(n-1)

def Two(num):
    temp = n^(n>>1)#判断异或之后是否全为1
    return temp&(temp+1)==0


#判断是否为2的次幂：
num & (num-1)==0
#获取数值的某些二进制位：
num & 0b1111
#获取数值第一个二进制1：
num & -num
```

