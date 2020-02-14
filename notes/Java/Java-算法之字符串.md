

# Java-算法之字符串

高频考点：对于字符串相关问题，90% 可以用动态规划来解决。
建议总结：通配符的匹配、最长公共子串、最小编辑代价、最长回文串等。


1. <a href="#如何快速判断回文串">如何快速判断回文串</a>
2. 剑指offer：字符串转整数
3. 正则表达式

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

### 如何快速判断回文串
```java
for(int i=0; i<s.length(); i++)
	if(s.charAt(i) != s.charAt(s.length()-1-i))
		return false;
return true;
```

