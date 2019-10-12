# JAVA-高级编程

**JAVA - 高级编程**

* JAVA - 数据结构
* JAVA - 网络编程
* JAVA - 多线程编程
* JAVA - 分布式编程
* JAVA - 框架

**JAVA - 反射**
在对象方法前，加上修饰符 synchronized ，同步对象是当前实例。
那么如果在类方法前，加上修饰符 synchronized，同步对象是这个类的反射。

使用反射方式，首先准备一个配置文件，就叫做 spring.txt 吧, 放在 src 目录下。 里面存放的是类的名称，和要调用的方法名。在测试类Test中，首先取出类名称和方法名，然后通过反射去调用这个方法。当需要从调用第一个业务方法，切换到调用第二个业务方法的时候，不需要修改一行代码，也不需要重新编译，只需要修改配置文件 spring.txt，再运行即可。
这也是 Spring 框架的最基本的原理，只是它做的更丰富，安全，健壮。


**JAVA - I/O**
[参考教程](http://how2j.cn/k/io/io-file/345.html#nowhere)

** I/O 文件对象**
**例题**：一般说来操作系统都会安装在C盘，所以会有一个 C:\WINDOWS目录。遍历这个目录下所有的文件(不用遍历子目录)，找出这些文件里，最大的和最小(非0)的那个文件，打印出他们的名。注: 最小的文件不能是 0 长度。
知识点：1如何创建文件对象，2返回文件夹下所有子文件的函数，3一个文件的长度
```java
import java.io.File; //引入必要的包
public class test{
	public static void main(String args[]){
		File fs = new File("C:/WINDOWS"); //创建一个文件对象
		long max=0, min=Integer.MAX_VALUE;
		File maxName=null, minName=null; //文件对象要初始化。
//该函数以文件数组的形式，返回当前文件夹下的所有文件（不包含子文件及子文件夹）
		for(File f : fs.listFiles()){ 
			if(f.length() > max){
				max = f.length();
				maxName = f;
			}
			if(f.length() < min && f.length() > 0){
				min = f.length();
				minName = f;
			}
		}
		System.out.println("最大的文件为： " + maxName + " 大小为：" + max);
		System.out.println("最小的文件为： " + minName + " 大小为：" + min);
		//输出文件名 maxName.getAbsoluteFile() 也可以。
	}
}
```

练习：对 d 盘的一个文件进行读取。
知识点：1文件输入输出流的创建，
```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class test{
	public static void main(String[] args) {
		try{ //必须对其进行保护，否则会报错
		//File f = new File("F:/aa//bb//cc");
		//f.mkdir(); 防止文件路径不存在，可以创建递归目录。
			File f = new File("F:/test.txt");
			FileInputStream fis = new FileInputStream(f);
			byte[] all = new byte[(int)f.length()];
			fis.read(all);
			for(byte b : all){
				System.out.println(b); //输出asc码
			}
			fis.close(); //每次使用完需要关闭
		} catch(IOException e){
			e.printStackTrace();
		}
	}
}
```

### JAVA - 多线程编程

[参考教程](http://how2j.cn/k/thread/thread-start/353.html#nowhere)
创建多线程有 3 种方式，分别是 继承线程类，实现 Runnable 接口，匿名类，个人感觉第一种比较好，代码耦合度比较低。
注： 启动线程是start()方法，run()并不能启动一个新的线程

**多线程常用方法**
疑问：主线程和子线程的运行顺序如何？？
java：equals 和 == 的区别？？
```java
Thread t1 = new Thread(){
	public void run(){}//匿名函数修改 run 函数
}
t1.start(); //启动线程 t1
t1.sleep(1000); //t1 线程暂停 1 秒
t1.join();
//在主线程中加入该线程，一般是 main 嘛，当 t1 执行完后，才会继续执行主函数
t1.setPriority(Thread.MAX_PRIORITY);
//为线程 t1 设置优先级，优先级越高，有越大的几率获得 CPU 资源。
t1.setDaemon(true); //如果一个进程只剩下守护线程，则该进程会自动结束。
```

**线程同步问题**
知识点：1线程同步的关键字，2同步主线程和子线程的三种方法，
疑问：一个线程运行结束后不会立马销毁么？？？会发生什么？？
疑问：脏数据？？？
```java
Object someObject =new Object();
synchronized (someObject){
  //此处的代码只有占有了someObject后才可以执行
}
```

问：java 实现多个子线程执行完毕后，再执行主线程
知识点：join 和 countDownLatch 进行阻塞
```java
//下面的代码没有实现此功能哦，主线程会先结束，子线程还未执行完毕。
//以下两种做法的区别，参考：https://blog.csdn.net/zhang199416/article/details/70846958
/*
1 final CountDownLatch latch = new CountDownLatch(3); 需要阻塞的线程数量
子线程中添加：latch.countDown();

2 子线程.join();
*/
public class test {
	public static void main(String[] args) {
		System.out.println("主线程开始执行....");
		for (int i = 0; i < 3; i++) {
			new Thread(){
				@Override
				public void run() {
					try {
						System.out.println(Thread.currentThread().getName()+"  开始执行存储过程..");						
						Thread.sleep(1000);
						System.out.println(Thread.currentThread().getName()+"  存储过程执行完毕...");						
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				};
			}.start();
		}
		System.out.println("主线程执行完毕....");
	}
}

```





**JAVA - 网络编程**
[参考W3C 网络编程](https://www.w3cschool.cn/java/java-networking.html)
[参考W3C 网络教程](https://www.w3cschool.cn/java/java-k5ct21dn.html)
能够实现：如何创建服务器套接字、如何创建客户端套接字、

```java

```



### JavaEE 主要技术
[参考博客](https://blog.csdn.net/Neuf_Soleil/article/details/80962686)
JavaEE 号称有十三种核心技术。它们分别是：**JDBC、Servlet、JSP**、JNDI、EJB、RMI、XML、JMS、Java IDL、JTS、JTA、JavaMail和JAF。一般来讲，初学者应该遵循路径 Servlet -> JSP -> Spring -> 组合框架。

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


### JAVA EE 方向

**Servlet**
<img src="../../pics/servlet.png" align="center">
[ 参考博客](https://learner.blog.csdn.net/article/details/81091580)
Servlet 是运行在 Web 服务器或应用服务器上的 Java "小"程序。Servlet 可以动态地生成网页，广义的 Servlet 指任何实现了 Servlet 接口的 Java 程序。
[安装 TomCat](https://www.cnblogs.com/limn/p/9358657.html)

**问：Java Servlet 与使用 CGI（Common Gateway Interface，公共网关接口）有什么优势**：

1. 性能明显更好。
2. Servlet 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求。
3. Servlet 是独立于平台的，因为它们是用 Java 编写的。
4. 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的。
5. Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互。

**JSP**
全称（Java Server Pages）是一种动态网页开发技术。它使用 JSP 标签在 HTML 网页中插入 Java代码。标签通常以<%开头以%>结束。与 JavaScript 相比：虽然 JavaScript 可以在客户端动态生成HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。




**JSP 和 Servlet 的区别**
从网络三层结构的角度看 JSP 和 Servlet 的区别，一个网络项目最少分三层：data layer(数据层)，business layer(业务层)，presentation layer(表现层)。当然也可以更复杂。Servlet 用来写 business layer 是很强大的，但是对于写 presentation layer 就很不方便。JSP 则主要是为了方便写 presentation layer 而设计的。综上所述，Servlet 是一个早期的不完善的产品，写 business layer 很好，写 presentation layer 就很臭，并且两层混杂。
所以，推出JSP+BEAN，用 JSP 写 presentation layer，用 BEAN 写 business layer。SUN 自己的意思也是将来用 JSP 替代 Servlet。这是技术更新方面 JSP 和 Servlet 的区别。
可是，这不是说，学了 Servlet 没用，实际上，你还是应该从 Servlet 入门，再上 JSP，再上 JSP+BEAN。
强调的是：学了JSP，不会用 Java BEAN 并进行整合，等于没学。大家多花点力气在 JSP+BEAN 上。
**常见容器**：Tomcat, Jetty, resin, Oracle Application server, WebLogic Server, Glassfish, Websphere, JBoss 等等。（提供了 Servlet 功能的服务器，叫做 Servlet 容器。对 web 程序来说，Servlet 容器的作用就相当于桌面程序里操作系统的作用，都是提供一些编程基础设施）




**Swing**
* Swing 是一个为Java设计的GUI工具包。包括了图形用户界面（GUI）器件如：文本框，按钮，分隔窗格和表。
**五子棋**：1.掌握 JavaGUI 界面设计、2.掌握鼠标事件的监听（MouseListener，MouseMotionListener）




### Eclipse 教程
[参考 W3C教程](https://www.w3cschool.cn/eclipse/)
**简介**：Eclipse是一款基于Java的开源可扩展开发平台，Eclipse不是一门编程语言，而是一个框架和一组服务。Eclipse为开发者提供了一个标准的插件集，包括Java开发工具（Java Development Kit，JDK）。虽然Eclipse 是使用Java语言开发的，但它的用途并不限于 Java 语言；还提供支持C/C++、COBOL、PHP、Android等编程语言的插件。

```java
//Java Project > Java 包 > .java 文件
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







