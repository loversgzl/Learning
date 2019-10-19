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
Integer.parseInt(String); //将String字符类型数据转换为Integer整型数据。
int n = Math.pow(a,b); //a 的 b 次方。

//增强型 for 循环
for(String str :strs)
    System.out.println(str);

//返回系统时间
long start = System.currentTimeMillis();

```

### 输入输出
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

### 集合框架与数组
数组缺点：固定长度，数据多了不够，少了浪费空间。
容器类：为了解决数组的缺点，如  ArrayList、
```java
//易混淆点
//数组的长度：.lenght; List的长度：.size(); 字符串的长度：length();
//List添加元素：add(x); 字符串添加元素：append(x);
//List 的获取用 get！不要总是用 [] 数组的东西。
//StringBuilder 用charAt,
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
	contains(x); 返回true/false
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

//字典
import java.util.HashMap;
HashMap<String,Integer> dic = new HashMap<>();
dic.put("Tom",12); //如果已经存在，则覆盖
dic.get("Tom");
dic.clear();

//集合
/*问：如何去除数组中重复的元素？
方法一：只需要创建一个集合，然后遍历数组逐一放入集合，只要在放入之前用 contains()方法判断一下集合中是否已经存在这个元素就行了，然后用 toArray 转成数组一切搞定。 
方法二：最简单的方法就是利用Set集合无序不可重复的特性进行元素过滤。 */
import java.util.HashSet;
Set<Integer> set = new HashSet();  //实例化一个set集合  
 
for (int x : arr)
    set.add(arr[i]);  //遍历数组并存入集合,如果元素已存在则不会重复存入 
return set.toArray();  //返回Set集合的数组形式


//队列queue
import java.util.Queue;
Queue<String> q = new LinkedList<>();//也实现了队列的接口
q.offer("inQueue");//入队列
q.poll();//出队
q.peek();//查看队首，但不取出

//Collection是 Set List Queue 和 Deque 的接口 
//Collections是一个类，容器的工具类，只能是容器，无返回值，改变原List
import java.util.Collections;
List<Integer> numbers = new ArrayList<>();
Collections.reverse(numbers); //倒转数组,
Collections.shuffle(numbers);//混淆
Collections.sort(numbers);
Collections.swap(numbers,0,1);
Collections.rotate(numbers,2);//所有元素向右循环移动 2 位

//栈
Stack stack = new Stack(); //初始化
stack.empty(); //判断是否为空，返回true/false
stack.peek(); //取栈顶值（不出栈），返回 Object
stack.push(Object);//进栈，返回 Object
stack.pop();//出栈，返回的是 Object 对象，需要类型转换

```

### 字符串
问：字符串拼接，StringBuffer，StringBuilder，concat 和 + 的区别。
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)

```java
//查
StringBuilder sb = new StringBuilder();
char ch = sb.charAt(index); //返回某个位置的字符!!很容易忘
int length = sb.length();
String s = sb.substring(beginIndex); //返回此坐标开始后的字符串

//增



//改
//字符串，整数，互换
String preStr = "192.168.1.1"; 
String[] strs = preStr.split("\\."); //正确写法。对小圆点进行转义
String ss = String.valueOf(11); //将数字转为字符串
int a = Integer.valueOf(strs[0]); //将 ip 地址转换为整数
int b = Integer.parseInt(strs[1]); //将 ip 地址转换为整数

//拼接字符串
String s = strs[0]+11+"22"; //一开始就可以确定的量，使用 + 更快
//字符串拼接，循环中切忌使用，StringBuilder 最好。
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

学习代码，查找重复的字符串
```java
    public static void main(String[] args) {
        // s创建一个长度是100的字符串数组
        // s字符串数组排序
        // for循环中采用StringBuilder比用String的 `+=`高效
        // Hashset的add 可以判断有没有添加重复的字符，size()用来计算重复的个数
 
        // s 创建一个长度是100的字符串数组
        // s使用长度是2的随机字符填充该字符串数组
        // s统计这个字符串数组里重复的字符串有多少种
        // s 使用HashSet来解决这个问题
        StringBuilder string = new StringBuilder();// 获取全部的有效字符并存储
        Random rand = new Random();//随机对象
        char ch = 0; //shengcheng 生成一个随机字符
        String[] str = new String[100];
        HashSet<String> set = new HashSet<String>();// 存放数据
        String chongfu = new String();// 拼接重复的字符串
        char c;
        for (short i = 0; i < 128; i++) {
            c = (char) i;
            if (Character.isDigit(c) || Character.isLetter(c)) {
                string.append(c);
            }
        }
        for (int i = 0; i < 100; i++) {
            str[i] = "";
            for (int j = 0; j < 2; j++) {
                int rs = rand.nextInt(string.length()); // 从62抽出一个随机数字
                ch = string.charAt(rs);// 调用charAt()，把随机数字传递实参，生成一个随机字符
                str[i] += String.valueOf(ch);// char转String
            }
            if (set.add(str[i]) == false) { // you 有重复的字符串返false
                chongfu += str[i] + ":" + (i + 1) + " ";// pinjie 拼接重复的字符串
            }
            System.out.printf("%3d:%s ", i + 1, str[i]);
//          System.out.print((i + 1) + ":" + str[i] + " ");
            if ((i + 1) % 10 == 0) {
                System.out.println();
            }
        }
        int len = str.length - set.size();// shuzu 数组的长度减去set的长度就是重复字符串的个数
        if (len != 0) {
            System.out.println("总共有" + len + "种重复的字符串");
            System.out.printf("分別是：%n%s", chongfu);
        } else {
            System.out.println("没有重复的字符串");
        }
    }
```



### 函数
```java

```

### 常用类
**Number 和 Math 类**
装箱与拆箱，Java语言为每一个内置数据类型提供了对应的包装类，所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。
```java
Math.max(a,b); //选取最大值，参数为 2.
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); //利用三目运算符，三数取最大值。
int num = (int)(Math.random()*100);//取[0,100)的随机数，一般这种写法，生成[0.0,1.0)的double数
```

**Character 类**
```java
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
```

**JAVA 常用包**
```java
//java.util 包
import java.util.Arrays;
import java.util.ArrayList;//动态数组
import java.util.List;//接口
import java.util.LinkedList;//链表
import java.util.Hashtable;
import java.util.HashMap;//字典
import java.util.HashSet;//集合
import java.util.Collection;//方法调用,容器的工具类
import java.util.Scanner;//输入
import java.util.Random;//随机数

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


