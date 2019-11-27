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

/*
F3 :可以查看某个函数的源码
放在某个函数上，F2 可以查看具体的使用方法
*/

```

### 命名
**类名**：驼峰命名法，所有单词首字母大写
**方法名**：第一个单词小写，其余单词首字母大写
**源文件名**：源文件名必须和类名相同，文件名的后缀为.java。（如果文件名和类名不相同则会导致编译错误）。
**关键字与保留字**：Java 保留字是指现在Java版本尚未使用，但以后版本可能会作为关键字使用。所以注意false、true、null等都是保留字。
**Java标识符**：字母、数字、下划线、美元符（$）、数字不能作为首位。

### JAVA 常用包
```java
/*
常用内置类
*/
Math,StringBuilder;StringBuffer;

/*
java.util 包
*/
import java.util.Arrays;
import java.util.ArrayList;//动态数组
import java.util.List;//接口
import java.util.LinkedList;//链表
import java.util.Hashtable;
import java.util.HashMap;//字典
import java.util.HashSet;//集合
import java.util.Collections;//方法调用,容器的工具类
import java.util.Scanner;//输入
import java.util.Random;//随机数

/*
java.io 包
*/
import java.io.File // I/O文件对象接口
import java.io.FileInputStream; //输入流
import java.io.FileOutputStream; //输出流
import java.io.ObjectOutputStream;
import java.io.DataOutputStream;
import java.io.Serializable; //序列化接口

/*
java.net 多线程编程
*/


/*
java.net 网络编程常用包
*/
import java.net.Socket;


/*
java.lang 包
*/
import java.lang.Math;
```
### 输入输出
按照流是否直接与特定的地方（如磁盘、内存、设备等）相连，分为 节点流 和 处理流 两类。
**节点流**：可以从或向一个特定的地方（节点）读写数据。
文 件 FileInputStream FileOutputStrean FileReader FileWriter 文件进行处理的节点流。




```java
//输入
import java.util.Scanner;
Scanner scan = new Scanner(System.in);
int one = scan.nextInt(); //一个一个读，遇到空格或者回车结束。
String str = scan.nextLine(); //一行一行读
char ch = (char)System.in.read();
ch = (char)(ch+32); //大写转小写

if(scan.hasNext()){//只读取一次，遇到空格或者回车结束
	String str1 = scan.next();//读取字符串
}

System.out.println(); //输出带换行
System.out.print(); //输出不带换行
System.out.println(String.format("x:%d,y:%d,radius:%d",x,y,radius)); //%d 十进制，%s 字符串
System.out.printf(); //C 格式输出
```

### 集合框架
![集合框架](../../pics/集合框架.jpg)

由图可见，集合框架主要包括两种类型的容器，集合（Collection）、图（Map），前者存储一个元素的集合，后者存储键/值对映射。集合又有三种子类型，List、Set、Queue，具体实现类有ArrayList、LinkedList、HashSet、LinkedHashSet，图的具体实现类有：HashMap、HashTable、LinkedHashMap、HashTree。
接口：Collection、List、Set、Map，之所以定义多个接口是为了以不同的方式操作集合对象。

数组缺点：固定长度，数据多了不够，少了浪费空间。
容器类：为了解决数组的缺点，如  ArrayList、LinkedList、

```java
/*
易混淆点
数组的长度：.lenght; List的长度：.size(); 字符串的长度：.length();
字符串添加元素：append(x);
List添加元素：add(x); 
List获取用 get！不要总是用 [] 数组的东西。
StringBuilder用charAt
*/

/*
普通数组
*/
//排序函数
import java.util.Arrays;
Arrays.parallelSort(nums);



String[] names = {"James", "Larry", "Tom", "Lacy"}; //字符串数组
return new int[]{1,2,3}; //直接返回一个匿名数组
int[] numbers = new int[10]; //默认值为 0
int[][] array = new int[10][10]; //二维数组
for(int i=0; i<names.length; i++)//长度为 length，注意和动态 size 的区分。
    System.out.println(names[i]);
for(String name : names) //迭代
    System.out.println(name);
System.arraycopy(origin,0,copy,0,9); //拷贝数组(原始数组,index,新数组,index,数量)。



/*
判断空的情况
*/
if(names == null || names != null && names.length == 0){}


