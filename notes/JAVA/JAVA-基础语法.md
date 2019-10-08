# JAVA-基础知识

**简介**
由 SUN 公司 詹姆斯-高斯林 开发，后被 甲骨文（Oracle）收购。
它既有能开发桌面应用的 Java SE（Java Platform，Standard Edition）
也有开发 Web 应用的 Java EE（Java Platform，Enterprise Edition）
还有 Android 开发移动应用 和 嵌入式 的 Java ME（Java Platform，Micro Edition）
[JAVA SE EE ME具体区别](https://blog.csdn.net/manjianchao/article/details/60959105)


**知名应用**：我的世界、淘宝网、Android 操作系统。
**JDK**：JDK（Java Development Kit）称为 Java 开发包 或 Java 开发工具，是一个编写 Java 的 Applet 小程序和应用程序的程序开发环境。JDK 是整个 Java 的核心，包括了 Java 运行环境 JRE（Java Runtime Envirnment），JVM 和 Java 的核心类库（Java API）。

**JAVA 注释类似 C++，这里就不单独介绍了**

**第一个 JAVA 程序**

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
//常量
final double PI = 3.1415927; //关键字 final 通常常量全部大写

//数组-常规数组
String[] names = {"James", "Larry", "Tom", "Lacy"}; //字符串数组
int[] numbers = new int[10]; //默认值为 0
for(int i=0; i<names.length; i++){//长度为 length，注意和动态 size 的区分。
    System.out.println(names[i]);
}
for(String name : names){ //迭代
    System.out.println(name);
}


//动态数组
import java.util.Arrays //类能方便地操作数组，它提供的所有方法都是静态的。
//包含 fill：给数组赋值，sort：对数组排序，equals：比较数组，binarySearch：查找数组元素
import java.util.ArrayList;
import java.util.List;
/* List 是一个接口，而 ArrayList 是 List 接口的一个实现类。 ArrayList 类继承并实现了 List 接口。 
因此，List 接口不能被构造，也就是我们说的不能创建实例对象，但是我们可以像下面那样为 List 接口创建一个指
向自己的对象引用，而 ArrayList 实现类的实例对象就在这充当了这个指向List接口的对象引用。 */
//增
List<List<Integer>> triangle = new ArrayList<List<Integer>>(); //构造二维数组，长度不固定
triangle.add(new ArrayList<>()); //添加一个数组元素
triangle.get(0).add(1); //给第一个数组添加一个元素

//改
triangle.get(0).set(0,99); //修改第一个数字的第一个值为 99

//查
triangle.get(0).get(0); //获取第一个数组的第一个值
triangle.size(); //动态数组的大小，注意和静态数组长度的区分

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


**JAVA 增强 for 循环**：Java5引入了一种主要用于数组的增强型for循环。

**switch 语句**：switch 语句中的变量类型只能为 byte、short、int 或者 char。当变量的值与 case语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句。

```java
for(int x : numbers){
	System.out.prin(x)
}

switch(10){//只能是常量或者一个字符
	case 10:
	System.out.println("输出 10");
	case 11: //这里也会执行
	System.out.println("输出 11");
	break; //遇到 break 才会跳出
	default: //前面没有跳出，执行完这里也会跳出
	System.out.println("输出 default");
}
```


**函数**
```java
Math.max(a,b); //选取最大值，参数为 2.
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); //利用三目运算符，三数取最大值。
```


**输入输出**
```java
//输入
import java.util.Scanner;
Scanner scan = new Scanner(System.in);
if(scan.hasNext()){//只读取一次，遇到空格结束
	String str1 = scan.next();
	System.out.println("输入为：" + str1);
}

if(scan.hasNextLine()){//可以读取一行，遇到换行符结束
	String str2 = scan.nextLine();
	System.out.println("输入为：" + str2);
} 



System.out.println(); //输出带换行
System.out.print(); //输出不带换行
System.out.println(String.format("x:%d,y:%d,radius:%d",x,y,radius)); //%d 十进制，%s 字符串
```

**Number 和 Math 类**
装箱与拆箱，Java语言为每一个内置数据类型提供了对应的包装类，所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。

**Character 类**


**JAVA 常用包**

```java
//java.util 包
import java.util.ArrayList;
import java.util.List;
import java.util.Hashtable;

//java.lang 包
import java.lang.Math;
```





