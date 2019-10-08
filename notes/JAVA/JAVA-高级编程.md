# JAVA-高级编程

**JAVA - 高级编程**
* JAVA - 网络编程
* JAVA - 多线程编程
* JAVA - 分布式编程
* JAVA - 框架

**JAVA - 网络编程**

```java
import java.util.Scanner; //引入 JAVA 包
public class test {
    public static void main(String args[]) {//所有的 Java 程序主方法入口
    	String name = "tom";
    	System.out.println(name);
    }
}
```
### 命名
**类名**：驼峰命名法，所有单词首字母大写
**方法名**：第一个单词小写，其余单词首字母大写
**源文件名**：源文件名必须和类名相同，文件名的后缀为.java。（如果文件名和类名不相同则会导致编译错误）。


### JAVA 基本数据类型
* **内置数据类型**：byte（8）、short（16）、int（32）、long（64）、float（32）、double（64）、boolean、char（16）
```java
//其他数据类型也有相同的常量
//Byte、Short、Integer、Long、Float、Double、boolean、Character
System.out.println(Byte.SIZE); //二进制位数
System.out.println(Byte.MIN_VALUE); //最小值
System.out.println(Byte.MAX_VALUE); //最大值

boolean b = true; //小写
char g = 'g'; //一个字符
String st = "this is a line"; //双引号
```

**常量和数组**
```java
final double PI = 3.1415927; //关键字 final 通常常量全部大写
String[] names = {"James", "Larry", "Tom", "Lacy"}; //字符串数组
int[] numbers = new int[10]; //默认值为 0

import java.util.Arrays //类能方便地操作数组，它提供的所有方法都是静态的。
//包含 fill：给数组赋值，sort：对数组排序，equals：比较数组，binarySearch：查找数组元素
```


**枚举**
```java
//代码参考 W3C
class FreshJuice {
   enum FreshJuiceSize{ SMALL, MEDIUM, LARGE }
   FreshJuiceSize size;
}

public class FreshJuiceTest {
   public static void main(String args[]){
      FreshJuice juice = new FreshJuice();
      juice.size = FreshJuice. FreshJuiceSize.MEDIUM ;
   }
}
```

**条件语句**
**JAVA 增强 for 循环**：Java5引入了一种主要用于数组的增强型for循环。
```java
for(int x : numbers){
	System.out.prin(x)
}
```


**函数**



**输入输出**
```java
System.out.println() //输出带换行
```

**Number 和 Math 类**
装箱与拆箱，Java语言为每一个内置数据类型提供了对应的包装类，所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。

**Character 类**







