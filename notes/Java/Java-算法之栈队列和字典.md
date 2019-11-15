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

<a name=""></a>
```java


```

<a name=""></a>
```java

```






