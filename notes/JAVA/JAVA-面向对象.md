# JAVA-面向对象

**源文件声明**
一个源文件中只能有一个 public 类且源文件的名称应该和此类名相同，可以有多个非 public 类。此源文件属于哪个包，就在首行标明 package 包名；

**JAVA 包（package）**：包主要用来对 类和接口 进行分类。当开发 Java 程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。有关联关系的类放在一个源文件中，该文件属于哪个包，那么就在该源文件的首行写 package 语句。而不是你想象的那样将许多源文件打包。

**import**：在 Java 中，如果给出一个完整的限定名，包括包名、类名，那么 Java 编译器就可以很容易地定位到源代码或者类。Import 语句就是用来提供一个合理的路径，使得编译器可以找到某个类。如果源文件包含 import 语句，那么应该放在 package 语句和类定义之间。

**接口（interface）**：除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。接口不能用于实例化对象，这是肯定的，都是抽象类嘛；接口没有构造方法；**接口中的方法必须是抽象方法（即没有实现的方法如：public abstract void addPrice();其中abstract可以省略），这是和抽象类重要的区别之一；接口不能包含成员变量（除了static和final），不要觉得可以像继承父类那样使用接口；**接口不应叫被继承，而应叫被实现；一个类可以同时实现多个接口（implements）。

**问：不能用来修饰 接口 里的方法的有哪些关键字？**
答：private、protected、java8中开始支持定义静态方法，常用关键字public、static、abstract、default。


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

**abstract 修饰符**：用来创建抽象类和抽象方法。抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。**如果一个类包含抽象方法，那么该类一定要声明为抽象类，但是一个抽象类不一定含有抽象方法**。抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。**任何继承抽象类的子类必须实现父类的所有抽象方法**，除非该子类也是抽象类。抽象类中也可以包含已经实现的方法，这是和接口的一个重要区别。抽象类中的抽象方法要加 abstract 关键字声明，但是接口中可以省略。
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


**继承（extends 或者 implements）**
类继承是用 extends，而接口是用 implements，**虽然只能继承一个类，但是接口可以多继承**。
继承是 JAVA 面向对象编程技术的一块基石，因为它允许创建分等级层次的类。继承可以理解为一个对象从另一个对象获取属性的过程。在 JAVA 中，类的继承是单一继承，也就是说，一个子类只能拥有一个父类。B 是 A 的子类，B 的实例拥有 A 所有的成员变量，但对于 private 的成员变量 B 却没有访问权限，这保障了 A 的封装性。

* 父类中声明为 public 的方法在子类中也必须为 public。
* 父类中声明为 protected 的方法在子类中要么声明为 protected，要么声明为 public。不能声明为private。
* 父类中声明为 private 的方法，不能够被调用。

**问：子类将继承父类所有的数据域和方法？**
正确，在一个子类被创建的时候，首先会在内存中创建一个父类对象，然后在父类对象外部放上子类独有的属性，两者合起来形成一个子类的对象。所以所谓的继承使子类拥有父类所有的属性和方法其实可以这样理解，子类对象确实拥有父类对象中所有的属性和方法，但是父类对象中的私有属性和方法，子类是无法访问到的，只是拥有，但不能使用。就像有些东西你可能拥有，但是你并不能使用。所以子类对象是绝对大于父类对象的，所谓的子类对象只能继承父类非私有的属性及方法的说法是错误的。可以继承，只是无法访问到而已。


**instanceof 判断是否是继承关系**

```java
System.out.println(dog instanceof Animal); //左边是子类 右边是父类
```

**重写（Override）与 重载（Overload）**
面：重载与重写的区别？[参考博客](https://blog.csdn.net/linzhaojie525/article/details/55213010)
答：方法重载是让类以统一的方式处理不同类型数据的一种手段，Java的方法重载，它们具有相同的函数名，但是参数或者返回值会不同，这就是我们常说的多态。重写：1、2、3、4、
1. 子类方法的访问权限必须大于或等于父类方法的访问权限。如果父类的一个方法被声明为 public，那么在子类中重写该方法就不能声明为 protected。
2. 声明为final的方法不能被重写。声明为 static 的方法不能被重写，但是能够被再次声明。
3. 重写 的函数名和参数必须相同，而重载 函数名 相同，但是 参数 或者 返回值 必须有一个不同。
4.  @Override ：不写也可以，但是写了有如下好处，1-可以当注释用，方便阅读；2-编译器可以给你验证 @Override下面的方法名是否是你 **父类** 中所有的，如果没有则报错。例如，你如果没写 @Override，而你下面的方法名又写错了，这时你的编译器是可以编译通过的，因为编译器以为这个方法是你的子类中自己增加的方法。




```java
class Animal {//注意默认是 default，public 只能有一个，且必须与文件名相同
	private int i; //私有成员不能被继承
    @Override //不写也可以，但是写了有上面介绍的好处
	public void move(){
		System.out.println("动物可以移动");
	}
	public void move(int x){//重载了 move 方法
		System.out.println("动物移动了 " + x + " 米");
	}
}

class Dog extends Animal{ 
    @Override
	public void move(){
		super.move(); //调用父类方法，使用 super 关键字
		System.out.println("小狗可以跑");
	}
}

public class test{
	public static void main(String args[]){
		Animal a = new Animal();
		Animal b = new Dog();
		a.move(); //调用父类函数
		b.move(); //调用子类函数
	}
}
```

**问：以下代码可以正常编译并运行么？**
```java
class Test{
	public static void hello(){System.out.println("hello");}
}
public class MyApp{
	public static void main(String[] args){
		Test test = null;
		test.hello();
	}
}

/*
是可以的！！！即使 Test test = null;也会加载静态方法，所以test包含Test类中的初始化数据，静态的，构造的，成员属性。额外，就算不是静态方法，编译也是可以通过的，只有在运行的时候才会报错。
如果一个成员被声明为static，它就能够在它的类的任何对象创建之前被访问，而不必引用任何对象（跟类是否有static修饰无关）。
*/
```




