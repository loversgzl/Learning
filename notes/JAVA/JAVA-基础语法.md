# JAVA-基础知识

* <a href="#简介">简介</a>
* <a href="#基础知识">基础知识</a>
* <a href="#输入输出">输入输出</a>
* <a href="#集合框架">集合框架</a>
* <a href="#字符串">字符串</a>
* <a href="#Java常用包">Java常用包</a>
* <a href="#Java常用内置类">Java常用内置类</a>
* <a href="#异常">异常和调试</a>

****
### 简介
<a name="简介" />
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

### 基础知识
<a name="基础知识"/>
**第一个 Java 程序**

```java
package test; 
/*如果有包需要在第一行注明，注意：导包只可以导当前层，如果里面有包，则包中的类不会导入。
如：import java.*; 和 import java.util.*; 第一个不可能把所有的包中类全导入。
*/
import java.util.Scanner; //引入 JAVA 输入包
public class test {
    public static void main(String args[]) {//所有的 Java 程序主方法入口
    	String name = "tom";
    	System.out.println(name);
    }
}
/*1.创建源文件 .java
2.javac 将 .java 源代码转换为 JVM 能够识别的字节码 .class
3.通过 JVM 执行 .class 文件
编译原理可参考简书：https://www.jianshu.com/p/af78a314c6fc */

/*F3 :可以查看某个函数的源码
放在某个函数上，F2 可以查看具体的使用方法*/

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
4、注意：这里的匹配相当于映射，而不是遍历的查找，因此，case 和 deault 语句顺序无所谓，当匹配后，无break语句，则会从当前位置往后执行。

```java
//return 比 break 效果更强，直接退出。
switch(1) {
    case 1: System.out.println(1);return;
    case 2: System.out.println(2);return;
    case 3: System.out.println(3);return;
    case 4: System.out.println(4);return;
    default:System.out.println("default");return; //这一句放在第一行也是可以的。
}

//逻辑运算符
&：不管&的左边是true还是false，右边都会进行运算 
&&： 只要左边是false，右边就不会进行运算 
一半情况下都会选择&&，因为这样可以提高效率，也可以进行异常处理，当右边产生异常的时候，同样可以跳过。

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
System.out.format("%d %d",1,2); //某些需要有格式的输出，优先使用C++的方式。
//%d 十进制，%s 字符串，%.2f 浮点数控制精度

System.out.print("front" + variable + "end"); //输出不带换行
System.out.format("x:%d,y:%d,radius:%d",x,y,radius); //%d 十进制，%s 字符串
System.out.format("%.2f", pi);//浮点数控制精度的方法，最后一位会自动四舍五入。
System.out.printf(); //C 格式输出
```
****

### 集合框架
<a name="集合框架"/>
![集合框架](../../pics/集合框架.jpg)

由图可见，集合框架主要包括两种类型的容器，以Collection为基类的线性表、以Map为基类的键值对类，前者存储一个元素的集合，后者存储键/值对映射。集合又有三种子类型，List、Set、Queue。List 具体实现类有 Vector、ArrayList、LinkedList；Set具体实现的类有SortedSet、TreeSet、HashSet、LinkedHashSet；Map的具体实现类有：HashMap、HashTable、LinkedHashMap。
图解：虚线框表示接口、实线框表示类、直线+空心三角形=实现接口/继承，虚线+空心三角形=扩展接口，虚线+箭头=依赖，
[详细参考此链接](https://www.jianshu.com/p/57620b762160)
接口：Collection、List、Set、Map，之所以定义多个接口是为了以不同的方式操作集合对象。
还有一些在集合框架出现以前的数据结构：stack

数组缺点：固定长度，数据多了不够，少了浪费空间。
容器类：为了解决数组的缺点，如  ArrayList、LinkedList、

**Collections、Arrays常用方法**
```java
/*Collections 容器的工具类，只能是容器，无返回值，改变原List
Collection是  List Set Queue 的接口，不要混淆
*/
import java.util.Collections;
List<Integer> numbers = new ArrayList<>();
Collections.max(numbers);//最大值
Collections.min(numbers);//最小值
Collections.indexOf(100);//定位
Collections.sort(numbers);//正序排序
Collections.reverse(numbers); //倒转数组,
Collections.shuffle(numbers);//混淆
Collections.swap(numbers,0,1);
Collections.rotate(numbers,2);//所有元素向右循环移动 2 位
Collections.synchronizedList(numbers);//将线程不安全的改为安全的，还有Set,Map等，但是没有String

