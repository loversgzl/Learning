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
元素个数为 (rear - fron + max)%max，注意如果有些题目指明位置就需要注意了，如问：大小为MAX的循环队列中，f为当前对头元素位置，r为当前队尾元素位置(最后一个元素的位置)，则任意时刻，队列中的元素个数为？ 那么就应该是(rear - fron + 1 + max)%max，队尾位置不同。

**记忆小技巧**：栈和队列，删除元素肯定需要 top 和 front 指向第一个元素。因此空 top = -1，front == rear。


1. <a href="#字节跳动面试题：特征提取">字节跳动面试题：特征提取</a>


快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。


<a name="字节跳动面试题：特征提取"></a>
```java
/*
特征提取问题：输入多行，每行包含多个点（x,y），求连续在每行中出现的点，最多连续几行。
例如：第 2，3，4 行都出现了(5,5),其余点最多连续出现在两行中，则输出 3 行.
解题思路：知道利用双map做，但是想到的是不断交换，一位coder更有效的解题方式。
*/
HashMap<String,Integer> map = new HashMap<>();
HashMap<String,Integer> tempmap = new HashMap<>();
for(int j=0; j<M; j++) { //输入 M 行
    tempmap.clear();
    int n = in.nextInt();//每行输入 n 个点
    for(int i=0; i<n; i++) {
        int x = in.nextInt();//输入 x 坐标
        int y = in.nextInt();//输入 y 坐标
        String key = Integer.toString(x)+"-"+Integer.toString(y);
        tempmap.put(key,map.getOrDefault(key, 0)+1);
        max = Math.max(tempmap.get(key), max);
    }
map.clear();
map.putAll(tempmap);
}
```

**剑指offer：栈的压入，弹出。**
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
<a name=""></a>
```java
import java.util.ArrayList;
import java.util.Stack;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        if(pushA == null || popA == null || pushA.length == 0 || popA.length == 0)
            return false;
        int i=0, j=0;
        Stack<Integer> stack = new Stack<>();
        stack.push(pushA[i++]);
        while( j < popA.length){
            while(popA[j] != stack.peek()){
                if( i >= pushA.length)
                    return false;
                stack.push(pushA[i++]);
            }
            stack.pop();
            j++;
        }
        return true;
    }
}
```

### 折木棍
问题：给定一个数组，里面是木棍长度，要求木棍长度不递减，可以折断木棍放在原来的位置。求最少的折断次数。
方法：采用递减栈的策略，优化后用一个参数代替栈。
<a name=""></a>
```java
public void breakNum(List<Integer> nums){
    int compareLast = Integer.MAX_VALUE;
    int ans = 0;
    for(int i = nums.size()-1; i >= 0; i--){
    //非常棒的三句代码，很厉害！
        if(nums.get(i) > compareLast) {
            int t = (nums.get(i)-1)/compareLast;
            ans += t;
            compareLast = nums.get(i)/(t+1);
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






