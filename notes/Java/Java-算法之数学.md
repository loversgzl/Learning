

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

### 循环求余法
```java
//大数越界：当 a 增大时，最后返回的 3^a 大小以指数级别增长，可能超出 int32 甚至 int64 的取值范围，导致返回值错误。解决方法，循环求余，快速幂指数求余。
//求(3^a)%p,循环求余
(x*y)%p = [(x%p)*(y%p)]%p;
int rem = 1;
for(int i=0; i<a; i++)
	rem = (rem*3)%p;

//求(x^a)%p,快速幂指数求余
int rem = 1;
int x = 3;
while(a>0){
    if(a%2==1) rem = (rem*x)%p;
    x = x*x%p;
    a /= 2;
}
```



