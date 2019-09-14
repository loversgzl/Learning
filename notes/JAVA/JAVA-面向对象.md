# JAVA-面向对象

**源文件声明**
一个源文件中只能有一个 public 类 且 源文件的名称应该和此类名相同，可以有多个非 public 类。此源文件属于哪个包，就在首行标明 package 包名；

**JAVA 包（package）**：包主要用来对 类和接口 进行分类。当开发 Java 程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。有关联关系的类放在一个源文件中，该文件属于哪个包，那么就在该源文件的首行写 package 语句。而不是你想象的那样将许多源文件打包。

**import**：在 Java 中，如果给出一个完整的限定名，包括包名、类名，那么 Java 编译器就可以很容易地定位到源代码或者类。Import 语句就是用来提供一个合理的路径，使得编译器可以找到某个类。如果源文件包含 import 语句，那么应该放在 package 语句和类定义之间。

**详解：比如说有一个 .java 的文件，这个文件里面有很多类，要是由若干个上述的文件，在文件的第一行有 package 包名 语句，如果 Package 语句相同，就是相同的包，不同就是不同的包。



**4 种访问控制修饰符**
**默认的【default 】**：在同一包内可见，不使用任何修饰符。
**私有的【private】**：在同一类内可见。主要用来隐藏类的实现细节和保护类的数据。类和接口不能声明为 private，
**共有的【public】**：对所有类可见。Java 程序的 main()  方法必须设置成公有的，否则，Java 解释器将不能运行该类。
**受保护的【protected】**：对同一包内的类和所有子类可见。
访问范围：【public】> 【protected】> 【default 】> 【private】


**非访问修饰符**
为了实现一些其他的功能，Java也提供了许多非访问修饰符。
** static 修饰符**：用来创建类方法和类变量。
```java
private static int numInstances = 0 ; //构造一个私有的静态变量
private static void addInstance(){ //构造一个私有的静态函数
	numInstances++;
}
```

**final 修饰符**：用来修饰类、方法和变量，final修饰的类不能够被继承，修饰的方法不能被继承类重新定义，修饰的变量为常量，是不可修改的。
```java
public static final int BOXWIDTH = 10; //声明一个常量，不可变，默认关键字是 default
```

**abstract 修饰符**：用来创建抽象类和抽象方法。抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。如果一个类包含抽象方法，那么该类一定要声明为抽象类。抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。任何继承抽象类的子类必须实现父类的所有抽象方法，除非该子类也是抽象类。
```java
abstract class Caravan{ //抽象类
	private double price;
	public abstract void addPrice(); //抽象方法
}
```


**synchronized 和 volatile 修饰符**：主要用于线程的编程。synchronized 关键字声明的方法同一时间只能被一个线程访问。Synchronized 修饰符可以应用于四个访问修饰符。
volatile 修饰的成员变量在每次被线程访问时，都强迫从共享内存中重读该成员变量的值。而且，当成员变量发生变化时，强迫线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。一个 volatile 对象引用可能是 null。
```java
public synchronized void showDetails(){} 
```


**继承（extends）**
* 父类中声明为 public 的方法在子类中也必须为 public。
* 父类中声明为 protected 的方法在子类中要么声明为 protected，要么声明为 public。不能声明为private。
* 父类中声明为 private 的方法，不能够被继承。







