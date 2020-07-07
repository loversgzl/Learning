

# 排列组合-Java

1. <a href="#递归求排列组合数">递归求排列组合数</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

```java
/*题目概述：有 n 个 a，m 个 b，求排列组合有多少种可能？*/
public int dfs(int n, int m){
    if(n == 0) return 1;//剩下都是 b，排列只有一种可能
    if(m == 0) return 1;//剩下都是 a，排列只有一种可能
    return dfs(n-1,m) + dfs(n,m-1);//第一个选 a，或者第一个选 b
}
```

