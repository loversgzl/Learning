# JAVA-基础知识

* <a href="#简介">简介</a>
* <a href="#基本概念">基本概念</a>
* <a href="#输入输出">输入输出</a>
* <a href="#集合框架">集合框架</a>
* <a href="#字符串">字符串</a>
* <a href="#Java常用包">Java常用包</a>
* <a href="#Java常用内置类">Java常用内置类</a>
* <a href="#异常">异常和调试</a>

****
### 简介
<a name="简介"></a>
由 SUN 公司 詹姆斯-高斯林 开发，后被 甲骨文（Oracle）收购。
它既有能开发桌面应用的 Java SE（Java Platform，Standard Edition）
也有开发 Web 应用的 Java EE（Java Platform，Enterprise Edition）
还有 Android 开发移动应用 和 嵌入式 的 Java ME（Java Platform，Micro Edition）
[JAVA SE EE ME具体区别](https://blog.csdn.net/qq_29611345/article/details/102384776)
[JAVA 各个版本的新增特性](https://blog.csdn.net/qq_22194659/article/details/86134443)

**知名应用**：我的世界、淘宝网、Android 操作系统。
**Java容器**：很多繁琐又重复的工作我们可以提前做好，然后调用，但谁来做呢，有个组织出来定义了接口，谁家都可以造轮子，用户想用哪家的都可以，各家自己造的轮子（如omcat、GlassFish、IBM WebSphere）就叫做 Java 容器。随着越来越多的企业加入到整个阵营，官方给出的规范组件并不是最受欢迎的，反而一些企业的组件在实际开发中更让人喜欢。
**JDK**：JDK（Java Development Kit）称为 Java 开发包 或 Java 开发工具，是一个编写 Java 的 Applet 小程序和应用程序的程序开发环境。JDK 是整个 Java 的核心，包括了 Java 运行环境 JRE（Java Runtime Envirnment），JVM 和 Java 的核心类库（Java API）。
**JAR**： java 类库的 class 文件。
[JDK 和 JRE 的区别](https://blog.csdn.net/shaochenshuo/article/details/78507132)

****

### 基本概念
<a name="基本概念"></a>
**第一个 Java 程序**
```java
package test; //如果有包需要在第一行注明
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

**命名**
**类名**：驼峰命名法，所有单词首字母大写
**方法名**：第一个单词小写，其余单词首字母大写
**源文件名**：源文件名必须和类名相同，文件名的后缀为.java。（如果文件名和类名不相同则会导致编译错误）。
**关键字与保留字**：Java 保留字是指现在Java版本尚未使用，但以后版本可能会作为关键字使用。所以注意false、true、null等都是保留字。
**Java标识符**：字母、数字、下划线、美元符（$）、数字不能作为首位。

**问：switch(x)语句中，x可以是哪些类型？**
答：包括：byte/short/int/char、enum枚举、java7后开始支持String、
注意事项：1、case 语句中的值的数据类型必须与变量的数据类型相同.
2、case 语句开始执行，直到 break 语句出现才会跳出 switch 语句，匹配到哪一个case就从哪一个位置向下执行，直到遇到了break或者整体结束为止。
3、多个case后面的数值不可以重复。
4、注意try-catch-finally的语法。
```java
//return 和 比 break 效果更强，直接退出。
switch(1) {
    case 1: System.out.println(1);return;
    case 2: System.out.println(2);return;
    case 3: System.out.println(3);return;
    case 4: System.out.println(4);return;
    default:System.out.println("default");return;
}
```
****

### 输入输出
```java
//输入
import java.util.Scanner;
Scanner scan = new Scanner(System.in);
int one = scan.nextInt(); //一个一个读，中间的空格和回车视为一次输入的结束，所以纯数字输入优先。
String str = scan.nextLine(); //一行一行读
char ch = (char)System.in.read();
ch = (char)(ch+32); //大写转小写

if(scan.hasNext()){//只读取一次，遇到空格或者回车结束
	String str1 = scan.next();//读取字符串
}

//输出
System.out.println(); 
//输出带换行，且输出的是字符串，如果不是，默认调用toString()方法。如果是基本数据类型，则会先进行装箱，再调用。

System.out.print("front" + variable + "end"); //输出不带换行
System.out.println(String.format("x:%d,y:%d,radius:%d",x,y,radius)); //%d 十进制，%s 字符串
System.out.println(String.format("%.2f", pi));//浮点数控制精度的方法，最后一位会自动四舍五入。
System.out.printf(); //C 格式输出
```
****

### 集合框架
<a name="集合框架"></a>
![集合框架](../../pics/集合框架.jpg)

由图可见，集合框架主要包括两种类型的容器，以Collection为基类的线性表、以Map为基类的键值对类，前者存储一个元素的集合，后者存储键/值对映射。集合又有三种子类型，List、Set、Queue。List 具体实现类有 Vector、ArrayList、LinkedList；Set具体实现的类有SortedSet、TreeSet、HashSet、LinkedHashSet；Map的具体实现类有：HashMap、HashTable、LinkedHashMap、HashTree。
图解：虚线框表示接口、实线框表示类、直线+空心三角形=实现接口/继承，虚线+空心三角形=扩展接口，虚线+箭头=依赖，
[详细参考此链接](https://www.jianshu.com/p/57620b762160)
接口：Collection、List、Set、Map，之所以定义多个接口是为了以不同的方式操作集合对象。
还有一些在集合框架出现以前的数据结构：stack

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
线程安全的集合对象，分别对应线性表，键值对，字符串。
Vector 线程安全：
HashTable 线程安全：
StringBuffer 线程安全：
其他都是非线程安全的集合框架
*/

/*Collections 容器的工具类，包含操控集合的常用方法
//Collection是  List Set Queue 的接口，不要混淆
*/

//Collections是一个类，容器的工具类，只能是容器，无返回值，改变原List
import java.util.Collections;
List<Integer> numbers = new ArrayList<>();
Collections.sort(numbers);//正序排序
Collections.reverse(numbers); //倒转数组,
Collections.shuffle(numbers);//混淆
Collections.swap(numbers,0,1);
Collections.rotate(numbers,2);//所有元素向右循环移动 2 位
Collections.synchronizedList(numbers);//将线程不安全的改为安全的，还有Set,Map等

/*
普通数组
1、值相同的数组，引用也不相同。
*/
//排序函数
import java.util.Arrays;
Arrays.sort(nums);

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


/*List
问：ArrayList 与 LinkedList 的区别？
相同：都是List接口的实现类
不同：ArrayList存储结构是线性表，LinkedList存储结构是链表。因此插入，删除，访问速度遵循存储结构的特性。
LinkedList是一个双向链表, 当数据量很大或者操作很频繁的情况下，添加和删除元素时具有比ArrayList更好的性能。但在元素的查询和修改方面要弱于ArrayList。
*/

/* 
ArrayList动态数组
List 是一个接口，而 ArrayList 是 List 接口的一个实现类。 ArrayList 类继承并实现了 List 接口。 因此，List 接口不能被构造，也就是我们说的不能创建实例对象，但是我们可以像下面那样为 List 接口创建一个指向自己的对象引用，而 ArrayList 实现类的实例对象就在这充当了这个指向List接口的对象引用。 
问：为什么ArrayList常用，Vector不常用？
Vector是线程安全的， Arraylist 是线程不安全的 所以在插入等操作中， Vector需要一定开销来维护线程安全，而大多数的程序都运行在单线程环境下，无须考虑线程安全问题，所以大多数的单线程环境下ArrayList 的性能要优于 Vector。
Vector和ArrayList一样都是基于数组的。
*/
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
ArrayList list = new ArrayList(); //这种方式存储的是Object对象，很难维护，知道即可。
List<String> list = new ArrayList<>();//通过泛型，约定存放同类数据
list.add("hello");list.add("java");

add(object);  add(index,object);  addAll(list);//增：末尾添加、指定位置添加、添加另一个列表、
remove();  clear();//删：下标或对象、清空、
set(index,obj); toArray();//改：修改某个位置的值、转换为数组、
//查：获取某个位置的对象、某个对象的位置、返回元素个数、返回是否包含元素x、
get(index);  indexOf(obj);  size();  contains(x); 
toString();//输出：以字符串逗号隔开的形式输出。

//三种遍历方式
for(String str : list)//第一种
    System.out.println(str);
for(int i=0; i<strArray.length; i++)//第二种
    System.out.println(strArray[i]);
/*第三种：这种其实才是被推荐使用的，但感觉用的不多。
注意：如果这种方式边遍历边修改，则会出错，在实际项目中也会增加出错的风险。
*/
Iterator<String> it = list.iterator();
while(it.hasNext())
    System.out.println(it.next());


//动态二维数组
List<List<Integer>> triangle = new ArrayList<List<Integer>>(); //构造二维数组，长度不固定
triangle.add(new ArrayList<>()); //增，添加一个数组元素
triangle.get(0).add(1); //增，给第一个数组添加一个元素
triangle.get(0).set(0,99); //改，修改第一个数组的第一个值为 99
triangle.get(0).get(0); //查，获取第一个数组的第一个值
triangle.size(); //查，动态数组的大小，注意和静态数组长度的区分

/*
LinkedList之双线链表数组，拥有和 ArrayList 一样的方法再加上下面这些
*/
import java.util.LinkedList; 
LinkedList<String> sts =new LinkedList<>();
sts.addLast("last1"); //增，在末尾插入，
sts.addFirst("first1");//增，在首部插入
sts.removeLast();sts.removeFirst();//删
sts.getLast(); sts.getFirst();//查


/* 集合 Set
问：如何去除数组中重复的元素？
方法一：只需要创建一个集合，然后遍历数组逐一放入集合，只要在放入之前用 contains()方法判断一下集合中是否已经存在这个元素就行了，然后用 toArray 转成数组一切搞定。 
方法二：最简单的方法就是利用Set集合无序不可重复的特性进行元素过滤。
SortedSet接口继承自Set，他根据对象的比较顺序（可以是自然顺序，也可以是自定义的顺序），而不是插入顺序进行排序；
TreeSet是SortedSet的唯一实现类，红黑树实现，树形结构，它的本质可以理解为是有序，无重复的元素的集合。
因为都是有序的，所以相应的就有get，remove和add方法。HashSet看他的源码可以知道，他的底层，是hashmap。
LinkedHashSet，维护的是插入时的顺序；

注意：在使用Set时，如果存放的不是基本数据类型，而是自定义的类，那么一定要继承Comparable接口，重写compareTo方法，否则Set无法去重，TreeSet也会根据该方法区分大小，进行排序，还要重写equals方法，一般包装类都重写了此方法，否则会调用父类Obeject中的equals方法，比较地址而不是值了。

问：TreeSet 和 HashSet 有什么区别？
答：HashSet 是基于哈希表实现的，允许存在一个null值，插入一个值时会调用HashCode()方法，生成HashCode值，来进行相同元素的区分，但它却不能保证插入次序与遍历次序的一致性，因此才有了LinkedHashSet，也是采用HashCode值方式存储，但多用了链表的方式来保证插入与遍历次序的一致性。
TreeSet 是 SortedSet 接口的唯一实现类，它是用二叉树存储数据的方式来保证存储的元素处于有序状态。但是TreeSet不允许插入null值。
*/

import java.util.HashSet;
Set<Integer> set = new HashSet();  //实例化一个set集合  
for (int x : arr)//增
    set.add(arr[i]);  //遍历数组并存入集合,如果元素已存在则不会重复存入 
return set.toArray();  //返回Set集合的数组形式
set.remove(x); //删除
set.contains(x);//查：返回是否包含x、

import java.util.TreeSet;
TreeSet<Integer> set = new TreeSet<>();//排序的集合
set.subSet(from,true,to,true);//截取某段值，[from,to],两端值看bool函数的取值。


/*
图接口 import java.util.MAP;
四个实现类 import java.util.HashMap、HashTable、HashSet

问：你有没有重写过 HashCode 方法和 equals 方法？
有，有一次在使用HashMap时，key是自定义的类，需要根据ID判断是否是同一个对象而不是根据地址，
如果我们在HashMap的键部分存放自定义的对象，一定要在这个对象中用自己的equals方法和hashCode方法覆盖掉Object中的同名方法。
*/
class key{
    private Integer id;
    public Integer getId(){return id;}
    public key(Integer id){this.id = id;}
    public boolean equals(Object ob){//重写了equals方法
        if(ob==null || !(ob instanceof key)) return false;
        return this.getId().equals(((key)ob).getId);
    }
    public int hashCode(){ return id.hashCode();}//重写了hashCode方法
}


import java.util.HashMap;
HashMap hm = new HashMap(); //这个可以存放多种不同类型的键值对
Map<String,Integer> map = new HashMap<>();
Map<int[],Integer> map = new HashMap<>();

put("Tom",12); putAll(anotherMap); //增，改,如果已经存在，则覆盖
clear();remove(x);//清空、删除某个键、
containsKey("Tom"); //返回Boolean
get("Tom"); getOrDefault(key, 0);

//三种遍历方式
for(String key : map.keySet())
    System.out.println(key+map.get(key));
for(Integer v : map.values())
    System.out.println(v);

for(Map.Entry<String, Integer> entry : map.entrySet())
    System.out.println(entry.getKey()+entry.getValue());

Iterator<Map.Entry<String, Integer>> it = map.entrySet().iterator();
while(it.hasNext){
    Map.Entry<String, Integer> entry = it.next();
    System.out.println(entry.getKey()+entry.getValue());
}

/*
在集合框架之前应用的一些类，现在一般用集合框架代替。
Dictionary、Vector、Stack、Properties、
*/

//栈 Stack
Stack stack = new Stack(); //初始化，对象默认是 Obejct
Stack<Integer> stack1 = new Stack<Integer>();  //指定类型的初始化
stack.empty(); //判断是否为空，返回true/false
stack.peek(); //取栈顶值（不出栈），返回 Object
stack.push(Object);//进栈，返回 Object
stack.pop();//出栈，返回的是 Object 对象，需要类型转换

/*
队列queue
*/
import java.util.Queue;
Queue<String> q = new LinkedList<>();//也实现了队列的接口
q.offer("inQueue");//入队列
q.poll();//出队
q.isEmpty();//判断是否队空
q.peek();//查看队首，但不取出
```
****

### 字符串
<a name="字符串"></a>
问：字符串拼接，StringBuffer，StringBuilder，concat 和 + 的区别。
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)

```java
/*
String s="abce"是一种非常特殊的形式,和 new 有本质的区别。它是 java 中唯一不需要 new 就可以产生对象的途径。以String s="abce";形式赋值在 java 中叫直接量,它是在常量池中而不是象 new 一样放在压缩堆中。这种形式的字符串，在JVM内部发生字符串拘留，即当声明这样的一个字符串后，JVM会在常量池中先查找有有没有一个值为"abcd"的对象,如果有,就会把它赋给当前引用.即原来那个引用和现在这个引用指点向了同一对象,如果没有,则在常量池中新创建一个"abcd",下一次如果有String s1 = "abc";又会将s1指向"abcd"这个对象,即以这形式声明的字符串,只要值相等,任何多个引用都指向同一对象
*/

/*问：java字符串如何比较是否相等？
答：java中字符串的比较：== 是比较地址，equals比较值。
*/
String s1 = "abc"; //定义的是一个常量，在常量池中。
String s2 = "abc"; 
String s3 = new String("abc"); //定义的是一个变量，在堆中，值是常量不可变。
String s4 = new String("abc"); 
true:s1 == s2;
false:s1 == s3; true:s3.equals(s1);
false:s3 == s4;

//对于 + 号的字符串分析
String a = "abc";
String c = "a";
String d = c + "bc";//会产生新对象
String e = "a" + "bc"; //两个常量的连接操作，还是常量
false: a==d
true: a==e

//注意！因为String字符串是常量，所以下面的所有操作都需要另外声明一个String变量接收，否则等于无效。

//String类型是常量，但下面不是，可以更改，内置类型，无需引包
StringBuilder sb = new StringBuilder();
StringBuffer br = new StringBuffer();

sb.append("abcd");//在末尾添加串
sb.delete(0,1);//删除一个字符，[0,1)，左闭右开，显示 bcd
sb.reverse(); //倒转字符串，dcba
sb.insert(0,9); //首位插入 9，9abcd
sb.replace(0,1,"e");//替换[0,1)，左闭右开
char ch = sb.charAt(index); //返回某个位置的字符!!很容易忘
int length = sb.length();
//注意这里的区别，sb截取的字符串返回的是String，不会更改sb的值和String一样和上面不同。
String s = sb.substring(beginIndex, endIndex); //[)返回此坐标开始后的字符串

//字符串，整数，互换
String preStr = "192.168.1.1"; 
String[] strs = preStr.split("\\."); //正确写法。对小圆点进行转义

//拼接字符串
String s = strs[0]+11+"22"; //一开始就可以确定的量，使用 + 更快
//循环中切忌使用+来拼接字符串，StringBuilder 最好。
String s2 = s2.concat(String.valueOf(i));
//list 拼接
List<String> list = new ArrayList<>();
for (int i = 0; i < 10000; i++)
	list.add(String.valueOf(i)); 
StringUtils.join(list, ""); //将字符串数组拼接
//StringBuffer 拼接
StringBuffer sb = new StringBuffer(); //StringBuilder 类似
for (int i = 0; i < 100000; i++)
	sb.append(String.valueOf(i));
sb.toString();
```

**正则表达式**

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
****

### Java常用包
<a name="Java常用包"></a>
```java
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
java.lang 包，每个程序自动载入的
*/
import java.lang.Math;
```

### Java常用内置类
<a name="Java常用内置类"></a>
**Math、Number、Character、Random、StringBuilder、StringBuffer**

**Math类**
```java
Math.max(a,b); //选取最大值，参数为 2.
//利用三目运算符，三数取最大值。
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); 
int n = Math.pow(a,b); //a 的 b 次方。
//向下取整，向上取整，四舍五入（内置为+0.5，向下取整），
Math.floor(11.5); Math.ceil(11.5); Math.round(-11.5); 
Math.sqrt(x); //求平方根

```

**Number类**
Java 语言是一个面向对象的语言，但是 Java 中的基本数据类型（原生类）却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)，其中六个包装类都是抽象类 Number 的子类。byte（8）、short（16）、int（32）、long（64）、float（32）、double（64）、boolean、char（16）
**原生类**：Java中，数据类型分为基本数据类型（或叫做原生类、内置类型）和引用数据类型。
Java不是纯的面向对象的语言，不纯的地方就是这些基本数据类型不是对象。当然初期Java的运行速度很慢，但是基本数据类型能在一定程度上改善性能。如果你想编写纯的面向对象的程序，用包装器类是取代基本数据类型就可以了。六种数字类型都是有符号的，固定的存储空间正是Java可移植性、跨平台的原因之一；
基本类型的存在导致了Java OOP的不纯粹性。因为基本类型不是对象，一切皆对象是个小小的谎言。这是出于执行效率的权衡。

```java
//基本数据类型 和 包装类
byte;short;int;long;float;double;char;boolean;
Byte;Short;Integer;Long;Float;Double;Character;Boolean;

//问：第二行Boolean等属于关键字么？
//答：不属于，只是java中的包装类，关键字应该更广泛一些，属于某种类型，这个应该和标识符类似，只是系统已经构造好的类的名称。

//问：java中的变量不一定要初始化？
//答：错误，一定会初始化，有些是系统自动做的，对应基本类型顺序，0，0，0，0L，0.0f，0，'u0000',false
//所有引用类型都是null。

//问：基本数据类型在堆栈上分配？
//答：是，八个基本数据类型不能看作对象（这点很特殊），存放在栈中。栈内操作速度快，创建销毁很容易。八个基本数据类型都有对应的包装类，包装类就是对象了。比如Integer j = new Integer（10）。j属于对象的引用，引用放在栈中，而实际的数据10 则放在堆中。 （堆区适合存放大的数据对象，但是操作速度远远不及栈中）（提示：对象的销毁---对象的引用放在栈中，所以使用完引用就被从栈中销毁了，但是实际的对象仍然存放在堆中，只有在没有任何的引用使用它的时候才被垃圾回收器销毁掉）

//装箱与拆箱，将基本数据类型包装为包装类，相加时再转为基本数据类型。
Integer x = 5; x = x +10;

int a = 99; 
Integer c = new Integer(a);//装箱
Integer b = Integer.valueOf(a);//装箱，该方法返回参数的原生Number对象。
int d = b.intValue(); //拆箱，可以拆成任意 Number 类

//包装类的常量
Byte.SIZE; //二进制位数
Byte.MIN_VALUE; //最小值
Byte.MAX_VALUE; //最大值127
short //最大值32767
int //最大值十位
    
//基本数据类型赋值，虽然每个数据类型都有一个默认值，但最好主动为每个初始值设置默认值。
final double PI = 3.1415927; //常量 final 通常常量全部大写
boolean b = true; //小写
char c1 = 'a'; char c2 = 97 //c1和c2都表示一个字符a
String st = "this is a line"; //双引号
Object ob = null; //空表示
&& | ! //逻辑运算符，与或非

//基本数据类型初始化
//注意boolean和int类型不可相互转化和使用，不管在if语句或其他什么地方。
//自动类型转换：低  ------------------------------------>  高
byte,short,char—> int —> long—> float —> double 

/*
强制类型转换，包装类的方法以Integer为例，其它类似：
Integer.parseInt()、Integer.toString()
注意：Integer.valueOf()是装箱，返回的是包装类。
*/
//强制类型转换通常的方法一
long a = 100; 
int b = (int)a;
char ch = 'G';
ch = (char)(ch+32); //大写转小写，强制转换外面套一层即可

String num = "123";
int intVal = Integer.valueOf(num); //通过封装类进行数据转换
int intVal = Integer.parseInt(num); //通过封装类进行数据转换
String st = String.valueOf(intVal); //通过封装类进行数据转换
String st = Integer.toString(intVal); //将整数转为字符串

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


枚举（Enumeration）、位集合（BitSet）、向量（Vector）、栈（Stack）、字典（Dictionary）、哈希表（Hashtable）、属性（Properties）
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


### 异常和调试
<a name="异常"></a>
异常可以分为三类：检查性异常(Exception)、错误(Error)、运行时异常(RuntimeException)、Error 和 Exception它们都继承 Throwable； RuntimeException 继承 Exception，其还有子类SQLException,IOException。

**问：请问error和exception有什么区别?**
**error**： 不可能指望程序能处理这样的情况，比如说内存溢出等，遇到错误建议中止程序。
**exception**： 与错误相比，程序一般可以捕获和处理异常，可以对异常进行简单的处理，如无法连接网络，可以十秒后再次尝试，而不是简单的终止程序。

**处理异常的常用方法**：try、catch、finally，无论是否发生异常，即使catch块中有return，finally也还是会被执行。

**运行时常见异常（RuntimeException）**
此类异常不必须包含在try内，因为可能出现在任何地方，下面这些都是。

**异常：Exception in thread "main" java.lang.Error: Unresolved compilation problem？**
解决：反复看都没有找到错误，是因为缺少了包名，如果有包一定要在首行添加，package test; 这个一定要放在第一句的！
**异常：xxx cannot be resolved to a type**
解决：缺少包，xxx不是一个类或接口。
**异常：java.lang.Error**：语法错误，如少了一个分号
**异常：java.lang.ArithmeticException**：算术异常，例如除零异常
**异常：java.lang.ArrayIndexOutOfBoundsException**：数组越界异常
**异常：java.lang.NullPointerException**：空指针异常

**问：throw,throws,Throwable的区别**
```java
//Throwable 是 Error和Exception的父类。
public class Main {
    public static void checkData(int data) throws Exception {
    	if(data < 0) 
    		throw new Exception("Data Error!");
    }
    
    public static void main(String[] args) {
    	try {
    		checkData(-1);
    	}catch(Exception e) {
    		e.printStackTrace();
    	}finally {
    		System.out.println("every thing is ok");
    	}
    }
}
/*finally块内应放一些回收内存的代码
1-如果连接数据库，需要关闭连接。
2-如果用到了IO对象，则需要关闭。
3-如果用到了ArrayList,LinkedList,HashMap，则需要clear.
4-如果有一个对象obj指向一个大的内存，则可以写obj=null。
*/
```

**Eclipse调试**
[参考博客](https://www.cnblogs.com/sjxbg/p/9768597.html)
1.表示当前实现继续运行直到下一个断点，快捷键为 F8。
2.表示打断整个进程。
3.表示进入当前方法，快捷键为 F5。
4.表示运行下一行代码，快捷键为 F6。
5.表示退出当前方法，返回到调用层，快捷键为 F7。
6.表示当前线程的堆栈，从中可以看出在运行哪些代码，并且整个调用过程，以及代码行号。