//容器的工具类
import java.util.Collections;
Collections.sort(arr);//正序排序
Collections.reverse(arr);//排完之后再倒转一下
//Collection是  List Set Queue 的接口 
//Collections是一个类，容器的工具类，只能是容器，无返回值，改变原List
import java.util.Collections;
List<Integer> numbers = new ArrayList<>();
Collections.reverse(numbers); //倒转数组,
Collections.shuffle(numbers);//混淆
Collections.sort(numbers);
Collections.swap(numbers,0,1);
Collections.rotate(numbers,2);//所有元素向右循环移动 2 位


/* 
集合框架-动态数组
List 是一个接口，而 ArrayList 是 List 接口的一个实现类。 ArrayList 类继承并实现了 List 接口。 因此，List 接口不能被构造，也就是我们说的不能创建实例对象，但是我们可以像下面那样为 List 接口创建一个指向自己的对象引用，而 ArrayList 实现类的实例对象就在这充当了这个指向List接口的对象引用。 
*/
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
List<String> list = new ArrayList<>();
list.add("hello");list.add("java");


//增：末尾添加、指定位置添加、添加另一个列表、
	add(object);  add(index,object);  addAll(list);
//删：下标或对象、清空、
	remove();  clear();
//改：修改某个位置的值、转换为数组、
	set(index,obj); toArray();
//查：获取某个位置的对象、某个对象的位置、返回元素个数、返回是否包含元素x、
	get(index);  indexOf(obj);  size();  contains(x); 
//输出：以字符串逗号隔开的形式输出。
	toString();
//三种遍历方式
for(String str : list)
    System.out.println(str);

list.toArray(strArray); //一个字符串数组
for(int i=0; i<strArray.length; i++)
    System.out.println(strArray[i]);

Iterator<String> it = list.iterator();
while(it.hasNext())
    System.out.println(it.next());


//动态二维数组

List<List<Integer>> triangle = new ArrayList<List<Integer>>(); //构造二维数组，长度不固定
//增
triangle.add(new ArrayList<>()); //添加一个数组元素
triangle.get(0).add(1); //给第一个数组添加一个元素
//改
triangle.get(0).set(0,99); //修改第一个数组的第一个值为 99
//查
triangle.get(0).get(0); //获取第一个数组的第一个值
triangle.size(); //动态数组的大小，注意和静态数组长度的区分


/*
链表结构，拥有和 ArrayList 一样的方法再加上下面这些
LinkedList是一个双向链表结构的list
*/
import java.util.LinkedList; 
LinkedList<String> sts =new LinkedList<>();
//增
sts.addLast("last1"); //在末尾插入，
sts.addFirst("first1");//在首部插入
//删
sts.removeLast();sts.removeFirst();
//查
sts.getLast(); sts.getFirst();

/* 集合 Set
问：如何去除数组中重复的元素？
方法一：只需要创建一个集合，然后遍历数组逐一放入集合，只要在放入之前用 contains()方法判断一下集合中是否已经存在这个元素就行了，然后用 toArray 转成数组一切搞定。 
方法二：最简单的方法就是利用Set集合无序不可重复的特性进行元素过滤。
*/
import java.util.HashSet;
Set<Integer> set = new HashSet();  //实例化一个set集合  

//增
for (int x : arr)
    set.add(arr[i]);  //遍历数组并存入集合,如果元素已存在则不会重复存入 
return set.toArray();  //返回Set集合的数组形式
//删除
remove(x); 
//查：返回是否包含x、
contains(x);

/*
队列queue
*/
import java.util.Queue;
Queue<String> q = new LinkedList<>();//也实现了队列的接口
q.offer("inQueue");//入队列
q.poll();//出队
q.peek();//查看队首，但不取出



/*
字典接口 import java.util.MAP;
四个实现类 import java.util.HashMap、LinkedHashMap、HashTable、TreeMap
*/
import java.util.HashMap;
Map<String,Integer> map = new HashMap<>();
Map<int[],Integer> map = new HashMap<>();

//增，改,如果已经存在，则覆盖
put("Tom",12); putAll(anotherMap); 

//清空、删除某个键、
clear();remove(x);

//查
containsKey("Tom"); //返回Boolean
get("Tom"); getOrDefault(key, 0);

//四种遍历方式
for(String key : map.keySet())
    System.out.println(key+map.get(key));

Iterator<Map.Entry<String, Integer>> it = map.entrySet().iterator();
while(it.hasNext){
    Map.Entry<String, Integer> entry = it.next();
    System.out.println(entry.getKey()+entry.getValue());
}

for(Map.Entry<String, Integer> entry : map.entrySet())
    System.out.println(entry.getKey()+entry.getValue());

for(String v : map.values())
    System.out.println(v);

