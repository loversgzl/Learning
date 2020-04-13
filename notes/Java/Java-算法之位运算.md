

# Java-算法之位运算



快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

1. <a href="#基本概念">基本概念</a>
<a name="基本概念"></a>
```java
//1-汉明重量：统计一个数中，1的数量。
while (num != 0){
    num &= (num-1)
    cnt += 1
}

//2-链表中逆序二进制转整数
int res = 0;
while(head != null) {
    res = res*2 + head.val;
    head = head.next;
}

```

3-只出现一次的数字I，II,III:
one：给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。
two:除了某个元素只出现一次以外，其余每个元素均出现了三次。
解题思路：两次用一位寄存器，三位需要用到两位寄存器。
它是一个逻辑电路，a、b变量中，相同位置上，分别取出一位，
负责完成00->01->10->00，也就是开头的那句话，当数字出现3次时置零。
先带入一位，即a,b,num都是一位，三次出现num为1的时，进行理解即可。不要带入数值，带入一位。
第一次b=1,第二次出现a=1,第三次出现a=b=0.
three:其中恰好有两个元素只出现一次，其余所有元素均出现两次。
解题思路：先全部异或一次, 得到的结果, 结果有1的位置必然是两个数不同位的地方。
用这一点将数分为两堆，分别进行I的解法。

```java
//One:全部异或即可，虽然简单，但很多题目都是这个的扩展。

public int Two(int[] nums){
    int a=0,b=0;
    for(int num : nums){
       	b = ~a & (b ^ num);
    	a = ~b & (a ^ num);
    } 
    return b;
}

public int Three(int[] nums){
    int diff = 0;
    for(int num : nums){
         diff ^= num;
    }
    diff &= -diff;//可以取出最低位1的位置。
    int[] sol = {0，0};
    for(int num : nums){
       if(num & diff)//将数分为两堆。
            sol[0] ^= num;
        else:
            sol[1] ^= num;
    }
    return sol;
}
```

4-异或：相同为0，不同为1，字母不能异或，只有数字可以。
经典例题：
One数字的补数：补数是对该数的二进制表示取反。
Two交替位二进制：换句话说，就是他的二进制数相邻的两个位数永不相等，01交替。

```java
//解题思路：找一个2**n大于num的数，然后减一与num异或。
//测试用例：0不是正整数，最小为1.
public int One(int num){
    int n = 1;
    while(n <= num)
        n <<= 1;
    return num^(n-1);
}

public int Two(int num){
    int temp = n^(n>>1); //判断异或之后是否全为1
    return temp&(temp+1)==0;

//判断是否为2的次幂：
num & (num-1)==0;
//获取数值的某些二进制位：
num & 0b1111;
//获取数值第一个二进制1：
num & -num;
```