/*Arrays 是数组的工具类*/
Arrays.sort(arr); //给数组排序
```

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

/*普通数组
1、值相同的数组，引用也不相同。
2、排序函数
3、数组赋值*/
import java.util.Arrays;
Arrays.sort(nums);//排序函数

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

//复制ArrayList,而不传引用
res.add(new ArrayList(oldList));

//将List转换为String[] 数组。
List<String> res = new LinkedList<>();
res.toArray(new String[res.size()]);
List<String> list = Arrays.asList(array);  //数组转ArrayList

//自定义比较函数，比较两个字符串连接顺序后的大小。
Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));

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

/*这种方式存储的是Object对象，很难维护，知道即可。*/
ArrayList list = new ArrayList(); 

/*通过泛型，约定存放同类数据,李氏替换原则，这里需要注意：list可以使用List接口定义的方法，但子类ArrayList的某些方法，如list.removeLast() 就不能用了。*/
List<String> list = new ArrayList<>();

/*增：末尾添加、指定位置添加、添加另一个列表*/
add(object);  add(index,object);  addAll(list);

/*删：删下标，删对象、清空*/
remove(index); remove((Object)index); clear();

/*改：修改某个位置的值、转换为数组*/
set(index,obj); toArray();
/*改：将ArrayList转换为普通数组，但是只能是String，int数组不行*/
String[] array = (String[])list.toArray(new String[size]);
/*改：数组转ArrayList*/
List<String> list=Arrays.asList(array); 

/*查：获取某个位置的对象、某个对象的位置、返回元素个数、返回是否包含元素x、输出字符串形式的数组*/
get(index);  indexOf(obj);  size();  contains(x); toString();

/*LinkedList之双向链表数组，拥有和 ArrayList 一样的方法再加上下面这些*/
import java.util.LinkedList; 
LinkedList<String> sts =new LinkedList<>();
sts.addLast("last1"); //增，在末尾插入，
sts.addFirst("first1");//增，在首部插入
sts.removeLast();sts.removeFirst();//删
sts.getLast(); sts.getFirst();//查



/*三种遍历方式*/
for(String str : list)
    System.out.println(str);

for(int i=0; i<strArray.length; i++)
    System.out.println(strArray[i]);

/*第三种：这种其实才是被推荐使用的，但感觉用的不多。
注意：如果这种方式边遍历边修改，则会出错，在实际项目中也会增加出错的风险。*/
Iterator<String> it = list.iterator();
while(it.hasNext())
    System.out.println(it.next());


/*动态二维数组*/
List<List<Integer>> triangle = new ArrayList<List<Integer>>(); 

/*增：添加一个数组元素、给第一个数组添加一个元素、*/
triangle.add(new ArrayList<>()); 
triangle.get(0).add(1); 

/*改：修改第一个数组的第一个值为 99*/
triangle.get(0).set(0,99);

/*查：获取第一个数组的第一个值、动态数组的大小，注意和静态数组长度的区分*/
triangle.get(0).get(0); 
triangle.size(); 





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


/*在集合框架之前应用的一些类，现在一般用集合框架代替。
Dictionary、Vector、Stack、Properties、*/
/*栈 Stack*/
Stack stack = new Stack(); //初始化，对象默认是 Obejct
Stack<Integer> stack1 = new Stack<Integer>();  //指定类型的初始化
stack.empty(); //判断是否为空，返回true/false
stack.peek(); //取栈顶值（不出栈），返回 Object
stack.push(Object);//进栈，返回 Object
stack.pop();//出栈，返回的是 Object 对象，需要类型转换

/*队列queue*/
import java.util.Queue;
Queue<String> q = new LinkedList<>();//也实现了队列的接口
q.offer("inQueue");//入队列
q.poll();//出队
q.isEmpty();//判断是否队空
q.peek();//查看队首，但不取出

/*Java中PriorityQueue通过二叉小顶堆实现，可以用一棵完全二叉树表示。
LinkedBlockingQueue是一个可选有界队列，不允许null值
PriorityQueue是一个无界队列，不允许null值，入队和出队的时间复杂度是O（log(n)）
*/
```
****

