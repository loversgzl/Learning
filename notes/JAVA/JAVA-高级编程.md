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
[参考博客](https://blog.csdn.net/Neuf_Soleil/article/details/80962686)
### JavaEE 主要技术
JavaEE 号称有十三种核心技术。它们分别是：JDBC、JNDI、EJB、RMI、Servlet、JSP、XML、JMS、Java IDL、JTS、JTA、JavaMail和JAF。
一般来讲，初学者应该遵循路径 Servlet -> JSP -> Spring -> 组合框架

### 框架
**组合框架**
**SSH**：Structs + Spring + Hibernate
**SSM**：Spring + SpringMVC + MyBatis（先学习这种）

**Spring**
Spring 框架是一个开源的 Java 平台，它为容易而快速的开发出耐用的 Java 应用程序提供了全面的基础设施。
**SpringMVC**
**MyBatis**

**Spring Security**
**Spring Boot**


### JAVA 项目练习
JAVA EE 方向
**Servlet**

**JSP**
全称（Java Server Pages）是一种动态网页开发技术。它使用 JSP 标签在 HTML 网页中插入 Java代码。标签通常以<%开头以%>结束。与 JavaScript 相比：虽然 JavaScript 可以在客户端动态生成HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。

**Swing**
* Swing 是一个为Java设计的GUI工具包。包括了图形用户界面（GUI）器件如：文本框，按钮，分隔窗格和表。
**五子棋**：1.掌握 JavaGUI 界面设计、2.掌握鼠标事件的监听（MouseListener，MouseMotionListener）




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







