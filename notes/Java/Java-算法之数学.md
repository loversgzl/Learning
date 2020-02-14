

# 算法之数学-Java


1. 最大公约数：<a href="#最大公约数">最大公约数</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

### 最大公约数
<a name="最大公约数"></a>
```java
/*最大公约数
题目概述：gcd算法（辗转相除法）
*/
public int gcd(int a, int b) {
  return b == 0 ? a : gcd(b, a%b);
}
```





