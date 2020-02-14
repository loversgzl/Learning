

# Java-算法之位运算

1. <a href="#基本概念">基本概念</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

<a name="基本概念"></a>
```java
//逆序二进制转整数
int res = 0;
while(head != null) {
    res = res*2 + head.val;
    head = head.next;
}
```