/*
在集合框架之前应用的一些类，现在一般用集合框架代替。
Dictionary、Vector、Stack、Properties、
*/

//栈 Stack
Stack stack = new Stack(); //初始化
stack.empty(); //判断是否为空，返回true/false
stack.peek(); //取栈顶值（不出栈），返回 Object
stack.push(Object);//进栈，返回 Object
stack.pop();//出栈，返回的是 Object 对象，需要类型转换





```

### String 字符串
问：字符串拼接，StringBuffer，StringBuilder，concat 和 + 的区别。
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)

```java
/*
String s="abce"是一种非常特殊的形式,和 new 有本质的区别。它是 java 中唯一不需要 new 就可以产生对象的途径。以String s="abce";形式赋值在 java 中叫直接量,它是在常量池中而不是象 new 一样放在压缩堆中。这种形式的字符串，在JVM内部发生字符串拘留，即当声明这样的一个字符串后，JVM会在常量池中先查找有有没有一个值为"abcd"的对象,如果有,就会把它赋给当前引用.即原来那个引用和现在这个引用指点向了同一对象,如果没有,则在常量池中新创建一个"abcd",下一次如果有String s1 = "abcd";又会将s1指向"abcd"这个对象,即以这形式声明的字符串,只要值相等,任何多个引用都指向同一对象.
*/
//内置类型
StringBuilder sb = new StringBuilder();
StringBuffer br = new StringBuffer();

//增
sb.append("abcd");//在末尾添加串

//删
sb.delete(0,1);//删除一个字符，[0,1)，左闭右开，显示 bcd

//改
sb.reverse(); //倒转字符串，dcba
sb.insert(0,9); //首位插入 9，9abcd
sb.replace(0,1,"e");//替换[0,1)，左闭右开
    
//查
char ch = sb.charAt(index); //返回某个位置的字符!!很容易忘
int length = sb.length();
String s = sb.substring(beginIndex); //返回此坐标开始后的字符串




//改
//字符串，整数，互换
String preStr = "192.168.1.1"; 
String[] strs = preStr.split("\\."); //正确写法。对小圆点进行转义

//拼接字符串
String s = strs[0]+11+"22"; //一开始就可以确定的量，使用 + 更快
//循环中切忌使用+来拼接字符串，StringBuilder 最好。
String s2 = s2.concat(String.valueOf(i));
//list 拼接
List<String> list = new ArrayList<String>();
for (int i = 0; i < 10000; i++)
	list.add(String.valueOf(i)); 
StringUtils.join(list, ""); //将字符串数组拼接
//StringBuffer 拼接
StringBuffer sb = new StringBuffer(); //StringBuilder 类似
for (int i = 0; i < 100000; i++)
	sb.append(String.valueOf(i));
sb.toString();


```

正则表达式
```java
import java.util.regex.Pattern;
public static void main(String[] args) {
    String content = "I am noob " + "from runoob.com.";
    String pattern = ".*runoob.*";
    boolean isMatch = Pattern.matches(pattern, content);
    System.out.println("字符串中是否包含了 'runoob' 子字符串? " + isMatch);
}   

public static void main(String[] args) {
	// 按指定模式在字符串查找
	String line = "This order was placed for QT3000! OK?";
	String pattern = "(\\D*)(\\d+)(.*)";
    //捕获组定义多个表达式,不同表达式组合作为一个表达式，所以调换顺序也会有很大影响
	
	// 创建 Pattern 对象
	Pattern r = Pattern.compile(pattern);
	
	// 现在创建 matcher 对象
	Matcher m = r.matcher(line);
	if (m.find( )) {
        //后面三个加起来应该等于第一个。
	   System.out.println("Found value: " + m.group(0) );//表示整个表达式匹配的内容
	   System.out.println("Found value: " + m.group(1) );//第一个左括号匹配到的内容
	   System.out.println("Found value: " + m.group(2) );//第二个左括号匹配到的内容
	   System.out.println("Found value: " + m.group(3) );//第三个左括号匹配到的内容
	} else {
	   System.out.println("NO MATCH");
	}
}

/*
^ ：匹配输入字符串开始的位置。
$ ：匹配输入字符串结尾的位置。
* ：零次或多次匹配前面的字符或子表达式。
+ ：一次或多次匹配前面的字符或子表达式。
? ：零次或一次匹配前面的字符或子表达式。
\d ：数字字符匹配。等效于 [0-9]。
\D ：非数字字符匹配。等效于 [^0-9]。
*/


