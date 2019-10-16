# JAVA-基础知识

**简介**
由 SUN 公司 詹姆斯-高斯林 开发，后被 甲骨文（Oracle）收购。
它既有能开发桌面应用的 Java SE（Java Platform，Standard Edition）
也有开发 Web 应用的 Java EE（Java Platform，Enterprise Edition）
还有 Android 开发移动应用 和 嵌入式 的 Java ME（Java Platform，Micro Edition）
[JAVA SE EE ME具体区别](https://blog.csdn.net/qq_29611345/article/details/102384776)
[JAVA 各个版本的新增特性](https://blog.csdn.net/qq_22194659/article/details/86134443)

**知名应用**：我的世界、淘宝网、Android 操作系统。
**JDK**：JDK（Java Development Kit）称为 Java 开发包 或 Java 开发工具，是一个编写 Java 的 Applet 小程序和应用程序的程序开发环境。JDK 是整个 Java 的核心，包括了 Java 运行环境 JRE（Java Runtime Envirnment），JVM 和 Java 的核心类库（Java API）。
[JDK 和 JRE 的区别](https://blog.csdn.net/shaochenshuo/article/details/78507132)

**JAVA 注释类似 C++，这里就不单独介绍了**

### 第一个 JAVA 程序
```java
import java.util.Scanner; //引入 JAVA 输入包
public class test {
    public static void main(String args[]) {//所有的 Java 程序主方法入口
    	String name = "tom";
    	System.out.println(name);
    }
}
/*
1.创建源文件 .java
2.javac 将 .java 源代码转换为 JVM 能够识别的字节码 .class
3.通过 JVM 执行 .class 文件
编译原理可参考简书：https://www.jianshu.com/p/af78a314c6fc
*/
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

final double PI = 3.1415927; //常量 final 通常常量全部大写
boolean b = true; //小写
char g = 'g'; //一个字符
String st = "this is a line"; //双引号
Object ob = null; //空表示

//强制类型转换
long a = 100;
int b = (int)a;
Integer.parseInt(String) //将String字符类型数据转换为Integer整型数据。
int n = Math.pow(a,b); //a 的 b 次方。
```

### 集合框架与数组
数组缺点：固定长度，数据多了不够，少了浪费空间。
容器类：为了解决数组的缺点，如  ArrayList、

```java
//数组-常规数组
String[] names = {"James", "Larry", "Tom", "Lacy"}; //字符串数组
int[] numbers = new int[10]; //默认值为 0
for(int i=0; i<names.length; i++)//长度为 length，注意和动态 size 的区分。
    System.out.println(names[i]);
for(String name : names) //迭代
    System.out.println(name);

//集合框架-动态数组
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
/* List 是一个接口，而 ArrayList 是 List 接口的一个实现类。 ArrayList 类继承并实现了 List 接口。 因此，List 接口不能被构造，也就是我们说的不能创建实例对象，但是我们可以像下面那样为 List 接口创建一个指向自己的对象引用，而 ArrayList 实现类的实例对象就在这充当了这个指向List接口的对象引用。 
方法：
增：add、addAll、
	add(object);末尾添加 add(index,object);指定位置添加
删：remove、clear、
	remove();可以是下标也可以是对象。
改：set、toArray、
	set(index,obj);修改某个位置的值 toArray();转换为数组
查：contains、get、indexOf、size、
	get(index); 获取某个位置的对象 indexOf(obj);某个对象的位置 size()集合大小
输出：toString(),以字符串逗号隔开的形式输出。
*/
//增
List<List<Integer>> triangle = new ArrayList<List<Integer>>(); //构造二维数组，长度不固定
triangle.add(new ArrayList<>()); //添加一个数组元素
triangle.get(0).add(1); //给第一个数组添加一个元素

//改
triangle.get(0).set(0,99); //修改第一个数组的第一个值为 99

//查
triangle.get(0).get(0); //获取第一个数组的第一个值
triangle.size(); //动态数组的大小，注意和静态数组长度的区分

import java.util.LinkedList; //LinkedList是一个双向链表结构的list

//链表结构，拥有和 ArrayList 一样的方法再加上下面这些
LinkedList<String> sts =new LinkedList<>();
//增
sts.addLast("last1"); //在末尾插入，
sts.addFirst("first1");//在首部插入
//删
sts.removeLast();sts.removeFirst();
//查
sts.getLast(); sts.getFirst();

//队列queue
import java.util.Queue;
Queue<String> q = new LinkedList<>();//也实现了队列的接口
q.offer("inQueue");//入队列
q.poll();//出队
q.peek();//查看队首，但不取出
```

### 字符串
问：字符串拼接，StringBuffer，StringBuilder，concat 和 + 的区别。
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)
```java
String preStr = "192.168.1.1"; 
String[] strs = preStr.split("\\."); //正确写法。对小圆点进行转义
String ss = String.valueOf(11); //将数字转为字符串
int a = Integer.valueOf(strs[0]); //将 ip 地址转换为整数
int b = Integer.parseInt(strs[1]); //将 ip 地址转换为整数

//拼接字符串
String s = strs[0]+11+"22"; //一开始就可以确定的量，使用 + 更快
//字符串拼接，循环中切忌使用，StringBuilder 最好。
String s2 = s2.concat(String.valueOf(i));
String s3 = s2.substring(beginIndex); //返回此坐标开始后的字符串

List<String> list = new ArrayList<String>();
for (int i = 0; i < 10000; i++)
	list.add(String.valueOf(i)); 
StringUtils.join(list, ""); //将字符串数组拼接

StringBuffer sb = new StringBuffer(); //StringBuilder 类似
for (int i = 0; i < 100000; i++)
	sb.append(String.valueOf(i));
sb.toString();


```



**集合**
```java
/*如何去除数组中重复的元素：
方法一：只需要创建一个集合，然后遍历数组逐一放入集合，只要在放入之前用 contains()方法判断一下集合中是否已经存在这个元素就行了，然后用 toArray 转成数组一切搞定。 
方法二：最简单的方法就是利用Set集合无序不可重复的特性进行元素过滤。 */

public static int[] one(int[] arr){
    Set<Integer> set = new HashSet();  //实例化一个set集合  
    //遍历数组并存入集合,如果元素已存在则不会重复存入  
    for (int i = 0; i < arr.length; i++) {  
    set.add(arr[i]);  
    }  
    return set.toArray();  //返回Set集合的数组形式
}

public static void two(int[] arr){  
    List list = new ArrayList();  //创建一个集合 
    for(int i=0; i<arr.length; i++){  //遍历数组往集合里存元素
        if(!list.contains(arr[i])){  //如果集合里面没有相同的元素才往里存
        list.add(arr[i]);  
        }  
    }  

    //toArray()方法会返回一个包含集合所有元素的Object类型数组  
    int[] newArr = list.toArray();  
    for(int i=0;i<newArr.length;i++){ //遍历输出一下测试是否有效 
   		System.out.println(" "+newArr[i]);  
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
System.out.printf(); //C 格式输出
```


### 函数
```java
Math.max(a,b); //选取最大值，参数为 2.
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); //利用三目运算符，三数取最大值。
```

### 常用类
**Number 和 Math 类**
装箱与拆箱，Java语言为每一个内置数据类型提供了对应的包装类，所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。

**Character 类**


**JAVA 常用包**

```java
//java.util 包
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import java.util.LinkedList;
import java.util.Hashtable;
import java.util.Scanner;
import java.util.Random;


//java.lang 包
import java.lang.Math;

//java.net 网络编程常用包
import java.net.Socket;

//java.io 包
import java.io.File // I/O文件对象包
import java.io.FileInputStream; //输入流
import java.io.FileOutputStream; //输出流
import java.io.OutputStream;
import java.io.DataOutputStream;

```

包括：枚举（Enumeration）、位集合（BitSet）、向量（Vector）、栈（Stack）、字典（Dictionary）、哈希表（Hashtable）、属性（Properties）

```java
/*
这种传统接口已被迭代器取代，虽然Enumeration 还未被遗弃，但在现代代码中已经被很少使用了。
*/
import java.util.Vector;
import java.util.Enumeration;

public class test{
	public static void main(String args[]){
		Enumeration days;
		Vector dayNames = new Vector();
		dayNames.add("Sunday");
		dayNames.add("Monday");
		dayNames.add("Tuesday");
		dayNames.add("Wednesday");
		dayNames.add("Thursday");
		dayNames.add("Friday");
		dayNames.add("Saturday");
		days = dayNames.elements();
		while(days.hasMoreElements()){
			System.out.println(days.nextElement());
		}
	}
}
```
问：equals 和 == 的区别？
前者：不能用于基本数据类型比较，只能用于引用类型，比较内存地址。
后者：基本数据类型比较值，引用类型比较内存地址。


