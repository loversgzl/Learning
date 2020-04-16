

# Java-算法之字符串

高频考点：对于字符串相关问题，90% 可以用动态规划来解决。
建议总结：通配符的匹配、最长公共子串、最小编辑代价、最长回文串等。

1、输入
2、<a href="#快速判断回文串">快速判断回文串</a>
3、<a href="#正则表达式">正则表达式</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

**1、输入**
<a name="输入">
```java
import java.util.Scanner;
Scanner input = new Scanner(System.in);
//一行输入一个整数
int num = input.nextInt();
int num = Integer.parseInt(input.nextLine().trim());
//一行输入多个字符串，或者整数
String[] strs = input.nextLine().split(" ");
```


**2、快速判断回文串**
<a name="快速判断回文串" />
```java
for(int i=0; i<s.length(); i++)
	if(s.charAt(i) != s.charAt(s.length()-1-i))
		return false;
return true;
```

**3、正则表达式**
<a name="正则表达式" />

```java
/*题目：对于字符串中连续的 m 个相同字符串 S 将会压缩为 [m|S] (m为一个整数且1<=m<=100)，例如字符串ABCABCABC将会被压缩为[3|ABC]，你能帮助他进行解压缩么？ 
*/
import java.util.Scanner;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
 
public class Main{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String content = in.next();
        String pattern = "\\[(\\d+)\\|([A-Z]+)\\]";
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(content);
        while(m.find()){
            int num = Integer.valueOf(m.group(1));
            String rep = m.group(2);
            StringBuilder temp = new StringBuilder(rep.length()*num);
            for(int i=0; i<num; i++)
                temp.append(rep);
            content = m.replaceFirst(temp.toString());
            m = p.matcher(content);
        }
        System.out.println(content);
    }
}
```