```

### 常用内置类 Math
```java
//Math
Math.max(a,b); //选取最大值，参数为 2.
//利用三目运算符，三数取最大值。
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); 
int n = Math.pow(a,b); //a 的 b 次方。
//向下取整，向上取整，四舍五入（内置为+0.5，向下取整），
Math.floor(11.5); Math.ceil(11.5); Math.round(-11.5); 
Math.sqrt(x); //求平方根



```

### 常用类需引包 Number、Character、
**Number类**
Java 语言是一个面向对象的语言，但是 Java 中的基本数据类型（原生类）却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)，其中六个包装类都是抽象类 Number 的子类。byte（8）、short（16）、int（32）、long（64）、float（32）、double（64）、boolean、char（16）
**原生类**：Java中，数据类型分为基本数据类型（或叫做原生类、内置类型）和引用数据类型。
Java不是纯的面向对象的语言，不纯的地方就是这些基本数据类型不是对象。当然初期Java的运行速度很慢，但是基本数据类型能在一定程度上改善性能。如果你想编写纯的面向对象的程序，用包装器类是取代基本数据类型就可以了。六种数字类型都是有符号的，固定的存储空间正是Java可移植性、跨平台的原因之一；
基本类型的存在导致了Java OOP的不纯粹性。因为基本类型不是对象，一切皆对象是个小小的谎言。这是出于执行效率的权衡。

```java
//基本数据类型 和 包装类
byte;short;int;long;float;double;char;boolean;
Byte;Short;Integer;Long;Float;Double;Character;Boolean;

//装箱与拆箱，将基本数据类型包装为包装类，相加时再转为基本数据类型。
Integer x = 5; x = x +10;

int a = 99; 
Integer c = new Integer(a);//装箱
Integer b = Integer.valueOf(a);//装箱，该方法返回参数的原生Number对象。
int d = b.intValue(); //拆箱，可以拆成任意 Number 类

//包装类的常量
Byte.SIZE; //二进制位数
Byte.MIN_VALUE; //最小值
Byte.MAX_VALUE; //最大值

//基本数据类型赋值
final double PI = 3.1415927; //常量 final 通常常量全部大写
boolean b = true; //小写
char g = 'g'; //一个字符
String st = "this is a line"; //双引号
Object ob = null; //空表示
&& | ! //逻辑运算符，与或非

/*
强制类型转换，包装类的方法以Integer为例，其它类似：
Integer.parseInt()、Integer.toString()
注意：Integer.valueOf()是装箱，返回的是包装类。
*/
long a = 100; 
int b = (int)a;
String st = Integer.toString(b); //将整数转为字符串
int n = Integer.parseInt(st); //字符串转整数，后面可带进制
String st = String.valueOf(b); //将整数转为字符串
char ch = 'G';
ch = (char)(ch+32); //大写转小写



//增强型 for 循环
for(String str :strs)
    System.out.println(str);

//List 使用迭代器
ArrayList list = new ArrayList();//省略赋值过程
Iterator iterator = list.iterator();
while(iterator.hasNext())
	System.out.println(iterator.next());

//返回系统时间
long start = System.currentTimeMillis();

```

**Character 类**
```java
import java.lang.Character;
char ch = 'a';
Character.isDigit(ch);
Character.isLetter(ch);
Character.isUpperCase(ch);
Character.isLowerCse(ch);
```

**Random 类**
```java
import java.util.Random;//重新创建随机数生成器
Random rand = new Random(11);//需要种子
int i = rand.nextInt(100);//产生[0,100) 的随机数
int num = (int)(Math.random()*100);//取[0,100)的随机数，一般这种写法，生成[0.0,1.0)的double数
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


### 异常
大致分为三类：检查性异常、运行时异常、错误。
异常分为两类，Error 和 Exception，它们都继承 Throwable。

try、catch、finally：无论是否发生异常，最后这个都会被执行。
**异常：Exception in thread "main" java.lang.Error: Unresolved compilation problem？**
解决：反复看都没有找到错误，是因为缺少了包名，package test; 这个一定要放在第一句的！

**异常：如果是语法错误，如少了一个分号，则会出现：java.lang.Error**

**异常：java.lang.ArithmeticException，除零异常等**


### 基本语法概念
**问：switch(x)语句中，x可以是哪些类型？**
答：包括：byte、short、int、char、java7后开始支持String、
case 语句中的值的数据类型必须与变量的数据类型相同，而且只能是常量或者字面常量。
case 语句开始执行，直到 break 语句出现才会跳出 switch 语句。


