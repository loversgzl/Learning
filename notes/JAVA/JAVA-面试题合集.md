# JAVA-面试题合集

**问：Java 中的值传递和引用传递？**
错误理解一：值传递和引用传递，区分的条件是传递的内容，如果是个值，就是值传递。如果是个引用，就是引用传递。
错误理解二：Java是引用传递。
错误理解三：传递的参数如果是普通类型，那就是值传递，如果是对象，那就是引用传递。
其实我觉得和 Python 中可变类型与不可变类型的概念类似，具体可参考[Python 五种数据类型](https://blog.csdn.net/qq_29611345/article/details/100736961)
我的理解就是不可变类型是值传递，如（Integer、String）
可变类型是引用传递：如（int[]，ArrayList）

**问：equals 和  ==  的区别？**
前者：不能用于基本数据类型比较，只能用于引用类型，比较内存地址。
后者：基本数据类型比较值，引用类型比较内存地址。

**错误：在循环内进行删除。**
```java
ArrayList<String> list = new ArrayList<String>();  
list.add("a");  
list.add("bb");  
list.add("bb");  
list.add("ccc");  
list.add("ccc");  
list.add("ccc");
//第一种，可能会没有删干净
for (int i = 0; i < list.size(); i++) {  
	String s = list.get(i);  
	if (s.equals("bb")) 
		list.remove(s);  
}
//第二种：出现异常 并发修改异常java.util.ConcurrentModificationException
for (String s : list) {  
	if (s.equals("bb")) 
		list.remove(s);  
}  
```




**第一个 JAVA 程序**

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

boolean b = true; //小写
char g = 'g'; //一个字符
String st = "this is a line"; //双引号

//强制类型转换
long a = 100;
int b = (int)a;

Integer.parseInt(String) //将String字符类型数据转换为Integer整型数据。
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

//java 网络编程常用包
//java.net 包，java.io 包
```

**数据结构**
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