### 字符串
<a name="字符串"/>
问：字符串拼接，StringBuffer，StringBuilder，concat 和 + 的区别。
答：[参考博客](https://www.cnblogs.com/lojun/articles/9664794.html)

```java
/*
String s="abce"是一种非常特殊的形式,和 new 有本质的区别。它是 java 中唯一不需要 new 就可以产生对象的途径。以String s="abce";形式赋值在 java 中叫直接量,它是在常量池中而不是象 new 一样放在压缩堆中。这种形式的字符串，在JVM内部发生字符串拘留，即当声明这样的一个字符串后，JVM会在常量池中先查找有有没有一个值为"abcd"的对象,如果有,就会把它赋给当前引用.即原来那个引用和现在这个引用指点向了同一对象,如果没有,则在常量池中新创建一个"abcd",下一次如果有String s1 = "abc";又会将s1指向"abcd"这个对象,即以这形式声明的字符串,只要值相等,任何多个引用都指向同一对象
*/

/*问：java字符串如何比较是否相等？
答：java中字符串的比较：== 是比较地址，equals比较值。
*/

/*是否是同一变量的分析：
和Integer不同，String只要是对象，== 就一定返回false。
注意 + 号，有对象，也会转为对象关系。
String str = "1" + new String("00"); 会产生一个对象。
str = str + 100; 也是成立的。
*/
String s1 = "abc"; //定义的是一个常量，在常量池中。
String s3 = new String("abc"); //定义的是一个变量，在堆中，值是常量不可变。
String s4 = new String("abc"); 
false:s1 == s3; 
true:s3.equals(s1);
false:s3 == s4;

String c = "a";
String d = c + "bc";//会产生新对象
String e = "a" + "bc"; //两个常量的连接操作，还是常量
false: a==d
true: a==e

/*
String str = "";
*/
/*增：*/
String s = strs[0]+11+"22"; 
String s = s2.concat(String.valueOf(11));

/*删：消除两端的空格*/
str.strip(); 

/*改（无论什么操作，请用变量接收，字符串是常量，不会改变）：返回长度、返回某个字符、返回拼接num次的字符串、*/
str.length(); str.charAt(int index);
String newStr = str.repeat(num);
String subStr = str.subString();
String[] strs = preStr.split("\\.");
String.join("",strs);
/*改：将List转换为String[] 数组、数组转ArrayList*/
List<String> res = new LinkedList<>();
res.toArray(new String[res.size()]);
List<String> list = Arrays.asList(array); 

/*查：*/
char ch = str.charAt(index);
int index = str.indexOf(obj);


/*
StringBuilder sb = new StringBuilder();
*/
/*增：在末尾添加串、在指定位置添加字符串*/
sb.append("abcd"); sb.insert(0,9);

/*删：删除一个字符 [0,1) 左闭右开、*/
sb.delete(0,1);

/*改：倒转字符串、替换[0,1) 左闭右开、*/
sb.reverse(); sb.replace(0,1,"e");
/*改：[) 左闭右开 返回截取的字符串*/
String s = sb.substring(beginIndex, endIndex); //

/*查：*/



```

**正则表达式**

```java
/*正则表达式的包：import java.util.regex.Pattern;

String content = "字符串"; String pattern = "正则表达式";

Pattern的静态方法：
Pattern.matches(pattern, content); //正则 匹配 字符串，返回boolean
Pattern r = Pattern.compile(pattern); //创建实例
Matcher m = r.matcher(content); // 现在创建 matcher 对象
String newContent = m.replaceFirst(newStr); //用新的字符串，替换第一个匹配的字符串

基本符合：
^ ：匹配输入字符串开始的位置。
$ ：匹配输入字符串结尾的位置。
. : 匹配任意字符
* ：零次或多次匹配前面的字符或子表达式。
+ ：一次或多次匹配前面的字符或子表达式。
? ：零次或一次匹配前面的字符或子表达式。
\d ：数字字符匹配。等效于 [0-9]。
\D ：非数字字符匹配。等效于 [^0-9]。
\w ：匹配任何字类字符，包括下划线。与"[A-Za-z0-9_]"等效。
例：匹配小数和整数 【^\d+(\.\d+)?】
例：匹配字符串含有多个空格 【this\s+is\s+text】
*/

import java.util.regex.Pattern;
public static void main(String[] args) {
    //示例一
    String content = "I am noob " + "from runoob.com.";
    String pattern = ".*runoob.*";
    boolean isMatch = Pattern.matches(pattern, content); 
    System.out.println("字符串中是否包含了 'runoob' 子字符串? " + isMatch);

	// 示例二
	String line = "This order was placed for QT3000! OK?";
	String pattern = "(\\D*)(\\d+)(.*)";
    /*注意：括号是捕获组的定义，有三个表达式组合作为一个表达式，所以调换顺序也会有很大影响
    并且，是分别不过，第一个匹配，第二，第三，所以group(0) = 1+2+3.
    */
	
	Pattern r = Pattern.compile(pattern);
	Matcher m = r.matcher(line);
	if (m.find( )) { //后面三个加起来应该等于第一个的表达式。
	   System.out.println("Found value: " + m.group(0) );//表示整个表达式匹配的内容
	   System.out.println("Found value: " + m.group(1) );//第一个左括号匹配到的内容
	   System.out.println("Found value: " + m.group(2) );//第二个左括号匹配到的内容
	   System.out.println("Found value: " + m.group(3) );//第三个左括号匹配到的内容
	} else {
	   System.out.println("NO MATCH");
	}
}


```
****

### Java常用内置类
<a name="Java常用内置类"/>
**Math、Number、Character、Random、StringBuilder、StringBuffer**

**Number类**
Java 语言是一个面向对象的语言，但是 Java 中的基本数据类型（原生类）却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)，其中六个包装类都是抽象类 Number 的子类。byte（8）、short（16）、int（32）、long（64）、float（32）、double（64）、boolean、char（16）
**原生类**：Java中，数据类型分为基本数据类型（或叫做原生类、内置类型）和引用数据类型。
Java不是纯的面向对象的语言，不纯的地方就是这些基本数据类型不是对象。当然初期Java的运行速度很慢，但是基本数据类型能在一定程度上改善性能。如果你想编写纯的面向对象的程序，用包装器类是取代基本数据类型就可以了。六种数字类型都是有符号的，固定的存储空间正是Java可移植性、跨平台的原因之一；
基本类型的存在导致了Java OOP的不纯粹性。因为基本类型不是对象，一切皆对象是个小小的谎言。这是出于执行效率的权衡。

