# Java-算法栈、队列和字典

**栈的知识点**
栈：是限定在表尾进行插入删除操作的线性表，允许插入和删除操作的一端称为栈顶 top，另一端称为栈底 bottom。
遗忘点：数组两端都可以当作 bottom，一般从数组下标0开始，因此空栈表示：top = -1，top 指向当前元素。例子：如一个栈的 StackSize = 5,则栈空 top = -1，栈满 top = 4.

双向栈：数组两头都可以进行栈操作，从左右两边向中间靠拢，数组两端为栈底。
空栈：stackLeft = -1，stackRight = n；栈满：stackLeft + 1 = stackRight

链栈：即用链表作为栈的存储结构，top 和 head 功能相同，因此可不要 head，用 top 指向头节点。
栈空：top = None，基本不存在栈满的情况。
混淆点：链表向下，top 指向头节点，插入/删除节点都是在头结点操作。

**队列的知识点**
循环队列：只允许在一端进行插入，一端进行删除的线性表
遗忘点：空队列：front==rear，rear 指向可以插入的空位置，front 指向队列第一个元素。
队满：front == rear，与队空冲突：1 设置一个 flag，0 为空，1 为满。2 空一个元素，队满：rear + 1 = front
因此，空队列不变，满队列可以进行设置。
例子：如果一个队列 queueSize = 5，队空：front = rear = 0， 队满：rear + 1 = front
元素个数为 (rear - fron + max)%max，注意如果有些题目指明位置就需要注意了，如问：大小为MAX的循环队列中，front 为当前头元素位置，rear 为当前队尾元素位置(最后一个元素的位置)，则任意时刻，队列中的元素个数为？ 那么就应该是(rear - fron + 1 + max)%max，队尾位置不同。

**记忆小技巧**：栈和队列，删除元素肯定需要 top 和 front 指向第一个元素。因此空 top = -1，front == rear。
快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

1、单调栈或递减栈
2、后缀表达式



1、<a href="#单调栈">单调栈</a>
<a name="单调栈"></a>

```java
/*
基本概念：扫描数组，如果栈是空的那么压如一个元素，因为栈里没有元素比这个小，所以肯定要放在最底下。
如果栈不是空的并且当前扫描的元素小于（等于）栈顶元素，压入。如果栈不是空的并且扫描元素大于栈顶元素，我们要在不断弹出元素直至栈顶元素大于当前元素，或者空栈把新元素放入。
题目：共有n座高楼排成一行，当前面的楼的高度大于等于后面的楼时，后面的楼将被挡住。
题解：利用两个单调栈，往前后两个方向看。
还可以解决的题目：最长上升子序列、*/
import java.util.Stack;
public static void main(String[] args){
    int[] seeLeft = new int[n];
    int[] seeRight = new int[n];
    Stack<Integer> stack = new Stack<>();
    //单调栈核心代码，一直往后看
    for(int i=0; i<n; i++){
        seeLeft[i] = stack.size();
        while(!stack.isEmpty() && stack.peek() <= arr[i])
            stack.pop();
        stack.push(arr[i]);
    }//单调栈核心代码，一直往前看，倒着统计，省略。
    for(int i=0; i<n; i++)
        System.out.print(seeLeft[i]+seeRight[i]+1+" ");
}

```

2、<a href="#后缀表达式">后缀表达式</a>
<a name="后缀表达式"></a>
```java
/*中缀表达式，操作符在中间：(3 + 4) × 5 - 6
后缀表达式，操作符在后面：3 4 + 5 × 6 -
后缀表达式计算，利用栈解决：建立一个栈S 。从左到右读表达式，如果读到操作数就将它压入栈S中，如果读到 n 元运算符(即需要参数个数为 n 的运算符)则取出由栈顶向下的 n 项按操作符运算，再将运算的结果代替原栈顶的 n 项，压入栈S中 。如果后缀表达式未读完，则重复上面过程，最后输出栈顶的数值则为结束。*/
```

3、<a href="#递减栈">递减栈</a>
<a name="递减栈"></a>
```java
/*问题：给定一个数组，里面是木棍长度，要求木棍长度不递减，可以折断木棍放在原来的位置。求最少的折断次数。
方法：采用递减栈的策略，优化后用一个参数代替栈。*/
public void breakNum(List<Integer> nums){
    int compareLast = Integer.MAX_VALUE;
    int ans = 0;
    for(int i = nums.size()-1; i >= 0; i--){
    //非常棒的三句代码，很厉害！
        if(nums.get(i) > compareLast) {
            int t = (nums.get(i)-1)/compareLast;//最少折几次
            ans += t;
            compareLast = nums.get(i)/(t+1);//平均最大是多少
        }else {
            compareLast = nums.get(i);
        }
    }
}
```

<a name=""></a>
```java


```

<a name=""></a>
```java

```

<a name=""></a>
```java


```

<a name=""></a>
```java

```