```java
/*
问：Boolean属于关键字么？
答：不属于，只是java中的包装类，关键字应该更广泛一些，属于某种类型，这个应该和标识符类似，只是系统已经构造好的类的名称。

问：int i = null?
错误，null 表示没有地址，可以赋值给引用变量，不能赋值给基本类型。

问：java中的变量不一定要初始化？
答：错误，一定会初始化，全局变量是系统自动做的，对应基本类型顺序，0，0，0，0L，0.0f，0，'u0000',false
所有引用类型都是null。局部变量要手动，否则直接报错。

问：基本数据类型在堆栈上分配？
答：是，八个基本数据类型不能看作对象（这点很特殊）。栈内操作速度快，创建销毁很容易。八个基本数据类型都有对应的包装类，包装类就是对象了。比如Integer j = new Integer（10）。j属于对象的引用，引用放在栈中，而实际的数据10 则放在堆中。 （堆区适合存放大的数据对象，但是操作速度远远不及栈中）（提示：对象的销毁---对象的引用放在栈中，所以使用完引用就被从栈中销毁了，但是实际的对象仍然存放在堆中，只有在没有任何的引用使用它的时候才被垃圾回收器销毁掉）
注意：局部变量是在栈上分配的，且没有默认值，必须初始化才可以使用！

问：count = 0; count = count++ 时，count是多少？
答：0，赋值操作是最后执行的，那么赋值之前的一步是将0给count，count++是在将0给count之后再加，所以编译器不再执行++。

问：byte b1=1,b2=2,b3; b3 = b1 + b2; 报错？
答：java中byte,short,char进行计算都会自动提升为int，所以需要强制类型转换，不能赋给b3。
*/

/*基本数据类型 和 包装类*/
byte;short;int;long;float;double;char;boolean;
Byte;Short;Integer;Long;Float;Double;Character;Boolean;
48个关键字，2个保留字[goto,const]，3个特殊直接量[true,false,null]

/*基本数据类型赋值，虽然每个数据类型都有一个默认值，但最好主动为每个初始值设置默认值。*/
final double PI = 3.1415927; //常量 final 通常常量全部大写
float one = 3.0f; //默认是double，要加f。
int octal = 011;//八进制
int hexa = 0x11;//十六进制
int gap = 'z' - 'a';
boolean b = true; //小写，默认false
char c1 = 'a'; char c2 = 97 //c1和c2都表示一个字符a
String st = "this is a line"; //双引号
Object ob = null; //空表示
&& || ! //逻辑运算符，与或非
    
/*Character 类*/
import java.lang.Character;
char ch = 'a';
Character.isDigit(ch);
Character.isLetter(ch);
Character.isUpperCase(ch);
Character.isLowerCse(ch);

/*包装类的常量，每个都有,二进制位数、最小值、最大值127*/
Byte.SIZE; 
Byte.MIN_VALUE; 
Byte.MAX_VALUE; 

/*注意boolean和int类型不可相互转化和使用，不管在if语句或其他什么地方，但是char可以。
自动类型转换：低  ------------------------------------>  高
byte,short,char—> int —> long—> float —> double 
java核心卷I中43页有如下表述：两个数值进行二元操作时，会有如下的转换操作： 
  如果两个操作数其中有一个是double类型，另一个操作就会转换为double类型。 
  否则，如果其中一个操作数是float类型，另一个将会转换为float类型。 
  否则，如果其中一个操作数是long类型，另一个会转换为long类型。 
  否则，两个操作数都转换为int类型。 
  故，x==f1[0]中，x将会转换为float类型。*/

/*强制类型转换*/
long a = 100; 
int b = (int)a;//强制类型转换通常的方法一，大 转 小。
char ch = 'G';
ch = (char)(ch+32); //大写转小写，强制转换外面套一层即可

String str = "123";
int intVal = Integer.valueOf(str); //通过封装类进行数据转换,(int)str 这种方式会报错，这种适用于Number类
String str = Integer.toString(intVal); //将整数转为字符串
String st = String.valueOf(intVal); //通过封装类进行数据转换

/*装箱与拆箱，将基本数据类型包装为包装类，相加时再转为基本数据类型。
1、int == Integer/new Integer() ,永远都是true，会自动拆箱；
2、new Integer() == new Integer(), 永远是false，两个对象的地址永远不同；
3、Integer == Integer，这种没有显示调用装箱，因此在[-128,127]之间为true，之外会自动构建对象为false；*/
Integer a = 99; //等价于int a = 99; 超过[-128,127]，等价于new Integer();
Integer c = new Integer(a);//装箱，对象形式
Integer b = Integer.valueOf(a);//装箱，该方法返回参数的原生Number对象。
int d = b.intValue(); //拆箱，可以拆成任意 Number 类
a = Integer.parseInt("1024");//字符串转整数

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

**Math类**
```java
Math.max(a,b); //选取最大值，参数为 2.
//利用三目运算符，三数取最大值。
Math.max(maxValue*x, (x > minValue*x) ? x : minValue*x); 
int n = Math.pow(a,b); //a 的 b 次方。
//向下取整，向上取整，四舍五入（内置为+0.5，向下取整），
Math.ceil(11.6); //12.0
Math.floor(11.6); //11.0
Math.round(-11.6); //-12 整数
Math.sqrt(x); //求平方根

```

**Random 类**
```java
import java.util.Random;//重新创建随机数生成器
Random rand = new Random(11);//需要种子
int i = rand.nextInt(100);//产生[0,100) 的随机数
int num = (int)(Math.random()*100);//取[0,100)的随机数，一般这种写法，生成[0.0,1.0)的double数
```

**引用**
1、强引用：一个对象赋给一个引用就是强引用，比如new一个对象，一个对象被赋值一个对象。    2、软引用：用SoftReference类实现，一般不会轻易回收，只有内存不够才会回收。    
3、弱引用：用WeekReference类实现，一旦垃圾回收已启动，就会回收。   
4、虚引用：不能单独存在，必须和引用队列联合使用。主要作用是跟踪对象被回收的状态。
对于一个对象来说，只要有强引用的存在，它就会一直存在于内存中；如果一个对象仅持有虚引用，那么它就和没有引用一样，在任何时候都可能被垃圾回收机回收；如果一个对象只具有软引用，则内存空间足够，垃圾回收器就不会回收，如果内存空间不足了，就会回收这些对象的内存；一旦发现只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的空间。

**泛型**
泛型仅仅是java的语法糖，它不会影响java虚拟机生成的汇编代码，在编译阶段，虚拟机就会把泛型的类型擦除，还原成没有泛型的代码，顶多编译速度稍微慢一些，执行速度是完全没有什么区别的.

**正则表达式**
 ^：起始符号，^x表示以x开头
 $：结束符号，x$表示以x结尾
 [n-m]：表示从n到m的数字
 \d：表示数字，等同于[0-9]
 X{m}：表示由m个X字符构成，\d{4}表示4位数字

 15位身份证的构成：六位出生地区码+六位出身日期码+三位顺序码
 18位身份证的构成：六位出生地区码+八位出生日期码+三位顺序码+一位校验码

 C选项的构成：
 [1-9]\d{5}：六位出生地区码，出生地区码没有以0开头，因此第一位为[1-9]。
 [1-9]\d{3}：八位出生日期码的四位年份，同样年份没有以0开头。
 ((0\d)|(1[0-2]))：八位出生日期码的两位月份，| 表示或者，月份的形式为0\d或者是10、11、12。
 (([0|1|2]\d)|3[0-1])：八位出生日期码的两位日期，日期由01至31。
 \d{4}：三位顺序码+一位校验码，共四位。
 A选项的构成：
 [1-9]\d{7}：六位出生地区码+两位出生日期码的年份，这里的年份指后两位，因此没有第一位不能为0的限制，所以合并了。
 后面的与C选项类似了。
 好吧其实我也是第一次知道身份证还有15位的。


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


/*try catch finally 中包含return的几种情况，及返回结果*/
参考链接：https://www.cnblogs.com/sunshineweb/p/7656463.html
第一种：try发生异常时，立即跳转到catch，执行里面的return操作。
第二种：上面情况下，finally里有对上述返回值的操作，但返回值不变（基本数据类型，如果是引用则另说）。
第三种：finally里面也有return语句，则会捷足先登先返回。try-catch中的语句也会执行，只是存起来，未返回

```
* **问：当某个线程抛出OutOfMemoryError时，其他线程有可能不受影响？**
答：是的，在程序内存溢出之后，溢出内存的线程所占的内存会被快速释放。

* **问：下面哪个行为被打断不会导致InterruptedException？**
答：API里面写的：当线程在活动之前或活动期间处于正在等待、休眠或占用状态且该线程被中断时，抛出该异常。Thread.suspend不会。

**Eclipse调试**
[参考博客](https://www.cnblogs.com/sjxbg/p/9768597.html)
1.表示当前实现继续运行直到下一个断点，快捷键为 F8。
2.表示打断整个进程。
3.表示进入当前方法，快捷键为 F5。
4.表示运行下一行代码，快捷键为 F6。
5.表示退出当前方法，返回到调用层，快捷键为 F7。
6.表示当前线程的堆栈，从中可以看出在运行哪些代码，并且整个调用过程，以及代码行号。



### Java常用包
<a name="Java常用包"/>

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
